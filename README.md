# SupportAI — Real-Time Intelligent Customer Support Platform

SupportAI is an end-to-end customer-support intelligence platform that integrates modern data engineering, data-quality validation, hybrid retrieval, reranking, and grounded response generation using Llama through Groq.

## Repository Structure
SupportAI-Final-Project/
├── SupportAI_Final_Project.ipynb
├── README.md
├── requirements-colab.txt
├── PROJECT_MANIFEST.json
└── knowledge_base/
    ├── README.md
    ├── metadata.csv
    ├── kb_chunks.jsonl
    ├── test_queries.csv
    ├── account_and_password.md
    ├── damaged_or_missing_items.md
    ├── order_cancellation.md
    ├── order_tracking.md
    ├── payments_and_billing.md
    ├── refunds_and_returns.md
    ├── shipping_and_delivery.md
    └── support_escalation.md
```

## Main Architecture

```text
Customer Support Events
        ↓
Streaming Ingestion
        ↓
Bronze / Silver / Gold Lakehouse
        ↓
Data-Quality Validation
        ↓
NovaStore Knowledge Base
        ↓
ChromaDB Semantic Retrieval + BM25 Keyword Retrieval
        ↓
Reciprocal Rank Fusion
        ↓
Cross-Encoder Reranking
        ↓
Grounded Llama Response through Groq
```

## Main Components

- Streaming-style customer-support event ingestion
- Bronze, Silver, and Gold Lakehouse layers
- Data-quality validation
- Persistent ChromaDB vector index
- BM25 sparse retrieval
- Reciprocal Rank Fusion
- Cross-encoder reranking
- Grounded answer generation using Llama through Groq
- Source-aware fallback responses
- Data-lineage and project artifacts

## Run in Google Colab

1. Open `SupportAI_Final_Project.ipynb` in Google Colab.
2. Connect to a fresh runtime.
3. Run the notebook installation cell.
4. Restart the session once if requested.
5. Run the remaining cells from top to bottom.
6. Do not skip the configuration or Knowledge Base cells before running the RAG section.

## Groq Configuration

Add the following secret in Google Colab:

```text
Name: GROQ_API_KEY
Value: your Groq API key
```

Enable **Notebook access** before running the Llama generation cells.

## Technologies

Python, PySpark, Delta Lake, Great Expectations, ChromaDB, Sentence Transformers, BM25, Reciprocal Rank Fusion, Cross-Encoder Reranking, Groq, and Llama.
