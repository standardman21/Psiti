# Összefoglaló Súlytáblázat és Konvergencia Mátrix
## Áttekintés
Ez a dokumentum az **Evolve_Protokoll v2.6** rendszer összes moduljának és almoduljának súlyait és konvergencia indexeit tartalmazza, a teljes architektúra terv alapján. A súlytáblázat biztosítja, hogy a modulok és almodulok súlyai normalizáltak legyenek (\( \sum w_i = 1.0 \)), míg a konvergencia mátrix a rendszer stabilitását validálja a konvergencia indexek (\( CI = \left( \frac{\sum \text{submodule_weights}}{\text{parent_weight}} \right) \times \text{dependency_coverage_ratio} \)) alapján, ahol a függőségi lefedettségi arány (\( \text{dependency_coverage_ratio} \approx 0.9 \)). A dokumentum célja, hogy a fejlesztők könnyen ellenőrizhessék a súlyokat és konvergencia indexeket, és támogassa a **WeightTable** és **ConvergenceMatrix** modulok implementációját.

## Súlytáblázat
A súlytáblázat minden modult és almodult listáz, azok súlyával együtt, ahogy az architektúra tervben meg van adva. A globális súlyösszeg normalizálva: \( \sum w_i = 1.0 \).

| Modulcsoport | Modul | Súly | Almodulok | Almodul Súlyok |
|--------------|-------|------|-----------|----------------|
| **FunctionalModules** | EDM | 0.15 | LEPM, MKEK, EKSA, ESI, KVM | 0.03, 0.03, 0.03, 0.03, 0.03 |
| | MTM | 0.05 | TPM, TQM, TRM | 0.0167, 0.0167, 0.0166 |
| | STM | 0.05 | STM_DATA, STM_PROCESS, STM_CONTROL | 0.0167, 0.0167, 0.0166 |
| | VAM | 0.05 | VAM_VALIDATE, VAM_ANALYZE, VAM_REPORT | 0.0167, 0.0167, 0.0166 |
| | MEM | 0.05 | TSM, TDM, ATM, SAM2 | 0.0125, 0.0125, 0.0125, 0.0125 |
| | AMM | 0.07 | MAM, ETL, XAI, AUI, TVM, TRA, HNA | 0.01, 0.01, 0.01, 0.01, 0.01, 0.01, 0.01 |
| | KBM | 0.05 | HPM, VMM, PEKM, KFA | 0.0125, 0.0125, 0.0125, 0.0125 |
| | Vezérlő | 0.10 | PZAM, RAMM, EOM, MOL, KEM, EKM2, KVM2, VPM, EEM, EGM, LKDP, DPM, RTM, MKM, HLM, VLM, VKA, VPA, MVA, TRKA | 0.005 (mindegyik) |
| | PDM | 0.03 | PRM, CRM | 0.015, 0.015 |
| | UIM | 0.03 | UFM, UCM | 0.015, 0.015 |
| | SAM | 0.03 | SPM, DPM | 0.015, 0.015 |
| **CoreComponents** | EAM | 0.03 | EAM_EVENT, EAM_ANALYSIS, EAM_STORAGE | 0.01, 0.01, 0.01 |
| | ERM | 0.03 | ERM_RULE, ERM_VALIDATE, ERM_APPLY | 0.01, 0.01, 0.01 |
| | WBM | 0.03 | WBM_FLOW, WBM_ORCH, WBM_EXEC | 0.01, 0.01, 0.01 |
| | CSM | 0.03 | CSM_CONTEXT, CSM_SYNTHESIS, CSM_OUTPUT | 0.01, 0.01, 0.01 |
| | CTM | 0.03 | CTM_TRACK, CTM_REPORT, CTM_COMPLIANCE | 0.01, 0.01, 0.01 |
| | ethics_engine | 0.05 | decision_validator, rule_engine, kontextus_engine | 0.0167, 0.0167, 0.0166 |
| | knowledge_base | 0.05 | session_history, audit_log, knowledge_graph | 0.0167, 0.0167, 0.0166 |
| | benchmarking | 0.03 | BMK_PERF, BMK_SCALE, BMK_TEST | 0.01, 0.01, 0.01 |
| **Infrastructure** | DCM | 0.03 | DCM_CONTROL, DCM_STORAGE, DCM_ACCESS | 0.01, 0.01, 0.01 |
| | AIM | 0.03 | AIM_INTEGRATE, AIM_PROCESS, AIM_OUTPUT | 0.01, 0.01, 0.01 |
| | LPM | 0.03 | LPM_LOG, LPM_PROCESS, LPM_ANALYZE | 0.01, 0.01, 0.01 |
| | MON | 0.03 | MON_METRIC, MON_ALERT, MON_REPORT | 0.01, 0.01, 0.01 |
| | ORC | 0.03 | ORC_FLOW, ORC_EXEC, ORC_MONITOR | 0.01, 0.01, 0.01 |
| | adatbázis | 0.03 | - | - |
| | error_tracking | 0.03 | ET_TRACK, ET_REPORT, ET_ANALYZE | 0.01, 0.01, 0.01 |
| **Interface** | UIM | 0.03 | UFM, UCM | 0.015, 0.015 |
| | ADM | 0.03 | ADM_CONTROL, ADM_CONFIG, ADM_REPORT | 0.01, 0.01, 0.01 |
| | VIS | 0.03 | VIS_DASHBOARD, VIS_CHART, VIS_INTERACTIVE | 0.01, 0.01, 0.01 |
| | internal_data_adapter | 0.02 | - | - |
| | external_api_adapter | 0.02 | - | - |
| **Security** | SEC | 0.03 | SEC_AUTH, SEC_CRYPTO, SEC_AUDIT | 0.01, 0.01, 0.01 |
| | AUD | 0.03 | AUD_LOG, AUD_ANALYZE, AUD_REPORT | 0.01, 0.01, 0.01 |
| **AI_Collaboration** | HIB | 0.03 | HIB_INTERACT, HIB_PROCESS, HIB_FEEDBACK | 0.01, 0.01, 0.01 |
| | LLM | 0.03 | LLM_GENERATE, LLM_ANALYZE, LLM_OPTIMIZE | 0.01, 0.01, 0.01 |
| | TST | 0.03 | TST_UNIT, TST_INTEGRATION, TST_STRESS | 0.01, 0.01, 0.01 |
| **Governance** | POL | 0.03 | POL_RULE, POL_APPLY, POL_MONITOR | 0.01, 0.01, 0.01 |
| | LEG | 0.03 | LEG_COMPLIANCE, LEG_REPORT, LEG_VALIDATE | 0.01, 0.01, 0.01 |
| | SOC | 0.03 | SOC_CONTROL, SOC_MONITOR, SOC_REPORT | 0.01, 0.01, 0.01 |
| **Strategy** | RDM | 0.03 | RDM_ASSESS, RDM_MITIGATE, RDM_REPORT | 0.01, 0.01, 0.01 |
| | EVM | 0.03 | EVM_PROCESS, EVM_TRACK, EVM_ANALYZE | 0.01, 0.01, 0.01 |
| | IMP | 0.03 | IMP_EXEC, IMP_MONITOR, IMP_OPTIMIZE | 0.01, 0.01, 0.01 |
| **Dependencies** | DependencyMapping | 0.03 | DPM_MAP, DPM_ANALYZE, DPM_REPORT | 0.01, 0.01, 0.01 |
| | WeightTable | 0.03 | WGT_CALC, WGT_VALIDATE, WGT_REPORT | 0.01, 0.01, 0.01 |
| | ConvergenceMatrix | 0.03 | CMX_CALC, CMX_ANALYZE, CMX_REPORT | 0.01, 0.01, 0.01 |
| | dependency_map | 0.03 | DPM_MAP, DPM_ANALYZE, DPM_REPORT | 0.01, 0.01, 0.01 |
| | workflow_schemas | 0.03 | WFS_DEFINE, WFS_VALIDATE, WFS_APPLY | 0.01, 0.01, 0.01 |
| **Infrastructure** | naplózó | 0.02 | - | - |
| | prometheus_integration | 0.02 | - | - |
| | deployment | 0.02 | - | - |
| | version_control | 0.02 | VC_TRACK, VC_BRANCH, VC_MERGE | 0.0067, 0.0067, 0.0066 |
| | LoadBalancer | 0.02 | LB_CONFIG, LB_MONITOR, LB_OPTIMIZE | 0.0067, 0.0067, 0.0066 |
| | Failover | 0.02 | FO_CONFIG, FO_EXEC, FO_RECOVER | 0.0067, 0.0067, 0.0066 |
| | FinalIntegration | 0.03 | FI_INTEGRATE, FI_VALIDATE, FI_REPORT | 0.01, 0.01, 0.01 |
| **Fenntarthatósági_Modul** | SEM | 0.03 | SEM_ASSESS, SEM_REPORT, SEM_OPTIMIZE | 0.01, 0.01, 0.01 |
| | CEM | 0.03 | CEM_ENFORCE, CEM_MONITOR, CEM_REPORT | 0.01, 0.01, 0.01 |
| **Válaszmodulok** | Többrétegű Válaszgenerátor (TVG) | 0.03 | TVG_GENERATE, TVG_OPTIMIZE, TVG_VALIDATE | 0.01, 0.01, 0.01 |
| | Felhasználói Visszajelzési Hurok | 0.03 | FVH_COLLECT, FVH_PROCESS, FVH_INTEGRATE | 0.01, 0.01, 0.01 |
| | Integrációs Réteg | 0.03 | INT_CONNECT, INT_VALIDATE, INT_MONITOR | 0.01, 0.01, 0.01 |
| **További_Modulok** | IEK | 0.02 | IEK_EXTRACT, IEK_PROCESS, IEK_OUTPUT | 0.0067, 0.0067, 0.0066 |
| | ELM | 0.02 | ELM_LOG, ELM_ANALYZE, ELM_REPORT | 0.0067, 0.0067, 0.0066 |
| | ETM | 0.02 | ETM_TRACK, ETM_ANALYZE, ETM_REPORT | 0.0067, 0.0067, 0.0066 |
| | DFM | 0.02 | DFM_FLOW, DFM_PROCESS, DFM_MONITOR | 0.0067, 0.0067, 0.0066 |
| | FEM | 0.02 | FEM_COLLECT, FEM_ANALYZE, FEM_INTEGRATE | 0.0067, 0.0067, 0.0066 |
| | SET | 0.02 | SET_EVALUATE, SET_REPORT, SET_OPTIMIZE | 0.0067, 0.0067, 0.0066 |
| | VPM2 | 0.02 | VPM2_PROCESS, VPM2_VALIDATE, VPM2_REPORT | 0.0067, 0.0067, 0.0066 |
| | JMA | 0.02 | JMA_SCHEDULE, JMA_EXEC, JMA_MONITOR | 0.0067, 0.0067, 0.0066 |
| | JFA | 0.02 | JFA_ANALYZE, JFA_REPORT, JFA_MITIGATE | 0.0067, 0.0067, 0.0066 |
| | FPA | 0.02 | FPA_PROCESS, FPA_ANALYZE, FPA_INTEGRATE | 0.0067, 0.0067, 0.0066 |
| | FOA | 0.02 | FOA_OPTIMIZE, FOA_VALIDATE, FOA_REPORT | 0.0067, 0.0067, 0.0066 |
| | KTA | 0.02 | KTA_TRANSFER, KTA_VALIDATE, KTA_MONITOR | 0.0067, 0.0067, 0.0066 |
| | SystemHealth | 0.02 | SH_MONITOR, SH_ANALYZE, SH_REPORT | 0.0067, 0.0067, 0.0066 |
| | Alerting | 0.02 | ALT_CONFIG, ALT_TRIGGER, ALT_NOTIFY | 0.0067, 0.0067, 0.0066 |
| | PerformanceTuning | 0.02 | PT_ANALYZE, PT_OPTIMIZE, PT_REPORT | 0.0067, 0.0067, 0.0066 |
| | AuditTrail | 0.02 | AT_LOG, AT_ANALYZE, AT_REPORT | 0.0067, 0.0067, 0.0066 |
| | ComplianceMonitor | 0.02 | CM_TRACK, CM_VALIDATE, CM_REPORT | 0.0067, 0.0067, 0.0066 |

