# Sec PoC Pipeline (Docs Ingest + Search + Vectors + ML Starter)

This repo bundles a local lab stack for ingesting security documents/PoCs into object storage, extracting text/metadata, indexing into OpenSearch, storing vectors in Milvus for semantic search, and a small web UI to query both. It also includes an Airflow TaskFlow DAG and a starter ML notebook for anomaly detection.

> **Safety**: Do not execute PoC code on production systems. Inspect all PoC artifacts before use. Run tests only in isolated lab environments you own or have explicit permission to test.

## Stack
- OpenSearch (full-text search)
- Milvus (vector search)
- MinIO (S3-compatible object storage)
- Postgres (document catalog)
- Apache Tika (text extraction from docs)
- Ingestion Worker (Python + Sentence-Transformers)
- Web UI (FastAPI + Uvicorn) for keyword + vector search
- Airflow DAG (TaskFlow) for scheduled ingest
- Notebook: `notebooks/log_ml_starter.ipynb`

## Quickstart

1) **Start the stack**

```bash
docker compose up --build
```

2) **Create bucket & upload a test doc**

- Open MinIO: http://localhost:9000 (minioadmin/minioadmin)
- Create bucket: `sec-repo`
- Upload a PDF or text under prefix `pocs/` (e.g. `pocs/fortinet/CVE-2024-XXXX-writeup.pdf`)

The ingestion worker will: download -> Tika extract -> embed -> index to OpenSearch -> insert vector to Milvus.

3) **Search**

- Web UI: http://localhost:8088 (enter a query, see keyword + vector results JSON)
- OpenSearch API: http://localhost:9200 (index: `docs`)

4) **Notebook (optional)**

Open `notebooks/log_ml_starter.ipynb` in Jupyter/VSCode and run. Replace synthetic data with real features pulled from OpenSearch.

## Useful Environment Variables
- `OPENSEARCH_URL` (default `http://opensearch-node:9200`)
- `MILVUS_HOST` / `MILVUS_PORT` (default `milvus:19530`)
- `MINIO_ENDPOINT` (`minio:9000`), `MINIO_ACCESS_KEY`, `MINIO_SECRET_KEY`
- `POSTGRES_CONN` (default `postgresql://secuser:secpass@postgres:5432/seccatalog`)
- `TIKA_URL` (`http://tika:9998`)
- `EMBEDDING_MODEL` (`sentence-transformers/all-mpnet-base-v2`)
- `INGEST_BUCKET` (`sec-repo`)

## Airflow (optional)
The DAG `dags/doc_ingest_taskflow_dag.py` demonstrates TaskFlow + dynamic mapping to process S3/MinIO keys. To run, use your existing Airflow environment and mount this `dags/` directory.

## Next Steps
- Join Milvus IDs to OpenSearch docs by storing doc IDs in Milvus as an additional field (add a scalar field).
- Add Postgres joins in `webui` to show titles when doing vector-only search.
- Add authentication/TLS, secrets management, backups, and monitoring for production.
- Extend the ML pipeline with real logs (OpenSearch queries → features → model training).