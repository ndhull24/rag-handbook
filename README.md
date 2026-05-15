# RAG Handbook

A practical handbook where I document everything I know about Retrieval-Augmented Generation (RAG): concepts, architectures, and design trade‑offs, with an emphasis on real-world analytics and finance use cases.

👉 Live notebook-style version of this handbook:  
[https://raghandbook-dtla9uep.manus.space](https://raghandbook-dtla9uep.manus.space)

---

## Project overview

This repo is not a typical “app” but a structured knowledge base of RAG concepts.  
Each note is written to be readable by humans and also easy for LLMs to retrieve as reference material.

Right now, the focus is on:

- Building a clean mental model of the full RAG pipeline.
- Capturing trade-offs between different retrieval and storage choices.
- Laying the foundation for future, code-backed patterns and examples.

---

## Folder structure

```text
rag-handbook/
├── README.md
├── concepts/
│   ├── 01_rag_basics.md
│   ├── 02_chunking_and_indexing.md
│   ├── 03_embeddings.md
│   ├── 04_vector_storage.md
│   ├── 05_evaluation.md
│   ├── 06_vector_vs_vectorless.md
│   ├── 07_hybrid_rag.md
│   └── 08_system_design.md
└── patterns/
    └── (coming soon)
```

- `concepts/` holds the core theory and design notes.
- `patterns/` will contain concrete application patterns and case studies (e.g., finance assistant, policy copilot) once they are written.

---

## Concepts index

Each concept file is intentionally short and opinionated, so you can read it in isolation or as a sequence.

- `01_rag_basics.md`  
  High-level introduction to RAG: what it is, when to use it, and the standard ingestion → indexing → retrieval → generation pipeline.

- `02_chunking_and_indexing.md`  
  How and why we chunk documents, common chunking strategies, and how indexing choices affect retrieval behavior.

- `03_embeddings.md`  
  What embeddings are, why they matter in RAG, how they are generated and used, and practical considerations around model choice and re-embedding.[web:43][web:46]

- `04_vector_storage.md`  
  Vector stores and indexes: how embeddings are stored and searched, types of vector databases, indexing strategies (flat, IVF, HNSW), and metadata handling.[web:52][web:55]

- `05_evaluation.md`  
  Evaluation mindset for RAG: separating retrieval from generation, key metrics (recall@k, precision@k, faithfulness), and a lightweight evaluation loop you can run locally.[web:37][web:44]

- `06_vector_vs_vectorless.md`  
  Comparing vector-based retrieval with lexical / structure-aware (vectorless) approaches, their strengths/weaknesses, and when to favor each.

- `07_hybrid_rag.md`  
  Hybrid retrieval patterns (vector + keyword, multi-stage retrieval, structure-first then semantic), and design questions for mixing signals.[web:14][web:17]

- `08_system_design.md`  
  Treating RAG as a system: ingestion, retrieval services, generator, orchestration, monitoring, and the operational questions you need to answer for real deployments.[web:42][web:45]

You can read them in order or jump directly to the topic you care about.

---

## Live site (Manus)

This handbook is also published as a Manus site:

- URL: [https://raghandbook-dtla9uep.manus.space](https://raghandbook-dtla9uep.manus.space)
- Purpose: act as a browsable “notebook” version of these notes, and a place to experiment with AI-assisted editing and publishing.

The GitHub repo is the source of truth; the Manus site mirrors the content and will evolve alongside it.[web:60]

---

## Roadmap

Short-term:

- Fill in the first pattern in `patterns/` (e.g., a finance RAG assistant pattern).
- Add simple, language-model-friendly diagrams / examples to concept files.
- Link any future code examples back to the relevant concept notes.

Later:

- Add evaluation notebooks and small test sets.
- Document concrete hybrid RAG architectures and scaling/ops lessons.
- Integrate more tightly with the Manus deployment (and GitHub) workflow.[web:60]

---

## How to use this repo

- As a **personal notebook**: clone it, tweak the notes, and turn them into your own RAG handbook.
- As **prep material**: re-read selected concepts before interviews or RAG design discussions.
- As a **design aid**: when you build a new RAG system, use the concepts index as a checklist of decisions you need to make.