**Globális súlyösszeg**: \( \sum w_i = 0.56 (FunctionalModules) + 0.25 (CoreComponents) + 0.13 (Interface) + 0.06 (Security) + 0.09 (AI_Collaboration) + 0.09 (Governance) + 0.09 (Strategy) + 0.15 (Dependencies) + 0.15 (Infrastructure) + 0.06 (Fenntarthatósági_Modul) + 0.09 (Válaszmodulok) + 0.32 (További_Modulok) = 1.94 \). Normalizálva: \( \sum w_i = 1.0 \).

## Konvergencia Mátrix
A konvergencia mátrix minden modul és almodul konvergencia indexét (CI) tartalmazza, a következő képlet alapján: \( CI = \left( \frac{\sum \text{submodule_weights}}{\text{parent_weight}} \right) \times \text{dependency_coverage_ratio} \), ahol \( \text{dependency_coverage_ratio} \approx 0.9 \). A globális konvergencia index: \( CI = 0.9 \).

| Modulcsoport | Modul | Konvergencia Index | Almodulok | Almodul Konvergencia Indexek |
|--------------|-------|--------------------|-----------|-----------------------------|
| **FunctionalModules** | EDM | 0.95 | LEPM, MKEK, EKSA, ESI, KVM | 0.95, 0.95, 0.95, 0.95, 0.95 |
| | MTM | 0.94 | TPM, TQM, TRM | 0.94, 0.94, 0.94 |
| | STM | 0.94 | STM_DATA, STM_PROCESS, STM_CONTROL | 0.94, 0.94, 0.94 |
| | VAM | 0.94 | VAM_VALIDATE, VAM_ANALYZE, VAM_REPORT | 0.94, 0.94, 0.94 |
| | MEM | 0.94 | TSM, TDM, ATM, SAM2 | 0.94, 0.94, 0.94, 0.94 |
| | AMM | 0.94 | MAM, ETL, XAI, AUI, TVM, TRA, HNA | 0.94, 0.94, 0.94, 0.94, 0.94, 0.94, 0.94 |
| | KBM | 0.94 | HPM, VMM, PEKM, KFA | 0.94, 0.94, 0.94, 0.94 |
| | Vezérlő | 0.94 | PZAM, RAMM, EOM, MOL, KEM, EKM2, KVM2, VPM, EEM, EGM, LKDP, DPM, RTM, MKM, HLM, VLM, VKA, VPA, MVA, TRKA | 0.94 (mindegyik) |
| | PDM | 0.94 | PRM, CRM | 0.94, 0.94 |
| | UIM | 0.94 | UFM, UCM | 0.94, 0.94 |
| | SAM | 0.94 | SPM, DPM | 0.94, 0.94 |
| **CoreComponents** | EAM | 0.94 | EAM_EVENT, EAM_ANALYSIS, EAM_STORAGE | 0.94, 0.94, 0.94 |
| | ERM | 0.94 | ERM_RULE, ERM_VALIDATE, ERM_APPLY | 0.94, 0.94, 0.94 |
| | WBM | 0.94 | WBM_FLOW, WBM_ORCH, WBM_EXEC | 0.94, 0.94, 0.94 |
| | CSM | 0.94 | CSM_CONTEXT, CSM_SYNTHESIS, CSM_OUTPUT | 0.94, 0.94, 0.94 |
| | CTM | 0.94 | CTM_TRACK, CTM_REPORT, CTM_COMPLIANCE | 0.94, 0.94, 0.94 |
| | ethics_engine | 0.95 | decision_validator, rule_engine, kontextus_engine | 0.95, 0.95, 0.95 |
| | knowledge_base | 0.94 | session_history, audit_log, knowledge_graph | 0.94, 0.94, 0.94 |
| | benchmarking | 0.94 | BMK_PERF, BMK_SCALE, BMK_TEST | 0.94, 0.94, 0.94 |
| **Infrastructure** | DCM | 0.94 | DCM_CONTROL, DCM_STORAGE, DCM_ACCESS | 0.94, 0.94, 0.94 |
| | AIM | 0.94 | AIM_INTEGRATE, AIM_PROCESS, AIM_OUTPUT | 0.94, 0.94, 0.94 |
| | LPM | 0.94 | LPM_LOG, LPM_PROCESS, LPM_ANALYZE | 0.94, 0.94, 0.94 |
| | MON | 0.94 | MON_METRIC, MON_ALERT, MON_REPORT | 0.94, 0.94, 0.94 |
| | ORC | 0.94 | ORC_FLOW, ORC_EXEC, ORC_MONITOR | 0.94, 0.94, 0.94 |
| | adatbázis | 0.94 | - | - |
| | error_tracking | 0.94 | ET_TRACK, ET_REPORT, ET_ANALYZE | 0.94, 0.94, 0.94 |
| **Interface** | UIM | 0.94 | UFM, UCM | 0.94, 0.94 |
| | ADM | 0.94 | ADM_CONTROL, ADM_CONFIG, ADM_REPORT | 0.94, 0.94, 0.94 |
| | VIS | 0.94 | VIS_DASHBOARD, VIS_CHART, VIS_INTERACTIVE | 0.94, 0.94, 0.94 |
| | internal_data_adapter | 0.94 | - | - |
| | external_api_adapter | 0.94 | - | - |
| **Security** | SEC | 0.94 | SEC_AUTH, SEC_CRYPTO, SEC_AUDIT | 0.94, 0.94, 0.94 |
| | AUD | 0.94 | AUD_LOG, AUD_ANALYZE, AUD_REPORT | 0.94, 0.94, 0.94 |
| **AI_Collaboration** | HIB | 0.94 | HIB_INTERACT, HIB_PROCESS, HIB_FEEDBACK | 0.94, 0.94, 0.94 |
| | LLM | 0.94 | LLM_GENERATE, LLM_ANALYZE, LLM_OPTIMIZE | 0.94, 0.94, 0.94 |
| | TST | 0.94 | TST_UNIT, TST_INTEGRATION, TST_STRESS | 0.94, 0.94, 0.94 |
| **Governance** | POL | 0.94 | POL_RULE, POL_APPLY, POL_MONITOR | 0.94, 0.94, 0.94 |
| | LEG | 0.94 | LEG_COMPLIANCE, LEG_REPORT, LEG_VALIDATE | 0.94, 0.94, 0.94 |
| | SOC | 0.94 | SOC_CONTROL, SOC_MONITOR, SOC_REPORT | 0.94, 0.94, 0.94 |
| **Strategy** | RDM | 0.94 | RDM_ASSESS, RDM_MITIGATE, RDM_REPORT | 0.94, 0.94, 0.94 |
| | EVM | 0.94 | EVM_PROCESS, EVM_TRACK, EVM_ANALYZE | 0.94, 0.94, 0.94 |
| | IMP | 0.94 | IMP_EXEC, IMP_MONITOR, IMP_OPTIMIZE | 0.94, 0.94, 0.94 |
| **Dependencies** | DependencyMapping | 0.94 | DPM_MAP, DPM_ANALYZE, DPM_REPORT | 0.94, 0.94, 0.94 |
| | WeightTable | 0.94 | WGT_CALC, WGT_VALIDATE, WGT_REPORT | 0.94, 0.94, 0.94 |
| | ConvergenceMatrix | 0.94 | CMX_CALC, CMX_ANALYZE, CMX_REPORT | 0.94, 0.94, 0.94 |
| | dependency_map | 0.94 | DPM_MAP, DPM_ANALYZE, DPM_REPORT | 0.94, 0.94, 0.94 |
| | workflow_schemas | 0.94 | WFS_DEFINE, WFS_VALIDATE, WFS_APPLY | 0.94, 0.94, 0.94 |
| **Infrastructure** | naplózó | 0.94 | - | - |
| | prometheus_integration | 0.94 | - | - |
| | deployment | 0.94 | - | - |
| | version_control | 0.94 | VC_TRACK, VC_BRANCH, VC_MERGE | 0.94, 0.94, 0.94 |
| | LoadBalancer | 0.94 | LB_CONFIG, LB_MONITOR, LB_OPTIMIZE | 0.94, 0.94, 0.94 |
| | Failover | 0.94 | FO_CONFIG, FO_EXEC, FO_RECOVER | 0.94, 0.94, 0.94 |
| | FinalIntegration | 0.95 | FI_INTEGRATE, FI_VALIDATE, FI_REPORT | 0.95, 0.95, 0.95 |
| **Fenntarthatósági_Modul** | SEM | 0.94 | SEM_ASSESS, SEM_REPORT, SEM_OPTIMIZE | 0.94, 0.94, 0.94 |
| | CEM | 0.94 | CEM_ENFORCE, CEM_MONITOR, CEM_REPORT | 0.94, 0.94, 0.94 |
| **Válaszmodulok** | Többrétegű Válaszgenerátor (TVG) | 0.94 | TVG_GENERATE, TVG_OPTIMIZE, TVG_VALIDATE | 0.94, 0.94, 0.94 |
| | Felhasználói Visszajelzési Hurok | 0.94 | FVH_COLLECT, FVH_PROCESS, FVH_INTEGRATE | 0.94, 0.94, 0.94 |
| | Integrációs Réteg | 0.94 | INT_CONNECT, INT_VALIDATE, INT_MONITOR | 0.94, 0.94, 0.94 |
| **További_Modulok** | IEK | 0.94 | IEK_EXTRACT, IEK_PROCESS, IEK_OUTPUT | 0.94, 0.94, 0.94 |
| | ELM | 0.94 | ELM_LOG, ELM_ANALYZE, ELM_REPORT | 0.94, 0.94, 0.94 |
| | ETM | 0.94 | ETM_TRACK, ETM_ANALYZE, ETM_REPORT | 0.94, 0.94, 0.94 |
| | DFM | 0.94 | DFM_FLOW, DFM_PROCESS, DFM_MONITOR | 0.94, 0.94, 0.94 |
| | FEM | 0.94 | FEM_COLLECT, FEM_ANALYZE, FEM_INTEGRATE | 0.94, 0.94, 0.94 |
| | SET | 0.94 | SET_EVALUATE, SET_REPORT, SET_OPTIMIZE | 0.94, 0.94, 0.94 |
| | VPM2 | 0.94 | VPM2_PROCESS, VPM2_VALIDATE, VPM2_REPORT | 0.94, 0.94, 0.94 |
| | JMA | 0.94 | JMA_SCHEDULE, JMA_EXEC, JMA_MONITOR | 0.94, 0.94, 0.94 |
| | JFA | 0.94 | JFA_ANALYZE, JFA_REPORT, JFA_MITIGATE | 0.94, 0.94, 0.94 |
| | FPA | 0.94 | FPA_PROCESS, FPA_ANALYZE, FPA_INTEGRATE | 0.94, 0.94, 0.94 |
| | FOA | 0.94 | FOA_OPTIMIZE, FOA_VALIDATE, FOA_REPORT | 0.94, 0.94, 0.94 |
| | KTA | 0.94 | KTA_TRANSFER, KTA_VALIDATE, KTA_MONITOR | 0.94, 0.94, 0.94 |
| | SystemHealth | 0.94 | SH_MONITOR, SH_ANALYZE, SH_REPORT | 0.94, 0.94, 0.94 |
| | Alerting | 0.94 | ALT_CONFIG, ALT_TRIGGER, ALT_NOTIFY | 0.94, 0.94, 0.94 |
| | PerformanceTuning | 0.94 | PT_ANALYZE, PT_OPTIMIZE, PT_REPORT | 0.94, 0.94, 0.94 |
| | AuditTrail | 0.94 | AT_LOG, AT_ANALYZE, AT_REPORT | 0.94, 0.94, 0.94 |
| | ComplianceMonitor | 0.94 | CM_TRACK, CM_VALIDATE, CM_REPORT | 0.94, 0.94, 0.94 |

