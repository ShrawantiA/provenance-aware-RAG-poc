# 📘 **Provenance-aware RAG PoC**
*A minimal Retrieval-Augmented Generation pipeline with SHA-256 provenance tagging for AIOps/SOC workflows.*

## 🧭 **1. Overview**
This repository implements a small proof-of-concept system that demonstrates:
- how to retrieve relevant log fragments using semantic search (FAISS)
- how to attach **cryptographic provenance (SHA-256 hashes)** to each retrieved chunk
- how to measure the **latency overhead** added by provenance
- how this retrieval layer supports trustworthy, explainable AIOps agents

It is intentionally simple and self-contained to make it easy to extend as part of research work.

## 🔍 **2. Why Provenance?**
In AIOps and SOC workflows, LLM-based explanations and recommendations must be:
- **trustworthy**
- **grounded in real evidence**
- **verifiable after the fact**

By attaching a **SHA-256 hash** to every retrieved chunk, we create a tamper-proof fingerprint of the evidence used to answer a question.  
This ensures:
- reproducible experiments
- auditability
- reduced hallucinations
- transparent decision traces for operators

This PoC tests how expensive it is to add provenance in a retrieval pipeline.

## 🏗️ **3. System Workflow**

### Data → Chunking → Embeddings → FAISS → Top-k Retrieval → Provenance Hash → Timing Metrics

1. **Load logs** from a small JSON file (`id`, `text` fields).
2. **Chunk** each log into overlapping windows for better retrieval granularity.
3. **Embed** each chunk using SentenceTransformers.
4. **Index** embeddings in FAISS (`IndexFlatL2`).
5. **Retrieve** top-k chunks most semantically similar to the query.
6. **Compute SHA-256 provenance** for each retrieved chunk.
7. **Measure overhead**:
   - pure retrieval time
   - retrieval + provenance time
   - % overhead

## ⚙️ **4. Installation**
```bash
python -m venv .venv
source .venv/bin/activate      # Linux/macOS
.venv\Scripts\activate       # Windows

pip install -r requirements.txt
```

If no requirements file:
```bash
pip install sentence-transformers faiss-cpu numpy pandas tqdm langchain
```

## 🚀 **5. Running the PoC Script**
```bash
python provenance_poc_script.py \
    --data data/sample_logs.json \
    --out results/provenance_results.csv \
    --topk 3 \
    --chunk_size 80 \
    --overlap 20
```

If the data file is missing, the script auto-generates a small dataset.

Outputs include:
- retrieved chunks  
- SHA-256 hashes  
- timing results  
- CSV with overhead metrics  

## 🧪 **6. Running the Notebook**
Use the notebook in the `notebooks/` folder to interactively explore:
- data loading
- chunking
- embeddings
- FAISS vector store
- minimal RAG retrieval
- provenance hashing
- overhead measurement

## 📤 **7. Understanding the Output**
**Retrieved Chunks** – top-k semantic matches.  
**Provenance Hashes** – SHA-256 fingerprints of each chunk.  
**Timing Results** – retrieval time vs provenance overhead.

## 📁 **8. Data Format**
Input JSON list like:
```json
[
  {
    "id": "log-1",
    "text": "User login from IP 10.0.0.1 succeeded."
  }
]
```

You can add fields like timestamp, host, severity for richer datasets.

## 🎓 **9. Research Motivation (MScR AIOps)**
This PoC ties directly into explainable AIOps research:
- provenance ensures traceability
- retrieval grounds LLM responses in evidence
- overhead measurement evaluates feasibility
- modular design supports causal reasoning & policy enforcement

## 🧭 **10. Roadmap / Next Steps**
- Switch to HNSW/IVF for large-scale retrieval
- Add LLM generation layer
- Add provenance graphs (Neo4j/PROV-O)
- Hybrid semantic + metadata search
- Evaluate retrieval accuracy
- Integrate OPA/Rego for policy verification

## 📚 **11. References**
- FAISS documentation  
- SentenceTransformers  
- RAG architecture patterns  

