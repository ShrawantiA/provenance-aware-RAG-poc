# Provenance-aware RAG Prototype (PoC)

**Purpose:**  
Early proof-of-concept for provenance-aware RAG pipeline
This prototype explores *provenance anchoring* in retrieval-augmented generation (RAG) pipelines for SOC environments.  
The goal is to tag each retrieved data fragment with a cryptographic hash to maintain content provenance and integrity.

---

## Objectives
- Build a simple RAG pipeline using **LangChain** and a local text dataset.
- Implement provenance tracking with **SHA-256** hashes.
- Measure overhead (latency %) for provenance tagging.
- Prepare baseline for integration into a SOC testbed (Abacws Lab).

---

## Tech Stack
Python, LangChain, FAISS, hashlib, pandas, OpenAI API (or Llama/Mistral).

---

## Example Workflow
1. Load a small set of SOC-like text logs (`sample_logs.json`).
2. Create vector embeddings and index them with FAISS.
3. For each query:
   - Retrieve top-k relevant chunks.
   - Compute a SHA-256 hash for each retrieved chunk.
   - Attach hashes as provenance tags to the response context.
4. Log latency overhead and store results in a CSV.

---

## Next Steps
- Expand to multi-hop retrieval chains.
- Explore verification policies (Rego/OPA).
- Integrate differential privacy redaction for sensitive data.

---

# Provenance-aware RAG Prototype (PoC)

Proof-of-concept for a provenance-aware retrieval-augmented generation (RAG) pipeline targeting SOC-like datasets.
This PoC tags each retrieved chunk with a cryptographic SHA-256 hash and measures the overhead of provenance anchoring.

---

## Quickstart (local, minimal)

**Prereqs**
- Python 3.12.x
- `venv` or similar
- `git`, `pip`

```bash
# clone once (if not already)
git clone https://github.com/ShrawantiA/provenance-aware-RAG-poc.git
cd provenance-aware-RAG-poc

# create & activate a venv (Mac / Linux)
python -m venv .venv
source .venv/bin/activate

# install
pip install -r requirements.txt

# Run the key notebook interactively:
jupyter lab   # then open notebooks/provenance_poc.ipynb

---

*Author:* Shrawanti Anabattula  
*Date:* October 2025  
