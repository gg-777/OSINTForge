# OSINTForge
where intelligence is “forged” and scored
<h1 align="center">🧠 OSINTForge ⚒️</h1>

<p align="center">
  <em>Forging raw OSINT into actionable threat intelligence</em><br/>
  <img src="assets/osintforge_banner.svg" width="600"/>
</p>

---

### 🔍 Overview
OSINTForge automates open-source intelligence ingestion, normalization, and risk scoring through a unified AI pipeline.

- **Ingests** dozens of intel feeds (MISP, OTX, NVD, CISA, Intel471, ExploitDB)
- **Normalizes** all data into STIX-2 format
- **Correlates** indicators with your core assets
- **Scores** threats via asset criticality, exploitability, and threat activity
- **Visualizes** results in dashboards or APIs for SOC and analysts


          +--------------------+
          |  OSINT Feeds (APIs)|  ←  OTX, MISP, Intel471, CVE, etc.
          +--------------------+
                     ↓
              [ ingestion ]
                     ↓
          +----------------+
          |  minio / pgsql |
          +----------------+
                     ↓
              [ Agent Zero ]   ←  ⚡ brains: scoring, ML, correlation
                     ↓
            [ opensearch / webui ]


---

### ⚙️ Stack
| Layer | Technology |
|-------|-------------|
| Storage | PostgreSQL · OpenSearch · Milvus · MinIO |
| Processing | Python · FastAPI · Airflow DAGs |
| Models | Sentence-Transformers · LSTM Autoencoders |
| Orchestration | Docker Compose · RangeOS Integration |

---

### 🧩 Roadmap
- [ ] Threat actor relationship graph (Neo4j)
- [ ] Risk scoring API endpoints
- [ ] ML-driven prioritization model
- [ ] PoC correlation dashboard

---

### 🛡️ License
Apache 2.0 — built for research, SOC augmentation, and continuous red-blue collaboration.
