# NovaStore Customer Support Knowledge Base

This folder contains a compact, fictional e-commerce knowledge base designed for the final project:
**Real-Time Intelligent Customer Support Platform Using RAG**.

## Included files

- 8 policy documents in Markdown
- `metadata.csv`: document-level metadata
- `kb_chunks.jsonl`: section-level records ready for embedding and vector indexing
- `test_queries.csv`: retrieval test set with expected document IDs

## Recommended RAG workflow

1. Load `kb_chunks.jsonl`.
2. Embed the `text` field.
3. Store embeddings with metadata in ChromaDB.
4. Run BM25 over the same `text` field.
5. Merge dense and sparse results using Reciprocal Rank Fusion.
6. Rerank the top candidates with a cross-encoder.
7. Return the answer with the `source`, `doc_id`, and `section`.

## Important note

All policies are fictional and created only for academic demonstration. They are not legal, financial, or commercial advice and do not represent a real company.
