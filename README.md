# 🧠 Information Retrieval Search Engine — SelfIndex vs Elasticsearch

A modular **Information Retrieval (IR)** system implemented in **Python** to explore and evaluate different **indexing, compression, and query processing strategies** against **Elasticsearch**.  
This project demonstrates the full pipeline — from data preprocessing to indexing, query evaluation, and performance benchmarking — over large **News** and **Wiki** datasets.

---

## 🎯 Project Overview

The goal of this project is to build a simplified search engine (**SelfIndex**) and compare its efficiency and accuracy against **Elasticsearch (ESIndex-v1.0)** using multiple retrieval and compression techniques.  
Each version of **SelfIndex (SelfIndex-v1.xyziq)** represents a combination of indexing type, datastore choice, compression method, and query processing optimization.

---

## 🧩 Activity Breakdown

### 1. 📚 **Data Sources**
- **News Data**: From [Webz.io](https://webz.io) (available on GitHub).  
- **Wiki Data**: From [Hugging Face Datasets](https://huggingface.co/datasets), using split `20231101.en`.  

Both datasets were preprocessed before indexing.

---

### 2. 🧼 **Data Preprocessing**
- Performed **tokenization**, **stopword removal**, and **stemming**.  
- Removed **punctuation** and **special symbols**.  
- Generated **word frequency plots** *before* and *after* preprocessing for both datasets.  
- Saved cleaned outputs for indexing into Elasticsearch and SelfIndex.

---

### 3. ⚙️ **Indexing in Elasticsearch**
- Indexed preprocessed documents into **Elasticsearch** (`ESIndex-v1.0`).  
- Measured **latency**, **throughput**, **memory usage**, and **retrieval metrics** such as `Precision@10`, `Recall@10`, and `MRR@10`.  
- Plotted performance comparisons for both News and Wiki datasets.

---

### 4. 🧠 **Implementation of Custom Search Engine — SelfIndex-v1.xyziq**

A modular, version-controlled custom search engine built in Python.

#### Versioning Convention: `SelfIndex-v1.xyziq`
Each letter encodes an experimental configuration:
| Parameter | Meaning | Values |
|------------|----------|--------|
| **x** | Information Indexed | 1 → Boolean (Doc IDs + Positions) <br> 2 → Ranked (Word Counts) <br> 3 → Ranked (TF-IDF) |
| **y** | Datastore Type | 1 → Custom (Pickle / JSON) <br> 2 → Off-the-shelf (PostgreSQL GIN / RocksDB / Redis) |
| **z** | Compression Method | 1 → Simple (Gap + Variable Byte Encoding) <br> 2 → Off-the-shelf library compression |
| **i** | Index Optimization | 0 → Basic <br> 1 → With Skip Pointers |
| **q** | Query Engine | Tn → Term-at-a-Time <br> Dn → Document-at-a-Time |

---

### 5. 🔍 **Indexing and Query Processing**

- **Boolean Index (x=1):**  
  Built using document and position IDs for basic Boolean retrieval.  
- **Word Count Index (x=2):**  
  Added ranking based on term frequency.  
- **TF-IDF Index (x=3):**  
  Incorporated term weighting for relevance scoring.  

- **TAAT (Term-at-a-Time):**  
  Processes each term’s posting list sequentially to compute document scores.  
- **DAAT (Document-at-a-Time):**  
  Traverses all posting lists in parallel for faster retrieval and scoring.  

---

### 6. 🧮 **Compression and Optimization**

#### Compression
- **Simple Compression (z=1):**  
  Implemented **Gap Encoding** + **Variable Byte Encoding** to reduce posting list size.  
- **Library Compression (z=2):**  
  Applied standard compression utilities (e.g., `zlib`, `lzma`) for comparison.  

#### Optimization
- **Skipping with Pointers (i=1):**  
  Implemented skip pointers to reduce traversal time in long posting lists.  

---

### 7. 📊 **Performance Evaluation and Plots**
- **Plot.C (x variants):** Evaluates impact of Boolean, Ranked, and TF-IDF indexing.  
- **Plot.A (y variants):** Compares custom vs. off-the-shelf datastore choices.  
- **Plot.AB (z variants):** Compares two compression methods.  
- **Plot.AC (q variants):** Compares Term-at-a-Time and Document-at-a-Time retrieval efficiency.  

Metrics Evaluated:
- Latency (p50, p95, p99)  
- Throughput (queries/sec)  
- Memory usage (client and server)  
- Precision@10, Recall@10, MRR@10  

---

## 🧰 **Tech Stack**
- **Language:** Python  
- **Libraries:** `nltk`, `json`, `pickle`, `collections`, `matplotlib`, `re`  
- **Datastores:** JSON / Pickle (Custom), PostgreSQL GIN, Redis, RocksDB  
- **Search Engine for Benchmark:** Elasticsearch  

---

## 📦 **Compression Module**
- **Gap Encoding:** Stores doc-ID differences instead of full IDs.  
- **Variable Byte Encoding:** Encodes integers in minimal bytes.  
- **Effect:** Reduced index size and improved query speed without loss of accuracy.  

---

## 💡 **Example Queries**
```bash
search("machine AND learning")               # Boolean Query
search('"information retrieval system"')     # Phrase Query
search("(data OR analysis) AND NOT model")   # Nested Boolean Query

search("neural networks", mode="TAAT")       # Term-at-a-Time
search("deep learning", mode="DAAT")         # Document-at-a-Time
