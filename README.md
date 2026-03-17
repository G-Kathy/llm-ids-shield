# 🛡️ LLM-IDS-Shield  

LLM-IDS-Shield is a **next-generation cybersecurity system** that combines **Machine Learning (XGBoost)**, **Vector Search (Pinecone)**, and **Large Language Models (LLMs)** to perform **intelligent intrusion detection, explanation, and mitigation recommendation**.

This project implements a **Security RAG (Retrieval-Augmented Generation)** pipeline for analyzing network traffic and generating **SOC-style incident reports**.

---

##  Key Features

- **ML-based Intrusion Detection** (XGBoost)
- **Semantic Feature Encoding** using BGE embeddings
- **Vector Database Retrieval** (Pinecone)
- **Confidence-Aware Retrieval Strategy**
- **Hybrid Retrieval (Global + Filtered)**
- **Reranking with Cross-Encoder**
- **Mitigation Knowledge Integration (MITRE + NIST)**
- **LLM-based Reasoning (Groq LLaMA 3.3 70B)**
- **Explainable SOC Reports**
- **Research-grade Evaluation Pipeline**

---

##  System Architecture

```

Network Traffic
↓
ML IDS Model (XGBoost)
↓
Prediction + Confidence
↓
Incident Packet Generation
↓
Semantic Embedding (BGE)
↓
Pinecone Retrieval
├── Global Search
└── Filtered Search (by attack)
↓
Merge + Deduplication
↓
Reranking (Cross-Encoder)
↓
Mitigation Retrieval (MITRE + NIST)
↓
LLM Reasoning (Groq LLaMA)
↓
SOC Incident Report

```

---

## 📂 Project Structure

```
LLM-IDS-SHIELD/
│
├── cicids2017/
│   ├── results/
│   ├── datapreprocessing.ipynb
│   ├── model_training.ipynb
│   
│
├── cicids2017_results_evaluation/
│
├── iotid20/
│   ├── results/
│   ├── embedding.ipynb
│   ├── model_training.ipynb
│
├── iotid20_results_evaluation/
│       
│
├── unswnb15/
│   ├── results/
│   ├── datapreprocessing.ipynb
│   ├── model_training.ipynb
│
├── unswnb15_results_evaluation/
│       
│
├── datasets/
│   ├── cicids2017/
│   ├── iotid20/
│   └── unswnb15/
│
├── Graphs/
│   ├── embedding_visualization/
│   ├── sample_llm_outputs/
│   ├── shap_analysis/
│   └── visualizations/
│
├── metadata/
│   └── cicids17_features.md
│
├── methodology/
│   └── architecture.png
│
├── mitigation_embedding/
│   └── mitigation_embedding_all.ipynb
│
├── rag_llm_pipeline/
│   └── retrieve.ipynb
│
├── synthetic_data/
│
├── .env
├── .gitignore
├── README.md
└── requirements.txt

```

---

## Datasets Used

| Dataset | Description |
|--------|------------|
| **IoTID20** | IoT-based intrusion detection dataset |
| **CICIDS2017** | Enterprise network attack dataset |
| **UNSW-NB15** | Modern synthetic attack dataset |



---

## Feature Engineering

- Selected **shared features across datasets**
- Mapped all datasets → **IoTID20 schema**
- Applied:
  - Feature normalization
  - Percentile-based semantic binning
  - Hierarchical feature grouping

---

## Embedding Strategy

- Model: `BAAI/bge-large-en-v1.5`
- Method:
  - Convert structured data → natural language
  - Generate embeddings via HuggingFace Inference API
  - Apply **L2 normalization**

---

##  Vector Databases

### 1️⃣ Detection Index
- **Index Name:** `cybersec-llm-rag`
- Contains:
  - Traffic embeddings
  - Attack labels
  - Dataset info
  - Confidence & metadata

### 2️⃣ Mitigation Index
- **Index Name:** `mitigation-vector-db`
- Contains:
  - MITRE ATT&CK mappings
  - NIST controls
  - Mitigation descriptions

---

## Retrieval Strategy

### Confidence-Aware Retrieval

| Confidence | Strategy |
|----------|---------|
| High (≥ 0.8) | Filtered + Global |
| Medium (0.5–0.8) | Global + Filtered |
| Low (< 0.5) | Global only |

---

### Retrieval Pipeline

```

Embedding
↓
Global Search
↓
Filtered Search (attack-aware)
↓
Merge + Deduplicate
↓
Rerank (Cross-Encoder)

```

---

## LLM Reasoning

- Model: **Groq LLaMA-3.3-70B**
- Role: **SOC Analyst**
- Capabilities:
  - Attack validation
  - Conflict detection
  - Explanation generation
  - Mitigation reasoning
  - Severity assessment

---

## Example Output

```

SOC INCIDENT ANALYSIS REPORT

Predicted Attack: Mirai ACK Flooding
Confidence: 0.34

Analysis:
The prediction confidence is low. Retrieved cases indicate stronger similarity with Mirai UDP flooding...

Recommended Mitigations:

1. Network Intrusion Prevention (M1031)

   * Explanation: ...
   * Implementation: ...

Severity: High

````

---

## Installation

```bash
git clone https://github.com/your-username/LLM-ids-sheild.git
cd LLM-ids-sheild
pip install -r requirements.txt
````

---

## 🔑 Environment Variables

Create a `.env` file:

```
HF_TOKEN=your_huggingface_token
PINECONE_API_KEY=your_pinecone_key
GROQ_API_KEY=your_groq_key
```

---

## ▶️ Usage

### 1️⃣ Run Embedding Pipelines

* Execute notebooks in `/embedding/`

### 2️⃣ Upload to Pinecone

* Embeddings + metadata

### 3️⃣ Run RAG Pipeline

* Open:

```
rag_llm_pipeline/retrieve.ipynb
```

---

