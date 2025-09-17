# 🔧 Részletes Fejlesztői Roadmap – Evolve Protocol 2.6 Béta

## 🎯 Béta cél
Egy működő demo platform, amely megmutatja:
- hogyan működik a **hibrid intelligencia workflow** (esemény → validáció → döntés),
- hogyan jelenik meg az **etikai validáció** (fairness score, bias check),
- hogyan épül be a **transzparencia** (alap log + vizualizáció),
- és hogy a rendszer **skálázható, robusztus alapokra** épül.

---

## 📅 Iterációs terv (12 iteráció, 12–16 hét)

### **1. Iteráció – Alap infrastruktúra**
- Telepíts **lokális Kubernetes környezetet** (Minikube).
- Állíts be **PostgreSQL adatbázist** (Docker vagy k8s pod).
- Készíts **CI/CD pipeline-t** (GitLab CI `.gitlab-ci.yml`).
- Alap **error tracking** (Sentry vagy ELK logging).  
**Output**: cluster + DB fut, commit után automatikus build+teszt.

---

### **2. Iteráció – EDM (Event Data Manager)**
- Microservice (FastAPI) `/edm/process` endpointtal.
- Input: JSON esemény.
- Validáció: kötelező mezők, típusok.
- Mentés: `events` tábla.  
**Output**: működő API események fogadására.

---

### **3. Iteráció – TVG (Task Validation Generator)**
- Microservice `/tvg/generate`.
- Input: esemény ID.
- Generálj validációs szabályokat.
- Mentés: `validations` tábla.  
**Output**: eseményből validációs feladat generálódik.

---

### **4. Iteráció – ComplianceMonitor + Ethics Engine (alap)**
- Endpoint `/cm/validate`.
- Compliance check dummy (pl. GDPR flag).
- Ethics_engine: dummy fairness score (0.9).
- Mentés: `compliance_results` tábla.  
**Output**: compliance + etikai score az eseményhez kapcsolva.

---

### **5. Iteráció – Workflow összekapcsolás**
- End-to-end pipeline:  
  `/edm/process` → `/tvg/generate` → `/cm/validate`.
- Automatizáld DAG-gal (`update_dag.py`).
- Alap rollback mechanizmus (iteration_N-1_continuation.json).  
**Output**: teljes workflow futtatható.

---

### **6. Iteráció – Monitoring & Observability**
- Telepíts **Prometheus + Grafana**.
- Exporter metrikák: latency, error rate.
- Alap dashboard: API hívások, fairness score trend.  
**Output**: vizualizált metrikák.

---

### **7. Iteráció – Alap UX Demo**
- Egyszerű **frontend (React vagy Streamlit)**.
- Megjeleníti: esemény lista, compliance eredmény, fairness score.
- Grafikon: latency + error rate (Grafana embed vagy API-ból).  
**Output**: első end-user demó.

---

### **8. Iteráció – Deployment stabilizálás**
- Kubernetes deployment manifestek (Helm chart).
- Horizontal Pod Autoscaler (HPA, target 80% CPU).
- Alap retry policy (3x), rollback 5 percen belül.  
**Output**: robusztusabb demo környezet.

---

### **9. Iteráció – Etikai Self-check script**
- Script, ami logból számít fairness p-value-t (dummy, pl. >0.05).
- Automatizált futtatás CI/CD pipeline-ban.
- Eredmények mentése `ethics_checks` táblába.  
**Output**: látható etikai ellenőrzés.

---

### **10. Iteráció – Dokumentáció + Narratíva**
- API dokumentáció (OpenAPI/Swagger).
- Rövid „human story”: hogyan dolgozza fel az adatot, hogyan validál etikailag.
- Pitch deck integráció: mit mutat a demo a befektetőknek.  
**Output**: bemutatásra kész anyag.

---

### **11. Iteráció – Tesztelés & Mock-review**
- OWASP ZAP security scan.
- Dummy load test (pl. Locust).
- Mock review: ön-ellenőrzés a béta előtt.  
**Output**: stabilitási és biztonsági riport.

---

### **12. Iteráció – Béta Release**
- Publikus béta környezet (pl. GCP free tier / Hetzner).
- Stabil demo workflow + frontend bemutató.
- Dokumentáció + mérföldkő riport.  
**Output**: bemutatható béta verzió.

---

## ✅ Béta kritériumok
- **Latency**: <150 ms  
- **Error rate**: <2%  
- **Fairness score**: ≥0.9  
- **Convergence index**: ≥0.9  
- **Demo workflow**: EDM → TVG → ethics_engine → ComplianceMonitor  
- **Dokumentáció**: befektetőbarát + szakértőbarát
