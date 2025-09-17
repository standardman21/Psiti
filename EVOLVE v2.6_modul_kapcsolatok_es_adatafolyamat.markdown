# Evolve_Protokoll v2.6: Modul Kapcsolatok és Adatáramlás

## Áttekintés
Ez a dokumentum az **Evolve_Protokoll v2.6** rendszer teljes modulrendszerének kapcsolatait és adatáramlását ábrázolja, az architektúra terv alapján. A három kidolgozott modul (**EDM**, **TVG**, **ComplianceMonitor**) kiemelten szerepel, a fő adatfolyamat (EDM → TVG → MTM → AUI → Vezérlő) és a kulcsfüggőségek (pl. `ethics_engine`, `LLM`, `POL`) ábrázolásával. A rendszer ~100 modult tartalmaz, tematikus modulcsoportok szerint (FunctionalModules, CoreComponents, Infrastructure, Interface, Security, AI_Collaboration, Governance, Strategy, Dependencies, Fenntarthatósági_Modul, Válaszmodulok, További_Modulok). A rendszer Kubernetes-alapú, Prometheus/Grafana monitorozással, ELK naplózással (PII anonimizálás: `[0-9]{4}-[0-9]{4}`), és hibatűréssel (3 retry, rollback `iteration_N-1_continuation.json`).

## Vizuális Ábra
Az alábbi Mermaid diagram a modulok közötti kapcsolatokat és adatáramlást ábrázolja, modulcsoportok szerint strukturálva. A kidolgozott modulok (**EDM**, **TVG**, **ComplianceMonitor**) sárga háttérrel kiemelve, a fő adatfolyamat (EDM → TVG → MTM → AUI → Vezérlő) és a függőségek egyértelműen láthatók. A címkék egyszerűsítettek a Mermaid kompatibilitás érdekében.

