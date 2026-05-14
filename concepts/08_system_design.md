# RAG system design

## Components at a high level?

A RAG system is more than "retrieve then generate". Core components include:

- **Ingestion pipeline** - Collect, clean, and chunk data. 
- **Indexing layer** - Build and maintain vector/ lexical/ structured indexes. 
- **Retriever service**- Expose retrieval via an API (often with multiple strategies).
- **Generator service**- Wrap the LLM with prompts, templates and safety checks. 
- **Orchestration**- Glue logic that coordinates retrieval and generation (and possibly tools).
- **Monitoring and evaluation**- Logs, metrics, and tests for quality and reliability. 

Thinking explicitly about these components helps avoid fragile "single script" setups.

## Key design questions

When designing a RAG system, I ask:

- ** Data and domain **
    - What types of documents do we have (PDFs, tables, logs, emails)?
    - How fast does the corpus change?

- **Retrieval strategy**
    - Vector, vectorless or hybrid?
    - How will I handle metadata filters( time range, source, language)?

- **Latency and scale**
    - How many queries per second?
    - How fresh do rsults need to be after documents are updated?

- **Explanability and traceability**
    - Do users need to see citations or links back to source snippets?
    - Are there compliance or audit requirements?

- **Failure modes**
    - What happens if retrieval returns nothing?
    - What happens when the model is down or times out?

## Common design patterns
Some patterns I keep in mind:

- **Thin client, thick backend**
    UI is simple; most logic (retrieval, generation, routing) lives in backend services. 

- **Central retrieval service**
    One service that handles all indexing and retrieval, used by multiple apps (chatbot, search, dashboards).

- **Hybrid retrieval with routing**
    A small router decides which retriever or prompt to use based on query type or user role. 

- **Offline-heavy ingestion**
    Most heavy work (parsing, chunking, embedding) runs offline or in batch jobs, so online queries are fast. 

## Deployment and operations:
Even small RAG systems benefit from basic ops practices:

- **Versioning**
    Track versions of indexes, embedding models, and prompts so you can reproduce behaviour.

- **Monitoring**
    - Latency, error rates and retrieval hit rates.
    - Distribution of query types and common failure patterns.

- **Feedback loops**
    - Collect user feedback or thumbs-up/down.
    - Use this data to refine your test set and evaluation metrics. 

My intent with this file is to treat RAG as a system design problem, not just a notebook trick: you know, something that can be reasoned about with components, trade-offs, and clear responsibilities.'