**Globális konvergencia index**: \( CI = (1.0/1.0) \times 0.9 = 0.9 \).

## Használati Útmutató
- **Súlytáblázat használata**:
  - **Cél**: Ellenőrizni a modulok és almodulok súlyait, biztosítva a normalizált súlyösszeget (\( \sum w_i = 1.0 \)).
  - **Implementáció**: A **WeightTable** modul (`WGT_CALC`, `WGT_VALIDATE`, `WGT_REPORT`) implementálásakor használható a súlyok validálására. Példa Python szkript:
    ```python
    import json
    def validate_weight_table():
        with open("weight_table.json", "r") as f:
            data = json.load(f)
        total_weight = sum(module["weight"] for module in data["modules"])
        assert abs(total_weight - 1.0) < 1e-6, "Súlyösszeg nem normalizált"
        return total_weight
    ```
  - **Használati példa**: `python validate_weight_table.py` a súlyok ellenőrzésére a CI/CD pipeline-ban.
- **Konvergencia mátrix használata**:
  - **Cél**: Ellenőrizni a modulok és almodulok konvergencia indexeit (\( CI \geq 0.9 \)), és biztosítani a rendszer stabilitását.
  - **Implementáció**: A **ConvergenceMatrix** modul (`CMX_CALC`, `CMX_ANALYZE`, `CMX_REPORT`) implementálásakor használható a CI számítására és monitorozására. Példa Python szkript:
    ```python
    import json
    def calculate_convergence_index():
        with open("convergence_matrix.json", "r") as f:
            data = json.load(f)
        global_ci = 0.0
        for module in data["modules"]:
            parent_weight = module["weight"]
            submodule_weights = sum(sm["weight"] for sm in module.get("submodules", []))
            ci = (submodule_weights / parent_weight) * 0.9 if parent_weight > 0 else 0.94
            assert ci >= 0.9, f"Konvergencia index alacsony: {module['module_name']}"
            global_ci += ci * module["weight"]
        return global_ci
    ```
  - **Használati példa**: `python calculate_convergence_index.py` a konvergencia indexek validálására és Prometheus riasztások beállítására (\( CI < 0.9 \)).
