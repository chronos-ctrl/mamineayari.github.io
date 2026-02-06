---
title: "Building NewsChrono: A Short-Form News Platform Powered by LLMs"
date: 2025-04-16T18:51:00-00:00
categories:
  - blog
tags:  
  - llm
  - embeddings
  - fastapi
  - cloudrun
---

*Published on {{ page.date | date: "%B %d, %Y" }}*

<a href="https://newschrono.com" target="_blank" rel="noopener noreferrer"><b>newschrono.com</b></a>
is a French short-form news platform I designed and developed end-to-end — from ideation and product design to backend architecture and production deployment.

The core idea behind NewsChrono is simple:  
> help users stay informed through **concise, readable news summaries**, while still enabling discovery of **related topics and context**.

Behind this simplicity sits a production-grade architecture that combines **LLMs, semantic embeddings, serverless infrastructure, and scalable backend services**. This article walks through the product vision, technical choices, and system design.

---

## 1. Product Vision: Short-Form, Not Shallow

Traditional news platforms overwhelm users with:
- long articles,
- redundant coverage,
- noisy feeds.

NewsChrono focuses on:
- **short, high-signal summaries**,
- fast consumption,
- semantic navigation across related stories.

The goal is not to replace journalism, but to **optimize access to information** for users who want to understand *what matters* quickly.

---

## 2. LLM-Based News Summarization

At the heart of NewsChrono is **automatic article summarization**.

Each full-length news article is processed using:
- **OpenAI LLM APIs**,
- prompt-engineered summarization logic,
- consistent output formatting.

The summarization pipeline:
1. Ingests the raw article
2. Applies an LLM-based summarization prompt
3. Produces a concise, readable summary
4. Stores both raw and summarized content

This approach ensures:
- consistent tone and length,
- language clarity,
- scalability across sources and topics.

Summarization is treated as a **backend service**, not a frontend feature.

---

## 3. Semantic Similarity: Finding Related Articles

Short-form content is more powerful when paired with **contextual discovery**.

To achieve this, each article is represented using:
- **LLM-generated embeddings**,
- a dense vector capturing semantic meaning.

### Embedding Pipeline
For each article:
1. Generate an embedding using an LLM embedding model
2. Store the embedding vector in the database
3. Use it as a semantic descriptor of the article

### Similarity Search
When displaying an article, NewsChrono retrieves:
- the **4 most related articles**,
- based on **cosine similarity** between embeddings.

This is implemented using:
- **Firebase `find_nearest`**
- cosine distance as the similarity metric

The result is a recommendation system based on *meaning*, not keywords or categories.

---

## 4. Backend Services: Systemd-Managed Workers

Both:
- article summarization,
- and embedding generation + similarity indexing

are handled by a **dedicated backend service**, running as a **systemd-managed process**.

This design choice provides:
- isolation from the web frontend,
- reliable background processing,
- controlled retries and monitoring.

The backend service is responsible for:
- calling OpenAI APIs,
- managing embeddings,
- updating Firebase records,
- triggering similarity indexing.

This separation keeps the system **robust and maintainable**.

---

## 5. Web Application Architecture

The NewsChrono web application is built as a:
- **FastAPI application**
- fully **Dockerized**
- deployed on **Google Cloud Run**

### Why FastAPI
- clean API design,
- async-friendly,
- easy integration with backend services.

### Why Cloud Run
- serverless scaling,
- zero infrastructure management,
- cost efficiency,
- fast iteration cycles.

Cloud Run allows the platform to scale automatically with traffic, without over-provisioning resources.

---

## 6. Firebase for Data Management

**Firebase** is used as the main data backend, handling:
- article storage,
- summaries,
- embeddings,
- metadata,
- similarity queries.

Key benefits:
- managed infrastructure,
- tight integration with Cloud Run,
- native support for vector similarity queries.

Firebase acts as both:
- a traditional database,
- and a lightweight semantic index.

---

## 7. End-to-End Data Flow

Putting it all together:

1. News article is ingested
2. Backend service summarizes it using an LLM
3. Embedding is generated and stored
4. Article is indexed for similarity search
5. Web app fetches summary + related articles
6. User consumes concise news with semantic context

Each component is loosely coupled but clearly defined.

---

## 8. From Ideation to Production

NewsChrono is not a prototype — it is a **complete product lifecycle**:
- idea and positioning,
- system design,
- LLM integration,
- backend services,
- cloud deployment,
- production operation.

Building it end-to-end required combining:
- product thinking,
- AI engineering,
- backend architecture,
- and cloud infrastructure.

---

## Closing Thoughts

NewsChrono demonstrates how **LLMs can be productized responsibly**, not just experimented with.

By combining:
- summarization,
- semantic embeddings,
- scalable backend services,
- and serverless deployment,

the platform turns raw news streams into **structured, digestible information**.

This project reflects a broader belief:  
> AI is most powerful when it quietly improves how people access and understand information.
