# Robotics-news-rag-dashboard

An end-to-end project that collects robotics-related news articles, builds a searchable knowledge base, and serves a lightweight **RAG (Retrieval-Augmented Generation)** demo with an LLM-powered dashboard.

## Features
- News crawling & parsing (site-specific scrapers)
- Cleaning, deduplication (hash-based), and metadata management
- Chunking + embeddings + vector index (e.g., FAISS)
- RAG QA pipeline (retrieve → prompt → generate)
- Simple dashboard for search, filtering, and Q&A (e.g., Streamlit)

## Repository Structure
- `crawlers/` : site-specific crawlers/parsers
- `data/` : (not included) raw/processed datasets
- `index/` : (not included) vector index artifacts
- `rag/` : retrieval + prompting + QA logic
- `dashboard/` : UI app

## Setup
- Copy `.env.example` to `.env` and fill in required keys.
- Datasets and index files are not tracked by git (see `.gitignore`).
