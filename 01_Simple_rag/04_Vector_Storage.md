# Vector storage

## What is a vector store?

A vector store (or vector database) is the component that stores embeddings and lets you search them efficiently.

It is responsible for:
- Persisting vectors and their metadata (IDs, source docs, chunk positions, timestamps).
- Running similarity search to find nearest neighbors for a query embedding.
- Handling indexing strategies so search stays fast as data grows. 

In most RAG systems, the vector store is the **data layer** behind semantic retrieval. 

## Vector store vs embeddings
Embeddings answer "how do we represent text?", while vector storage answers "how do we store and search those representations at scale?"

- Embedding model: Turns text into vectors.
- Vector store: indexes and searches those vectors.
- Retriever: orchestrates queries to the evctor store and returns matching chunks.

You can chaneg one without alwayss changing the others, but big changes (like switching embedding dimensions) usually requires re-indexing. 

## Types of vector stores and deployments:

In practice, vector storage shows up in three broad forms:
- **In-memory or file-based libraries**:
    - Ex. FAISS, HNSWLib, simple numpy arrays + custom code. 
    - Good for prototypes, local apps, and smaller datasets. 

- **Standalone/ vector databases:**
    - Ex. Qdrant, Weaviate, Pinecone, Milvus, ChromaDB, etc
    - Provide APIs, scaling, persistence and features like filtering and multi-tenancy.

- **Hybrid/ embedded in existing databases:**
    - Traditional databases (Postgres, MYSQL, etc) with vector extensions or hybrid stores.
    - Useful when you want vectors and relational data managed together. 

Your choice affects the operational complexity, cost and how you express filters and metadata queries.

## Indexing strategies inside a vector store

A raw vector list with linear search works only for small corpora. For larger collections, vector stores use indexing techniques for Approximate Nearest Neighbor (ANN) seach. 

- **Flat/ brute-force index**
    - Exact search over all vectors.
    - Simple, but per query. FIne for tens of thousands of vectors.

- **IVF (Inverted file)/ clustering based indexes:**
    - Cluster vectors and search only nearby clusters.
    - Faster but approximate. Useful for millions of vectors.

- **HNSW (Hierarchical Navigable Small World)**
    - Graph-based index with layered small-world graphs. 
    - Excellent recall/latency trade-off and widely used for production search.

The core idea: **indexes trade a bit of exactness for speed**, so you can search millions or billions of vectors interactively. 

## Why metadata lives with vectors?

Each stored vector is usually tied to:

- A **chunk ID** and source document reference. 
- **Text** for that chunk or a pointer to where the text is stored. 
- **Metadata**: title, section, document type, timestamps, tags or user IDs. 

Metadata enables hybrid queries like:

- "Top 10 similar chunks from the last 90 days."
- "Only from these document types or business units."
- "Exclude deprecated versions."

Most vector databases support metadata filters alongside similarity search.

## CRUD, multi-latency and lifecycle

In real systems, vector storage is not static:
- You need to **add** new content as documents are created.
- You need to **update or delete** vectors when documents change or users revoke access. 
- You may need per-user or per-tenant indexes for isolation and privacy. 

Production setups often combine:
- A vector index for embeddings.
- A separate metadata store (eg. relational/ NoSQL) to track document/ tenant relationships and support deletion, retention policies, and access control. 

## How I think about choosing a vector store

When deciding how to store vectors for a RAG project, I ask:

- **Scale**- Roughly how many chunks and how fast queries need to be. 
- **Operation contraints**- DO I want a manager service, or can I run my own?
- **Metadata complexity**- How complex are filters and access rules?
- **Ecosystem fit**- Which store integrates cleanly with my stack (LangChain, existing DBs, cloud)?

For a small local or portfolio project, an in-process library like FAISS with on-disk saving is usually enough. 
For multi-tenant or large-scale RAG, a dedicated vector database with proper indexing, filtering and lifecycle management becomes worth the extra complexity.