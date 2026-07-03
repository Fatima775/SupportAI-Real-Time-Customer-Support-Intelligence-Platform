# SupportAI — Real-Time Intelligent Customer Support Platform

![Python](https://img.shields.io/badge/Python-3.11+-blue)
![PySpark](https://img.shields.io/badge/PySpark-4.0.1-orange)
![Delta Lake](https://img.shields.io/badge/Delta%20Lake-4.0.0-00ADD8)
![RAG](https://img.shields.io/badge/RAG-Hybrid%20Retrieval-purple)
![LLM](https://img.shields.io/badge/LLM-Llama%203.1-green)
![Status](https://img.shields.io/badge/Status-Completed-success)

SupportAI is an end-to-end, production-simulated customer-support intelligence platform that integrates modern data engineering, data-quality validation, Lakehouse architecture, hybrid retrieval, cross-encoder reranking, and grounded response generation using Llama through Groq.

The project demonstrates the complete lifecycle of customer-support data, starting from streaming-style ticket ingestion and contract validation, moving through Bronze, Silver, and Gold Delta Lake layers, and ending with an Advanced Retrieval-Augmented Generation pipeline that generates source-grounded customer-support responses.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Project Objectives](#project-objectives)
- [Dataset](#dataset)
- [System Architecture](#system-architecture)
- [Project Structure](#project-structure)
- [Methodology](#methodology)
- [Advanced RAG Pipeline](#advanced-rag-pipeline)
- [Knowledge Base](#knowledge-base)
- [Data Quality and Governance](#data-quality-and-governance)
- [Technologies](#technologies)
- [Installation](#installation)
- [Usage](#usage)
- [Results](#results)
- [Example RAG Query](#example-rag-query)
- [Key Features](#key-features)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [Acknowledgments](#acknowledgments)
- [Author](#author)

---

## Project Overview

Customer-support systems receive large numbers of questions related to orders, refunds, payments, shipping, damaged items, account access, and other business processes.

A traditional Large Language Model may generate fluent responses, but it can produce information that is not aligned with the company's actual policies.

SupportAI addresses this issue by combining:

- Real-time and event-driven data-engineering concepts
- Data contracts and quality validation
- Bronze, Silver, and Gold Lakehouse layers
- A governed customer-support knowledge base
- Dense semantic retrieval
- Sparse keyword retrieval
- Reciprocal Rank Fusion
- Cross-encoder reranking
- Grounded Llama response generation
- Data lineage and workflow orchestration

The platform ensures that the final response is generated from approved company-policy documents rather than relying only on the general knowledge of the language model.

---

## Problem Statement

Customer-support agents must respond quickly while ensuring that their answers are accurate, consistent, and aligned with approved business policies.

Several challenges can affect response quality:

- Incoming customer tickets may contain missing or invalid fields.
- Duplicate tickets may distort operational analytics.
- Raw customer-support data may not be ready for AI consumption.
- Semantic search may miss exact keywords, policy numbers, or time limits.
- Keyword search may fail to understand paraphrased customer questions.
- Large Language Models may generate unsupported or hallucinated information.
- Data transformations and AI outputs may be difficult to audit or trace.

SupportAI solves these challenges through an integrated data and AI architecture.

---

## Project Objectives

The main objectives of this project are to:

- Build a streaming-style customer-support ingestion pipeline.
- Validate incoming events using a formal Pydantic data contract.
- Route invalid events into a quarantine layer.
- Build Bronze, Silver, and Gold tables using PySpark and Delta Lake.
- Apply automated data-quality checks using Great Expectations.
- Evaluate data using the six DAMA quality dimensions.
- Demonstrate Delta Lake schema enforcement and MERGE/UPSERT operations.
- Build a structured NovaStore customer-support knowledge base.
- Implement dense semantic retrieval using ChromaDB.
- Implement sparse keyword retrieval using BM25.
- Combine retrieval results using Reciprocal Rank Fusion.
- Improve ranking precision using a cross-encoder model.
- Generate grounded customer-support answers using Llama through Groq.
- Evaluate retrieval performance using Hit@1, Hit@3, and MRR.
- Record pipeline lineage using OpenLineage.
- Generate an executable Apache Airflow DAG.

---

## Dataset

The project uses the:

**Bitext Customer Support LLM Chatbot Training Dataset**

Dataset source:

```text
bitext/Bitext-customer-support-llm-chatbot-training-dataset
```

The complete dataset contains:

- **26,872 customer-support records**
- Customer instructions and questions
- Support categories
- Customer intents
- Reference responses
- Data-quality flags

### Main Source Fields

| Field | Description |
|---|---|
| `instruction` | Customer-support question |
| `category` | General support category |
| `intent` | Specific customer intent |
| `response` | Reference support response |
| `flags` | Dataset-related quality or linguistic flags |

For the production-simulated pipeline, the project samples **200 records** and enriches them with operational fields.

### Enriched Ticket Fields

| Field | Description |
|---|---|
| `ticket_id` | Unique ticket identifier |
| `customer_id` | Customer identifier |
| `question` | Customer question |
| `category` | Support category |
| `intent` | Customer intent |
| `channel` | Chat, email, mobile application, or web form |
| `priority` | Ticket priority |
| `status` | Current ticket status |
| `event_timestamp` | Event creation time |
| `reference_response` | Dataset reference response |
| `flags` | Original dataset flags |

A few controlled data-quality violations are intentionally introduced to test contract validation and quarantine behavior.

---

## System Architecture

```text
Bitext Customer-Support Dataset
                ↓
Production-Simulated Ticket Events
                ↓
Kafka-Style Producer and Consumer
                ↓
Pydantic Data Contract
        ┌───────┴────────┐
        ↓                ↓
 Valid Events       Invalid Events
        ↓                ↓
 Bronze Layer       Quarantine Layer
        ↓
Great Expectations Quality Gate
        ↓
DAMA Quality Evaluation
        ↓
PySpark + Delta Lake
        ↓
Bronze → Silver → Gold
        ↓
NovaStore Knowledge Base
        ↓
Semantic Chunking + Metadata
        ↓
ChromaDB Dense Retrieval
        +
BM25 Sparse Retrieval
        ↓
Reciprocal Rank Fusion
        ↓
Cross-Encoder Reranking
        ↓
Llama 3.1 through Groq
        ↓
Grounded Answer with Sources
        ↓
Gold RAG Answer Table
        ↓
OpenLineage + Airflow DAG
```

---

## Project Structure

```text
SupportAI-Final-Project/
│
├── SupportAI_Final_Project.ipynb
├── README.md
├── requirements-colab.txt
├── PROJECT_MANIFEST.json
│
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

---

## Methodology

### 1. Source Data Loading and Profiling

The Bitext dataset is loaded from Hugging Face and inspected before processing.

The profiling stage examines:

- Dataset size
- Available columns
- Missing values
- Duplicate identifiers
- Category distribution
- Intent distribution
- Customer-question length

The raw sample contains intentionally invalid records to demonstrate quality validation.

---

### 2. Ticket Event Enrichment

Each sampled record is transformed into a production-simulated customer-support event.

Operational information is added, including:

- Ticket identifiers
- Customer identifiers
- Communication channels
- Priorities
- Status values
- Event timestamps

This transformation makes the historical dataset behave like events arriving in a live customer-support system.

---

### 3. Pydantic Data Contract

A Pydantic v2 model defines the formal contract between the ticket producer and the downstream pipeline.

The contract validates:

- Required fields
- Identifier formats
- Valid communication channels
- Non-empty customer questions
- Question-length limits
- Valid event timestamps
- Standard domain values

Records that fail validation are not silently discarded. They are routed to a dedicated quarantine file with their rejection reasons.

---

### 4. Streaming-Style Ingestion

The notebook implements a local Kafka-style broker with:

- One producer
- Multiple topic partitions
- Multiple consumers
- Partition identifiers
- Offset tracking
- Event timestamps
- Concurrent processing

This simulation demonstrates event-driven architecture without requiring an external Kafka server inside Google Colab.

The project also generates additional artifacts that can be used with a real Kafka or Redpanda environment outside Colab.

---

### 5. Bronze Layer

The Bronze layer preserves events close to their original form.

It stores:

- Raw customer-support fields
- Kafka partition metadata
- Kafka offsets
- Ingestion timestamps
- Valid and invalid source characteristics

The Bronze Delta table contains all **200 received events**.

This layer provides a historical record that can be reprocessed if downstream logic changes.

---

### 6. Data-Quality Validation

Two complementary quality approaches are used.

#### Great Expectations

The Great Expectations suite validates the accepted dataset as a batch.

The checks include:

- Non-null ticket identifiers
- Unique ticket identifiers
- Non-empty customer questions
- Valid support channels
- Valid priority values
- Correct identifier formats
- Valid event timestamps

#### DAMA Quality Dimensions

The project also evaluates data across six standard DAMA quality dimensions:

1. Completeness
2. Accuracy
3. Consistency
4. Timeliness
5. Uniqueness
6. Validity

The downstream pipeline continues only when the quality gate passes.

---

### 7. Silver Layer

The Silver layer contains cleaned, contract-valid, quality-approved records.

Processing includes:

- Removing duplicate ticket identifiers
- Normalizing whitespace
- Normalizing domain values
- Masking email addresses
- Filtering invalid events
- Preserving standardized schemas

The final Silver Delta table contains **195 valid and unique tickets**.

---

### 8. Gold Layer

The Gold layer contains business-ready and AI-serving datasets.

The project creates:

- Support category and intent summaries
- Channel-level summaries
- Operational ticket statistics
- RAG answer records
- Retrieved source identifiers
- Generation metadata

The final generated answer is stored as a governed Gold Delta asset rather than remaining only as temporary notebook output.

---

### 9. Delta Lake Features

The project demonstrates several Delta Lake capabilities.

#### ACID Transactions

Delta Lake provides reliable and atomic writes over Parquet-based storage.

#### Schema Enforcement

A controlled negative test attempts to append an incompatible schema.

Delta Lake successfully rejects the invalid append, preventing schema drift and downstream corruption.

#### MERGE / UPSERT

The project demonstrates how an existing ticket can be updated while unseen tickets are inserted.

```text
Existing ticket_id → UPDATE
New ticket_id      → INSERT
```

#### Transaction History

Delta operations can be inspected through the table transaction history for auditing and reproducibility.

---

## Advanced RAG Pipeline

The RAG component retrieves relevant policy evidence before generating an answer.

```text
Customer Question
        ↓
Dense Vector Retrieval
        +
BM25 Keyword Retrieval
        ↓
Reciprocal Rank Fusion
        ↓
Cross-Encoder Reranking
        ↓
Top Policy Chunks
        ↓
Grounded Prompt
        ↓
Llama Response through Groq
```

---

### Dense Semantic Retrieval

The project uses:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The embedding model converts customer questions and policy chunks into numerical vector representations.

ChromaDB stores:

- Chunk embeddings
- Original text
- Document identifiers
- Policy titles
- Sections
- Categories
- Source filenames
- Policy versions
- Effective dates

Semantic retrieval can find relevant passages even when the customer uses different wording from the policy.

---

### BM25 Keyword Retrieval

BM25 provides sparse keyword search.

It is especially useful for:

- Exact policy terms
- Time periods
- Product names
- Order identifiers
- Specific procedural words
- Numbers and business constraints

BM25 complements vector search by capturing exact lexical matches.

---

### Reciprocal Rank Fusion

Dense retrieval and BM25 produce separate ranked lists.

Reciprocal Rank Fusion combines the rankings without requiring the scores from both retrieval methods to use the same scale.

Documents ranked highly by both retrievers receive stronger combined rankings.

---

### Cross-Encoder Reranking

The fused candidates are reranked using:

```text
cross-encoder/ms-marco-MiniLM-L-6-v2
```

Unlike the embedding retriever, the cross-encoder jointly reads the customer question and each candidate passage.

This produces a more precise relevance score and selects the strongest evidence before the prompt is sent to the LLM.

---

### Grounded Llama Generation

The final answer is generated using:

```text
llama-3.1-8b-instant
```

through the Groq API.

The model receives:

- The original customer question
- The top reranked policy chunks
- Source labels
- Instructions not to use external knowledge
- Instructions not to invent amounts, dates, or procedures
- Instructions to request human escalation when the context is insufficient

The system therefore uses Llama for response formulation, not as the primary source of company-policy knowledge.

---

### Extractive Fallback

If the Groq key is unavailable or the API request fails, the system generates an extractive grounded answer directly from the retrieved evidence.

This allows the pipeline to continue without losing the retrieved policy context.

---

## Knowledge Base

The fictional NovaStore knowledge base contains eight customer-support policy documents.

| Document | Purpose |
|---|---|
| `account_and_password.md` | Account access and password-reset procedures |
| `damaged_or_missing_items.md` | Damaged, incorrect, or missing item procedures |
| `order_cancellation.md` | Order cancellation policies |
| `order_tracking.md` | Shipment and order-tracking instructions |
| `payments_and_billing.md` | Payment, billing, and charge issues |
| `refunds_and_returns.md` | Return eligibility and refund procedures |
| `shipping_and_delivery.md` | Shipping times and delivery policies |
| `support_escalation.md` | Escalation to human support agents |

### Chunking Strategy

The chunking process:

- Preserves Markdown section boundaries
- Splits long sections into overlapping sentence windows
- Retains policy metadata
- Creates searchable, source-aware chunks

The eight policy documents are transformed into **50 indexed knowledge chunks**.

---

## Data Quality and Governance

### Quarantine Layer

Invalid records are stored with rejection reasons instead of being silently deleted.

### Metadata Preservation

Every knowledge chunk retains:

- Document ID
- Title
- Category
- Section
- Source
- Version
- Effective date

### OpenLineage

The project emits official OpenLineage START and COMPLETE events for:

- Kafka ingestion
- Quality validation
- Lakehouse transformation
- RAG indexing
- Retrieval evaluation
- RAG serving

The events are saved to a JSONL lineage file for auditability.

### Airflow DAG

The project generates an executable Apache Airflow DAG.

The workflow includes:

```text
Prepare Source Data
        ↓
Kafka Ingestion
        ↓
Quality Gate
        ↓
Lakehouse Transformation
        ↓
RAG Indexing
        ↓
Retrieval Evaluation
        ↓
RAG Serving
```

A failed quality command returns a non-zero exit code and blocks downstream tasks.

---

## Technologies

| Technology | Role |
|---|---|
| Python | Main implementation language |
| Hugging Face Datasets | Loading the Bitext dataset |
| Pandas | Data inspection and lightweight processing |
| Pydantic v2 | Event-level data contract |
| Asyncio | Concurrent producer and consumer simulation |
| Kafka-style Broker | Streaming and partition simulation |
| PySpark | Distributed data transformation |
| Delta Lake | Bronze, Silver, and Gold Lakehouse storage |
| Great Expectations | Batch data-quality validation |
| DAMA Quality Framework | Six-dimensional quality assessment |
| ChromaDB | Persistent vector database |
| Sentence Transformers | Text embeddings |
| BM25 | Sparse keyword retrieval |
| Reciprocal Rank Fusion | Hybrid result fusion |
| Cross-Encoder | Candidate reranking |
| Groq | Hosted Llama inference |
| Llama 3.1 | Grounded response generation |
| OpenLineage | Pipeline lineage and audit events |
| Apache Airflow | Workflow orchestration artifact |
| Google Colab | Notebook execution environment |
| GitHub | Project documentation and version control |

---

## Installation

### Recommended Environment

The notebook is primarily designed for Google Colab.

### Install Dependencies

Run the installation cell included at the beginning of the notebook.

The main dependencies are:

```text
datasets
loguru
pydantic
pyspark
delta-spark
great-expectations
chromadb
sentence-transformers
rank-bm25
transformers
sentencepiece
confluent-kafka
openlineage-python
groq
```

The dependency list is also available in:

```text
requirements-colab.txt
```

---

## Groq Configuration

To enable Llama generation in Google Colab:

1. Open the Colab **Secrets** panel.
2. Add a new secret:

```text
Name: GROQ_API_KEY
Value: your Groq API key
```

3. Enable **Notebook access**.

The API key should not be written directly inside the notebook.

---

## Usage

1. Open `SupportAI_Final_Project.ipynb` in Google Colab.
2. Connect to a fresh runtime.
3. Run the dependency-installation cell.
4. Restart the runtime once if requested.
5. Run the remaining cells sequentially from top to bottom.
6. Ensure `GROQ_API_KEY` is available before the generation stage.
7. Review the final summary printed by the notebook.

The cells should be executed in order because later components depend on configuration objects, Delta tables, knowledge chunks, and indexes created in earlier stages.

---

## Results

### Pipeline Summary

| Metric | Result |
|---|---:|
| Complete source dataset | 26,872 records |
| Sampled ticket events | 200 |
| Bronze events | 200 |
| Contract-valid events before deduplication | 196 |
| Quarantined contract violations | 4 |
| Silver valid and unique tickets | 195 |
| Knowledge-base documents | 8 |
| Knowledge chunks | 50 |
| ChromaDB vector index size | 50 |
| Great Expectations quality gate | Passed |
| DAMA quality gate | Passed |
| Delta schema enforcement | Passed |

### Retrieval Evaluation

| Metric | Score |
|---|---:|
| Hit@1 | 0.90 |
| Hit@3 | 1.00 |
| Mean Reciprocal Rank | 0.95 |

The results show that the expected policy document appeared in the first result for 90% of the test questions and within the top three results for all evaluated questions.

---

## Example RAG Query

### Customer Question

```text
I received a damaged product. What information do you need from me?
```

### Retrieval Process

```text
Vector search candidates : 10
BM25 search candidates   : 10
RRF fused candidates     : 8
Final reranked context   : 3 chunks
```

### Top Retrieved Evidence

The pipeline retrieves evidence from:

- `damaged_or_missing_items.md`
- `refunds_and_returns.md`

### Generated Grounded Answer

```text
To process your damaged product claim, please provide:

- Your order number
- A clear description of the damage
- Photos of the damaged item

Please report the issue within 48 hours of delivery, as stated
in the Damaged, Incorrect, or Missing Items Policy.
```

The answer is generated from retrieved NovaStore policy content and includes a source reference.

---

## Key Features

### End-to-End Data Engineering

- Streaming-style ingestion
- Producer and consumer architecture
- Partition and offset tracking
- Bronze, Silver, and Gold layers
- Delta Lake schema enforcement
- MERGE and UPSERT operations

### Data Quality

- Pydantic event validation
- Quarantine routing
- Great Expectations checks
- DAMA six-dimension evaluation
- Duplicate removal
- Domain-value normalization

### Advanced RAG

- Dense semantic retrieval
- BM25 sparse retrieval
- Reciprocal Rank Fusion
- Cross-encoder reranking
- Metadata-aware evidence
- Source-grounded generation
- Extractive fallback

### Governance and Observability

- OpenLineage events
- Source tracking
- Policy versions and effective dates
- Governed Gold answer storage
- Executable Airflow DAG
- Quality-gate dependency control

---

## Limitations

- The streaming broker used inside the Colab demonstration is a local Kafka-style simulation.
- The NovaStore policies are fictional and created for educational demonstration.
- The retrieval evaluation uses a relatively small labelled test set.
- The project is a production-simulated prototype rather than a deployed production service.
- Llama generation depends on Groq API availability.
- The notebook does not currently provide a web-based customer-support interface.

---

## Future Improvements

Future extensions may include:

- Deploying a real Kafka or Redpanda broker
- Adding Spark Structured Streaming
- Creating a FastAPI inference endpoint
- Building a Streamlit or React user interface
- Containerizing the platform using Docker Compose
- Adding automatic incremental knowledge-base updates
- Implementing Change Data Capture
- Adding multilingual customer-support policies
- Evaluating answer faithfulness using RAGAS
- Adding contradiction detection
- Integrating human-agent feedback
- Deploying monitoring and alerting dashboards
- Adding role-based access control
- Supporting production cloud object storage

---

## Acknowledgments

This project was developed as the final capstone project for the:

**Modern Data Engineering for AI Systems** course.

The project applies concepts related to:

- Modern Data Architectures
- Real-Time Data Pipelines
- Delta Lakehouse
- Data Quality
- Data Governance
- Vector Databases
- Advanced RAG
- Workflow Orchestration
- Data Lineage

The Bitext Customer Support LLM Chatbot Training Dataset was used as the source of customer-support examples.

---

## Author

**Fatimah Almalki**

M.Sc. Computer Science  
AI and NLP Researcher  
Deep Learning, Large Language Models, RAG, and Data Engineering

GitHub: [Fatima775](https://github.com/Fatima775)

---

## Project Status

```text
Completed — Final Capstone Project
```

This project is intended for educational, portfolio, and research-demonstration purposes.