```mermaid
graph TD
    %% FunctionalModules
    subgraph FunctionalModules
        EDM[EDM: Súly=0.15, CI=0.95, API=/edm/process] -->|Adatáramlás| TVG
        TVG[TVG: Súly=0.03, CI=0.94, API=/tvg/generate] -->|Adatáramlás| MTM
        MTM[MTM: Súly=0.05, CI=0.94, API=/mtm/monitor] -->|Adatáramlás| AUI
        AUI[AUI: Súly=0.01, CI=0.94, API=/aui/authenticate] -->|Adatáramlás| Vezérlő
        Vezérlő[Vezérlő: Súly=0.10, CI=0.94, API=/controller/orchestrate]
        STM[STM: Súly=0.05, CI=0.94]
        VAM[VAM: Súly=0.05, CI=0.94]
        MEM[MEM: Súly=0.05, CI=0.94]
        AMM[AMM: Súly=0.07, CI=0.94]
        KBM[KBM: Súly=0.05, CI=0.94]
        PDM[PDM: Súly=0.03, CI=0.94]
        UIM[UIM: Súly=0.03, CI=0.94]
        SAM[SAM: Súly=0.03, CI=0.94]
    end

    %% CoreComponents
    subgraph CoreComponents
        EAM[EAM: Súly=0.03, CI=0.94, API=/eam/process]
        ERM[ERM: Súly=0.03, CI=0.94]
        WBM[WBM: Súly=0.03, CI=0.94]
        CSM[CSM: Súly=0.03, CI=0.94]
        CTM[CTM: Súly=0.03, CI=0.94]
        ethics_engine[ethics_engine: Súly=0.05, CI=0.95]
        knowledge_base[knowledge_base: Súly=0.05, CI=0.94]
        benchmarking[benchmarking: Súly=0.03, CI=0.94]
    end

    %% Infrastructure
    subgraph Infrastructure
        DCM[DCM: Súly=0.03, CI=0.94]
        AIM[AIM: Súly=0.03, CI=0.94]
        LPM[LPM: Súly=0.03, CI=0.94]
        MON[MON: Súly=0.03, CI=0.94]
        ORC[ORC: Súly=0.03, CI=0.94]
        adatbázis[adatbázis: Súly=0.03, CI=0.94]
        error_tracking[error_tracking: Súly=0.03, CI=0.94]
        naplózó[naplózó: Súly=0.02, CI=0.94]
        prometheus_integration[prometheus_integration: Súly=0.02, CI=0.94]
        deployment[deployment: Súly=0.02, CI=0.94]
        version_control[version_control: Súly=0.02, CI=0.94]
        LoadBalancer[LoadBalancer: Súly=0.02, CI=0.94]
        Failover[Failover: Súly=0.02, CI=0.94]
        FinalIntegration[FinalIntegration: Súly=0.03, CI=0.95]
    end

    %% Interface
    subgraph Interface
        UIM --> VIS[VIS: Súly=0.03, CI=0.94]
        ADM[ADM: Súly=0.03, CI=0.94]
        internal_data_adapter[internal_data_adapter: Súly=0.02, CI=0.94]
        external_api_adapter[external_api_adapter: Súly=0.02, CI=0.94]
    end

    %% Security
    subgraph Security
        SEC[SEC: Súly=0.03, CI=0.94]
        AUD[AUD: Súly=0.03, CI=0.94]
    end

    %% AI_Collaboration
    subgraph AI_Collaboration
        HIB[HIB: Súly=0.03, CI=0.94]
        LLM[LLM: Súly=0.03, CI=0.94]
        TST[TST: Súly=0.03, CI=0.94]
    end

    %% Governance
    subgraph Governance
        POL[POL: Súly=0.03, CI=0.94]
        LEG[LEG: Súly=0.03, CI=0.94]
        SOC[SOC: Súly=0.03, CI=0.94]
    end

    %% Strategy
    subgraph Strategy
        RDM[RDM: Súly=0.03, CI=0.94]
        EVM[EVM: Súly=0.03, CI=0.94]
        IMP[IMP: Súly=0.03, CI=0.94]
    end

    %% Dependencies
    subgraph Dependencies
        DependencyMapping[DependencyMapping: Súly=0.03, CI=0.94]
        WeightTable[WeightTable: Súly=0.03, CI=0.94]
        ConvergenceMatrix[ConvergenceMatrix: Súly=0.03, CI=0.94]
        dependency_map[dependency_map: Súly=0.03, CI=0.94]
        workflow_schemas[workflow_schemas: Súly=0.03, CI=0.94]
    end

    %% Fenntarthatósági_Modul
    subgraph Fenntarthatósági_Modul
        SEM[SEM: Súly=0.03, CI=0.94]
        CEM[CEM: Súly=0.03, CI=0.94]
    end

    %% Válaszmodulok
    subgraph Válaszmodulok
        TVG --> FVH[FVH: Súly=0.03, CI=0.94]
        INT[Integrációs Réteg: Súly=0.03, CI=0.94]
    end

    %% További_Modulok
    subgraph További_Modulok
        ComplianceMonitor[ComplianceMonitor: Súly=0.02, CI=0.94, API=/cm/validate]
        IEK[IEK: Súly=0.02, CI=0.94]
        ELM[ELM: Súly=0.02, CI=0.94]
        ETM[ETM: Súly=0.02, CI=0.94]
        DFM[DFM: Súly=0.02, CI=0.94]
        FEM[FEM: Súly=0.02, CI=0.94]
        SET[SET: Súly=0.02, CI=0.94]
        VPM2[VPM2: Súly=0.02, CI=0.94]
        JMA[JMA: Súly=0.02, CI=0.94]
        JFA[JFA: Súly=0.02, CI=0.94]
        FPA[FPA: Súly=0.02, CI=0.94]
        FOA[FOA: Súly=0.02, CI=0.94]
        KTA[KTA: Súly=0.02, CI=0.94]
        SystemHealth[SystemHealth: Súly=0.02, CI=0.94]
        Alerting[Alerting: Súly=0.02, CI=0.94]
        PerformanceTuning[PerformanceTuning: Súly=0.02, CI=0.94]
        AuditTrail[AuditTrail: Súly=0.02, CI=0.94]
    end

    %% Key Dependencies
    EDM --> ethics_engine
    EDM --> dependency_map
    TVG --> LLM
    TVG --> HIB
    MTM --> MON
    MTM --> ORC
    AUI --> SEC
    AUI --> UIM
    Vezérlő --> ORC
    Vezérlő --> dependency_map
    STM --> ORC
    STM --> adatbázis
    VAM --> DCM
    VAM --> AIM
    MEM --> MON
    MEM --> SAM
    AMM --> AIM
    AMM --> SEC
    AMM --> HIB
    KBM --> knowledge_base
    KBM --> adatbázis
    PDM --> POL
    PDM --> LEG
    UIM --> VIS
    UIM --> HIB
    SAM --> SEC
    SAM --> AUD
    EAM --> adatbázis
    EAM --> LPM
    ERM --> ethics_engine
    ERM --> POL
    WBM --> ORC
    WBM --> workflow_schemas
    CSM --> knowledge_base
    CSM --> HIB
    CTM --> POL
    CTM --> LEG
    ethics_engine --> knowledge_base
    ethics_engine --> EDM
    knowledge_base --> adatbázis
    knowledge_base --> ELK[ELK]
    benchmarking --> Prometheus[Prometheus]
    benchmarking --> Locust[Locust]
    DCM --> adatbázis
    DCM --> SEC
    AIM --> LLM
    AIM --> HIB
    LPM --> ELK
    LPM --> adatbázis
    MON --> Prometheus
    MON --> Alerting
    ORC --> Vezérlő
    ORC --> workflow_schemas
    adatbázis --> PostgreSQL[PostgreSQL]
    adatbázis --> Flyway[Flyway]
    error_tracking --> Sentry[Sentry]
    error_tracking --> ELK
    naplózó --> ELK
    naplózó --> LPM
    prometheus_integration --> Prometheus
    prometheus_integration --> Grafana[Grafana]
    deployment --> Kubernetes[Kubernetes]
    deployment --> Istio[Istio]
    version_control --> Git[Git]
    version_control --> GitLab[GitLab]
    LoadBalancer --> Istio
    LoadBalancer --> Kubernetes
    Failover --> Kubernetes
    Failover --> Istio
    FinalIntegration --> ORC
    FinalIntegration --> dependency_map
    ADM --> SEC
    ADM --> UIM
    VIS --> Prometheus
    VIS --> Grafana
    internal_data_adapter --> DCM
    internal_data_adapter --> adatbázis
    external_api_adapter --> SEC
    external_api_adapter --> Kong[Kong]
    SEC --> AUD
    SEC --> ELK
    AUD --> ELK
    AUD --> Sentry
    HIB --> UIM
    HIB --> LLM
    LLM --> AIM
    LLM --> knowledge_base
    TST --> Locust
    TST --> Prometheus
    POL --> CTM
    POL --> LEG
    LEG --> CTM
    LEG --> ComplianceMonitor
    SOC --> MON
    SOC --> CTM
    RDM --> POL
    RDM --> ComplianceMonitor
    EVM --> EAM
    EVM --> ORC
    IMP --> ORC
    IMP --> Vezérlő
    DependencyMapping --> dependency_map
    DependencyMapping --> WeightTable
    WeightTable --> ConvergenceMatrix
    ConvergenceMatrix --> WeightTable
    ConvergenceMatrix --> dependency_map
    dependency_map --> DependencyMapping
    workflow_schemas --> ORC
    workflow_schemas --> WBM
    SEM --> Prometheus
    SEM --> CloudCarbon[CloudCarbon]
    CEM --> CTM
    CEM --> POL
    FVH --> UIM
    FVH --> HIB
    INT --> ORC
    INT --> dependency_map
    IEK --> knowledge_base
    IEK --> AIM
    ELM --> ELK
    ELM --> LPM
    ETM --> EAM
    ETM --> MON
    DFM --> DCM
    DFM --> ORC
    FEM --> UIM
    FEM --> HIB
    SET --> TST
    SET --> MON
    VPM2 --> version_control
    VPM2 --> VPM[VPM]
    JMA --> ORC
    JMA --> MTM
    JFA --> error_tracking
    JFA --> Sentry
    FPA --> FEM
    FPA --> HIB
    FOA --> FEM
    FOA --> FPA
    KTA --> knowledge_base
    KTA --> KBM
    SystemHealth --> MON
    SystemHealth --> Prometheus
    Alerting --> MON
    Alerting --> Prometheus
    PerformanceTuning --> Prometheus
    PerformanceTuning --> Locust
    AuditTrail --> AUD
    AuditTrail --> ELK
    ComplianceMonitor --> CTM
    ComplianceMonitor --> POL
    ComplianceMonitor -->|Megfelelőségi ellenőrzés| EDM
    ComplianceMonitor -->|Megfelelőségi ellenőrzés| TVG

    %% Styling
    classDef module fill:#f9f,stroke:#333,stroke-width:2px;
    classDef dependency fill:#bbf,stroke:#333,stroke-width:2px;
    class EDM,TVG,ComplianceMonitor module fill:#ff9,stroke:#333,stroke-width:3px;
    class MTM,AUI,Vezérlő,STM,VAM,MEM,AMM,KBM,PDM,UIM,SAM,EAM,ERM,WBM,CSM,CTM,ethics_engine,knowledge_base,benchmarking,DCM,AIM,LPM,MON,ORC,adatbázis,error_tracking,naplózó,prometheus_integration,deployment,version_control,LoadBalancer,Failover,FinalIntegration,ADM,VIS,internal_data_adapter,external_api_adapter,SEC,AUD,HIB,LLM,TST,POL,LEG,SOC,RDM,EVM,IMP,DependencyMapping,WeightTable,ConvergenceMatrix,dependency_map,workflow_schemas,SEM,CEM,FVH,INT,IEK,ELM,ETM,DFM,FEM,SET,VPM2,JMA,JFA,FPA,FOA,KTA,SystemHealth,Alerting,PerformanceTuning,AuditTrail module;
    class ELK,Prometheus,Grafana,PostgreSQL,Flyway,Sentry,Kubernetes,Istio,Git,GitLab,Kong,CloudCarbon,Locust,VPM dependency;
```

