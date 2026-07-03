# SupportAI — Real-Time Intelligent Customer Support Platform

SupportAI is an end-to-end customer-support intelligence project combining modern
data engineering, data-quality validation, hybrid retrieval, reranking, and
grounded Llama generation through Groq.

## Repository Structure

```text
SupportAI-Final-Project/
├── SupportAI_Final_Project.ipynb
├── README.md
├── requirements-colab.txt
├── .gitignore
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
Customer-support events
        ↓
Streaming ingestion
        ↓
Bronze / Silver / Gold Lakehouse
        ↓
Data-quality checks
        ↓
NovaStore Knowledge Base
        ↓
ChromaDB semantic retrieval + BM25 keyword retrieval
        ↓
Reciprocal Rank Fusion
        ↓
Cross-encoder reranking
        ↓
Llama grounded answer through Groq
```

## Components

- Streaming-style event ingestion
- Bronze, Silver, and Gold data layers
- Data-quality validation
- Persistent ChromaDB vector index
- BM25 sparse retrieval
- Reciprocal Rank Fusion
- Cross-encoder reranking
- Grounded answer generation with Llama through Groq
- Source-aware fallback response
- Lineage and project artifacts

## Run in Google Colab

1. Open `SupportAI_Final_Project.ipynb` in Google Colab.
2. Connect to a fresh runtime.
3. Run the notebook's installation cell.
4. When installation completes, restart the session once if the notebook requests it.
5. Run the remaining cells from top to bottom.
6. Do not skip configuration or Knowledge Base cells before running the RAG section.

## Groq Secret

In Google Colab, open **Secrets** and add:

```text
Name: GROQ_API_KEY
Value: your Groq API key
```

Enable **Notebook access**.

Never commit the API key to GitHub or paste it directly into the notebook.

## Knowledge Base

The `knowledge_base/` directory contains the NovaStore support-policy documents
used by the retrieval pipeline. It includes policy documents, metadata, prepared
chunks, and test questions.

The notebook may also build or copy working artifacts during execution. Those
generated runtime files do not need to be committed unless required for submission.

## GitHub Upload

Upload the contents of the `SupportAI-Final-Project` folder, not only the ZIP file.

The repository root should show:

```text
SupportAI_Final_Project.ipynb
README.md
requirements-colab.txt
knowledge_base/
```

## Security

- Do not upload `GROQ_API_KEY`.
- Do not upload `.env` files.
- Do not upload generated Chroma databases or large runtime data unless required.
- Colab Secrets are not stored inside this repository.

## Notebook Integrity

`SupportAI_Final_Project.ipynb` is copied exactly from the user-provided notebook.
No notebook cells were modified while creating this package.