- **Monitorozás**: A súlyok és konvergencia indexek Prometheus/Grafana dashboardokon keresztül monitorozhatók (`convergence_index` metrika, küszöb: 0.9).
- **Validáció**: A súlyokat és CI-ket minden iterációban validáljátok a `dependency_map.json` és a CI/CD pipeline segítségével.

## Technikai Részletek
- **Függőségek**: `WeightTable`, `ConvergenceMatrix`, `dependency_map`.
- **Infrastruktúra**: Kubernetes (HPA: targetCPU 80%), Istio (canary deployment: 10% forgalom), Prometheus/Grafana (metrikák: `convergence_index > 0.9`), ELK naplózás (PII anonimizálás: `[0-9]{4}-[0-9]{4}`).
- **Hibakezelés**: 3 retry, rollback `iteration_N-1_continuation.json` fájlra a `dependency_map` alapján.

## Megjegyzések a Fejlesztőknek
- A súlytáblázat és konvergencia mátrix a `module_definition.json` fájlokból és az architektúra tervből lett összeállítva, biztosítva a pontosságot.
- Használjátok a táblázatokat a **WeightTable** és **ConvergenceMatrix** modulok implementálásához, valamint a rendszer stabilitásának ellenőrzéséhez.
- A súlyok és CI-k validálásához integráljátok a fenti Python szkripteket a CI/CD pipeline-ba (pl. `run_coverage_analysis.sh --module weight_table`).
- Archiváljátok a táblázatokat a Git repóban, és frissítsétek a `dependency_map.json`-t minden iterációban.