# Document RAG QA + Evaluation Dashboard

A small end-to-end **Retrieval-Augmented Generation (RAG)** system with:

- A clean **Light / Dark mode** web UI for asking questions over local documents
- A simple **in-memory vector index** using `sentence-transformers` + `scikit-learn`
- A **/metrics API** that exposes RAG evaluation results as JSON
- A **dashboard page** that visualizes Hit@K and MRR per query

Built by **Mohamed Elsaka** as part of an AI engineering portfolio.

---

## Features

### 1. RAG Question Answering UI

- Loads and chunks text files from `data/documents/`
- Embeds chunks with a `sentence-transformers` model
- Uses cosine similarity over an in-memory nearest neighbors index
- Web UI at [`/app`](http://127.0.0.1:8000/app):
  - Light / Dark mode toggle
  - Top-K control
  - Retrieved chunks with document names and scores
  - Footer credit: *Built by Mohamed Elsaka*

### 2. Evaluation + Metrics

This project is designed to be evaluated by an external script (for example a
separate `rag-eval-benchmark` runner). That script writes evaluation outputs
into the `metrics/` folder:

- `metrics/summary.json` – aggregate metrics:
  - `hit_at_k`
  - `mrr`
  - `n_queries`
  - `k`
- `metrics/eval_logs.jsonl` – one JSON line per query:
  - `id`, `query`
  - `relevant_doc_ids`
  - `retrieved_doc_ids`
  - `hit_at_k`, `reciprocal_rank`, `first_relevant_rank`

The API exposes those files through:

- `GET /metrics` – raw JSON (summary + per-query logs)
- `GET /dashboard` – dark themed dashboard UI that renders a metrics table

---

## Project Structure

```text
.
├── README.md
├── requirements.txt
├── Dockerfile
├── .gitignore
├── src/
│   ├── __init__.py
│   ├── api.py              # FastAPI app: RAG UI, /metrics, /dashboard
│   ├── rag_pipeline.py     # RAGPipeline: loading docs, chunking, embeddings
│   └── schemas.py          # Pydantic models for requests and responses
├── data/
│   └── documents/          # Source documents for the RAG knowledge base
│       ├── rag_intro.txt
│       ├── rag_overview.txt
│       ├── rag_best_practices.txt
│       ├── chunking_strategy.txt
│       ├── vector_search.txt
│       ├── ml_basics.txt
│       ├── ml_monitoring.txt
│       ├── product_use_cases.txt
│       └── deployment_guide.txt
├── metrics/
│   ├── summary.json        # Example evaluation summary
│   └── eval_logs.jsonl     # Example per-query logs
└── logs/
    └── .gitkeep            # placeholder for runtime logs
