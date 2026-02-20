# Chat To PDF

## Overview
Welcome to the Enterprise RAG Workbench. This is a production-ready, full-stack Retrieval-Augmented Generation application designed to bridge the gap between powerful Large Language Models (LLMs) and your private enterprise data. 

A poorly structured README translates to poorly understood software. Therefore, this documentation serves as your architectural blueprint. This system moves beyond basic AI wrappers by implementing a sophisticated **Orchestration Loop** that enforces factual grounding, auditability, and semantic integrity. 

## The Architectural Triad (Features)

This application is built on three foundational pillars:

1. **Intelligent Ingestion & Indexing (Layer 1 & 2)**
   * **What we use:** `PyPDFLoader` and `RecursiveCharacterTextSplitter`.
   * **The Problem it Solves:** Traditional chunking cuts sentences in half, causing "Context Loss". 
   * **The Solution:** We use a 1000-token chunk size with a 15% overlap rule (150 tokens) to preserve semantic boundaries (Paragraph -> Sentence -> Word). This ensures the AI never loses context mid-thought.

2. **Manual Hybrid Search Retrieval (Layer 4 & 6)**
   * **What we use:** HuggingFace `all-MiniLM-L6-v2` local embeddings with **FAISS** (Dense) and **BM25** (Sparse).
   * **The Problem it Solves:** Semantic search misses exact technical jargon (like product codes), while keyword search misses broader concepts.
   * **The Solution:** We run both searches in parallel and fuse them. BM25 filters results using exact term matching, while FAISS refines them by identifying deeper semantic connections. We then apply a manual Reciprocal Rank Fusion (RRF) deduplication logic to get the best of both worlds.

3. **High-Speed Grounded Generation (Layer 8 & 9)**
   * **What we use:** `langchain-groq` with Llama 3.3 70B.
   * **The Problem it Solves:** LLMs hallucinate and invent facts.
   * **The Solution:** The Groq LPU provides sub-100ms reasoning. We use a strict "Prompt Stack" that forces the model to use ONLY the retrieved context and explicitly cite `[Source X]` in its answers. This guarantees 100% auditability.



## Tech Stack
* **Frontend:** Custom HTML5, CSS3, Vanilla JavaScript (Decoupled UI).
* **Backend Framework:** Flask (REST API orchestration).
* **Orchestration Engine:** LangChain v1.x (Stable Ecosystem).
* **Vector Store:** FAISS (CPU-optimized for high-speed local storage).
* **Sparse Index:** Rank-BM25.
* **Embeddings:** Sentence-Transformers (Local execution for privacy and zero API costs).
* **LLM:** Groq (Llama-3.3-70b-versatile).