## Adatfolyamat Leírása
Az **Evolve_Protokoll v2.6** rendszer adatfolyamata a modulcsoportok közötti kapcsolatokon alapul:
1. **FunctionalModules**: Az adatfeldolgozás az **EDM**-ben kezdődik (események validálása, `ethics_engine`, PostgreSQL: `event_data` tábla), majd a **TVG** válaszokat generál (`LLM`, `HIB`, PostgreSQL: `response_data` tábla). Az **MTM** monitorozza a feladatokat (`MON`, `ORC`, PostgreSQL: `task_data` tábla), az **AUI** hitelesít (`SEC`, PostgreSQL: `auth_records` tábla), és a **Vezérlő** orchestrálja a rendszert (`ORC`, PostgreSQL: `orchestration_data` tábla). Egyéb modulok (pl. **STM**, **VAM**, **MEM**, **AMM**, **KBM**, **PDM**, **UIM**, **SAM**) kiegészítő funkciókat biztosítanak.
2. **CoreComponents**: Az **EAM** eseményeket elemez, az **ERM** etikai szabályokat kezel, a **WBM** munkafolyamatokat orchestrál, a **CSM** kontextust szintetizál, a **CTM** megfelelőségi nyomon követést végez, az **ethics_engine** etikai validációt biztosít, a **knowledge_base** tudásgráfot kezel, és a **benchmarking** teljesítményteszteket futtat.
3. **Infrastructure**: Az **adatbázis** (PostgreSQL, Flyway), **naplózó** (ELK), **prometheus_integration** (Prometheus/Grafana), **deployment** (Kubernetes, Istio), **version_control** (Git, GitLab), **LoadBalancer**, **Failover**, és **FinalIntegration** támogatja az adatkezelést, monitorozást, és telepítést.
4. **Interface**: A **UIM** és **VIS** felhasználói interakciót és vizualizációt biztosít, az **ADM** adminisztrációs vezérlést, az **internal_data_adapter** és **external_api_adapter** adattranszformációt és külső API integrációt.
5. **Security**: A **SEC** hitelesítést és titkosítást, az **AUD** auditnaplózást végez.
6. **AI_Collaboration**: A **HIB** ember-AI interakciót, az **LLM** szöveggenerálást, a **TST** teszteket biztosít.
7. **Governance**: A **POL** szabályokat definiál, a **LEG** jogi megfelelőséget ellenőriz, a **SOC** rendszervezérlést biztosít.
8. **Strategy**: Az **RDM** kockázatértékelést, az **EVM** eseménykezelést, az **IMP** végrehajtást végez.
9. **Dependencies**: A **DependencyMapping**, **WeightTable**, **ConvergenceMatrix**, **dependency_map**, és **workflow_schemas** függőségeket és stabilitást kezel.
10. **Fenntarthatósági_Modul**: Az **SEM** energiafelhasználást, a **CEM** megfelelőségi szabályokat érvényesít.
11. **Válaszmodulok**: A **FVH** visszajelzéseket kezel, az **INT** modulokat kapcsol össze.
12. **További_Modulok**: A **ComplianceMonitor** megfelelőségi ellenőrzéseket végez (pl. EDM/TVG kimeneteire, `CTM`, `POL`, PostgreSQL: `compliance_records` tábla), API: `/cm/validate`. További modulok (**IEK**, **ELM**, **ETM**, **DFM**, **FEM**, **SET**, **VPM2**, **JMA**, **JFA**, **FPA**, **FOA**, **KTA**, **SystemHealth**, **Alerting**, **PerformanceTuning**, **AuditTrail**) kiegészítő funkciókat biztosítanak.

