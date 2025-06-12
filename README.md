# Invoice-Reimbursement-System

## 📌 Project Overview

This project is a full-stack solution to analyze a batch of invoice PDF files against a given insurance policy document. It leverages **Gemini Flash**, **ChromaDB**, **Mistral 7B**, and **FastAPI** to create a Retrieval-Augmented Generation (RAG) pipeline. The final product is a public-facing API where users can upload invoice files and ask queries to validate them against policy rules.

---

## ✅ Phase 1: PDF Parsing & Gemini Flash-based Invoice–Policy Analysis

### 🔧 Tools & Technologies:
- `PyMuPDF (fitz)` – for reading and parsing PDF content.
- `Gemini Flash API` – for quick and intelligent invoice-to-policy comparison.
- `Threading` – to parallelize and accelerate the analysis of multiple invoices.

### 📌 Description:
- The policy PDF is parsed and cleaned for semantic clarity.
- Each invoice is parsed and its textual content extracted.
- A composite prompt (invoice + policy) is sent to **Gemini Flash** for compliance analysis.
- Multithreading is used to process multiple invoices concurrently.
- The results for each invoice (compliance status and reasoning) are stored and later printed or saved.

---

## ✅ Phase 2: Vector Embedding with ChromaDB

### 🔧 Tools & Technologies:
- `SentenceTransformers` – for generating document embeddings using `"all-MiniLM-L6-v2"`.
- `ChromaDB` – a lightweight, serverless vector store to index and query documents.

### 📌 Description:
- Each invoice (in `.txt` format) is embedded using MiniLM.
- Metadata (e.g., filename) is stored alongside the document in ChromaDB.
- Top-k relevant documents are retrieved via cosine similarity during queries.

---

## ✅ Phase 3: FastAPI Public API Deployment

### 🔧 Tools & Technologies:
- `FastAPI` – to define RESTful endpoints for uploading files and querying documents.
- `Uvicorn` – as the ASGI server for local/cloud deployment.
- `Pyngrok` – to tunnel the local server and expose it publicly (for demo/testing).
- `Mistral 7B Instruct` – a locally loaded LLM used to answer questions based on document context.
- `Nest AsyncIO` – to handle async event loop in Jupyter/Colab environments.

### 📌 API Endpoints:
- **`/upload/`**: Accepts a `.txt` invoice file, embeds it, and stores it in ChromaDB.
- **`/ask/`**: Accepts a user query, fetches the most relevant invoice, and uses Mistral to generate an answer from its context.

---

## ⚙️ Example Workflow:

1. Upload an invoice (`.txt` format) to `/upload/`.
2. Ask a question like:  
   _“Was the blood test billed in accordance with the policy limits?”_
3. The system retrieves the best-matching invoice using ChromaDB.
4. The Mistral LLM generates a contextualized, human-like answer based on that invoice.

---

## 🧠 Challenges Faced & Solutions

### ❗ Challenges:
- **LLM/API Dilemma**: Selecting the right LLM to analyze invoice content against policy rules was hard. While OpenAI models performed well, they were paid. Free models were either code-centric or underperformed on invoice analysis.
- **Processing Delays**: Analyzing 28 PDF invoices sequentially was too slow. Many free APIs imposed context or rate limits.
- **Quota Limits on Gemini Flash**: Although Gemini Flash performed fast and efficiently, its free-tier quota restricted full batch processing in one run.

### ✅ Solutions:
- Switched to **Gemini Flash**, which processed the invoice–policy comparison much faster and more intelligently than other free options.
- Used **threading** to process multiple invoices in parallel, significantly reducing overall runtime.
- For scalable querying, moved to a **RAG pipeline** using ChromaDB and **Mistral 7B**, thus bypassing LLM quota constraints for querying.

---

## 🚀 Future Enhancements

- Support uploading raw PDF files via FastAPI instead of pre-converted `.txt`.
- Replace Gemini Flash with a self-hosted model like **LLaMA 3** for complete control.
- Add user-friendly frontend (e.g., Streamlit or React).
- Integrate full authentication and rate-limiting on the API.
- Enable batch uploading and querying of multiple invoices at once.

---

