# 📰 Newsfeeds App – Project Overview

We’re building a **modular, high-speed, AI-powered news platform** that delivers smart, summarized, and searchable news using real-time scraping, vector search, and Retrieval-Augmented Generation (RAG).

## 🚨 Problem
- News platforms overwhelm users with **raw, repetitive content**
- No context, no smart filtering, no personalized insights
- Slow, non-scalable scraping and search on traditional systems

## 💡 Our Solution
A fully automated AI pipeline that combines **fast scraping, intelligent summarization, and semantic search** in a modular, scalable architecture.

## 🏗️ System Architecture Overview

### 📦 Modular Dockerized Design
- Each component runs in a separate **Docker container**
- Easily scalable, isolated, and restartable independently

### ⚡ Fast Async Scraper (Celery + Beat)
- Built using **asynchronous Python** for non-blocking I/O
- Each news source (currently NDTV) runs as a **dedicated Celery worker**
- Scheduled with **Celery Beat** (every 30 seconds)
- Scraped data is pushed to **MongoDB (raw collection)**

### 🔄 Pub/Sub Pipeline (Redis + BLPOP)
- After scraping:
  - Article `_id` is pushed to a **Redis List** (acts as a queue)
  - A long-lived **AI worker** subscribes via `BLPOP` (blocking pop)
- Ensures real-time, event-driven flow with no polling or delays

### 🤖 AI Worker + RAG
- Pops new article ID → fetches from MongoDB
- Generates **summaries + insights** using **LLM (OpenAI)**
- Creates **embeddings** of enriched content
- Stores:
  - Merged raw + AI content in **processed MongoDB**
  - Embeddings in **Qdrant vector DB**
- Avoids duplicate inserts

### 🔍 Semantic Search with RAG
1. User query → converted to embedding
2. **Qdrant** returns top similar documents
3. Results + query → passed to **LLM**
4. LLM returns a **context-rich, smart summary**

### 🔧 FastAPI Backend
- REST API endpoints:
  - `/news` – Paginated news listing
  - `/domains`, `/categories` – Metadata views
  - `/search` – RAG-based semantic query interface

### 🎨 Frontend (React – In Progress)
- Filter by domain/category
- Display AI summaries
- RAG-powered smart search bar
- Modern, responsive UI

## 🧠 Tech Stack
- **Scraping**: Async Python, Celery, Beat, Docker
- **Queueing**: Redis (List + BLPOP)
- **Storage**: MongoDB (Raw + Enriched)
- **Search**: Qdrant Vector DB
- **AI**: OpenAI for summarization
- **Backend**: FastAPI
- **Frontend**: React (WIP)

## 🎯 End Goal
Deliver **summarized, relevant, and searchable news** in real-time using a scalable AI-first architecture—**no noise, just insights**.
