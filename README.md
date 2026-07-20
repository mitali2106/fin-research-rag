Financial Research RAG Assistant

An AI assistant that answers questions about public companies using source-cited answers drawn from real SEC filings — built to fight hallucination by grounding every response in retrieved evidence, not the model's memory.

Why this exists

Large language models are fluent but unreliable — they invent facts. For anything involving money, an unverifiable answer is worse than no answer. This project uses Retrieval-Augmented Generation (RAG): it retrieves the exact passages from a company's filings that are relevant to your question, then asks the LLM to answer only from those passages, with citations back to the source. If the evidence isn't there, it says so instead of guessing.

What it does
Ask questions about a company ("What are the main risk factors?" "How did revenue guidance change?") and get answers grounded in its filings.
Every answer is cited — each claim links back to the filing, section, and page it came from.
Auto-memo generator — produce a one-page, cited investment summary for a company.
Abstains when unsure — low retrieval confidence returns "not enough information" rather than a fabricated answer.
Architecture
  SEC filings ─► ingest & chunk ─► embed ─► FAISS vector store
                                                   │
                       user question ─► retrieve top-k + rerank
                                                   │
                          relevant chunks + sources ─► LLM (Llama 3)
                                                   │
                              cited answer / investment memo
                                                   │
              FastAPI backend  ◄────────────────►  Next.js chat UI
              (both services containerized with Docker + docker-compose)
Tech stack
Layer	Technology
LLM orchestration	LangChain
Generation model	Llama 3 via Ollama (local, free)
Embeddings	sentence-transformers (all-MiniLM-L6-v2)
Vector store	FAISS
Backend	FastAPI (Python)
Frontend	Next.js (React)
Containerization	Docker + docker-compose
Data source	SEC EDGAR filings

Uses a fully open-source, local stack — no paid API keys required.

Features
Ingestion and chunking pipeline for SEC filings
Semantic embedding and FAISS vector index
RAG retrieval with source citations on every answer
Low-confidence abstention guardrail to prevent fabricated answers
Auto-memo generator for one-page, cited investment summaries
FastAPI backend (/ask, /memo) and Next.js chat UI
Fully containerized with Docker and docker-compose