### Kulcsmodulok Adatfolyamata
- **EDM**: Események validálása, API: `/edm/process`.
- **TVG**: Válaszgenerálás, API: `/tvg/generate`.
- **MTM**: Feladatmonitorozás, API: `/mtm/monitor`.
- **AUI**: Hitelesítés, API: `/aui/authenticate`.
- **Vezérlő**: Rendszerorchestráció, API: `/controller/orchestrate`.
- **ComplianceMonitor**: Megfelelőségi ellenőrzés (indirekt kapcsolat az EDM/TVG-vel), API: `/cm/validate`.

## Technikai Részletek
- **Infrastruktúra**: Kubernetes (HPA: targetCPU 80%), Istio (canary deployment: 10% forgalom, rollback 5 perc).
- **Monitorozás**: Prometheus/Grafana (`latency_ms < 100`, `throughput_tps > 50`, `error_rate_percent < 1`, `convergence_index > 0.9`), ELK naplózás (PII anonimizálás: `[0-9]{4}-[0-9]{4}`).
- **Biztonság**: OAuth2, TLS1.3, OWASP ZAP, Kong API gateway (rate_limit: 100/min).
- **Hibakezelés**: 3 retry, rollback `iteration_N-1_continuation.json`.
- **Fenntarthatóság**: CI/CD energiafelhasználás és CO₂-lábnyom monitorozása (Cloud Carbon Footprint API).
- **Súlyok és Konvergencia**: Globális súlyösszeg: \( \sum w_i = 1.0 \), globális CI: \( 0.9 \), számítás: \( CI = \left( \frac{\sum \text{submodule_weights}}{\text{parent_weight}} \right) \times \text{dependency_coverage_ratio} \approx 0.9 \).

## Megjegyzések a Fejlesztőknek
- **Használat**: Használjátok a diagramot a `dependency_map.json` és a kiegészítő dokumentációval (`evolve_protokoll_v2.6_supplementary_documentation_final.md`) együtt az adatfolyamat implementálásához. A **ComplianceMonitor** megfelelőségi ellenőrzéseket végez az EDM/TVG kimeneteire, nem része a fő adatfolyamatnak (EDM → TVG → MTM → AUI → Vezérlő).
- **Validáció**: Ellenőrizzétek a függőségeket az `update_dag` szkripttel, biztosítva a ciklikus függőségek hiányát.
- **Monitorozás**: Integráljátok a Prometheus metrikákat (`convergence_index > 0.9`) és ELK naplózást minden modulhoz a `module_integration.yaml` alapján.
- **Tesztelés**: Futtassatok Locust stresszteszteket (-u 1000 -r 100) minden 3. iterációban, és használjátok a dummy adatokat a pipeline validálására.
- **Dokumentáció**: Archiváljatok minden fájlt a Git repóba, és frissítsétek a `dependency_map.json`-t minden iterációban.
- **Mermaid Renderelés**: A diagram kompatibilis a Mermaid Live Editorral (https://mermaid.live/). PNG/SVG kimenethez használjátok a Mermaid CLI-t: `mmdc -i input.mmd -o output.svg`.
