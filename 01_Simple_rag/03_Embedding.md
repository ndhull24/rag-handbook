# Embeddings in RAG

## What are embeddings?

Embeddings are vector representations of text that capture semantic meaning. 
Each chunk and each query is mapped to a vector in a high-dimensional space, and similarity in that space approximates semantic similarity.

In a RAG pipeline, embeddings are the bridge between raw text and vector search:
- We embed chunks during indexing.
- We embed user queries at runtime.
- We compare the query vector to chunk vectors to find relevant passages.

## Why embeddings matter?

Embeddings strongly influence:
- What the retriever considers "similar".
- How robust your system is to paraphrasing and synonyms.
- How well the system generalizes across topics and writing styles.

A good embedding setup:
- Uses the same model for chuncks and queries.
- Handles your domain vocabulary reasonably well.
- Is stable over time (you avoid re-embedding too often without reason)

## Types of embedding relevant to RAG

- **General-purpose text embeddings**: Trained on broad corpora, work well for generic documentation and Q&A.
- **Domain-specific embeddings**: Tuned or specialized for code, legal, financial or scientific text, improving relevance in those domains.
- **Sparse vs dense representations**: Dense embeddings are typical in RAG. Sparse (term-based) representations overlap with vectorless/lexical retrieval and can also be combined in hybrid systems. 

## Basic embedding workflow in RAG

1. **Preprocess text**: Clean up obvious noise (headers, footers, boilerplate) after chunking so embeddings reflect meaningful content.
2. **Embed chunks**: For each chunk, compute an embedding and store it with the chunk metadata in a vector index.
3. **Embed queries**: At query time, embed the user question using the same embedding model.
4. **Similarity search**: Use cosine similarity, dot product, or other distance measures to find top_k closest chunks.
5. **Use retrieved chunks in generation**: Pass the original text of those chunks (not the vectors) to the LLM as context.

## Practical considerations:

- **Model choice**: Pick an embedding model that balances quality, speed and cost for your scale and domain. 
- **Dimensionality and index**: Higher dimensions can capture more nuanace but cost more in storage and retrieval. Vector databases use different indexing algorithms (ex. HNSW, IVF) to keep search fast at scale.
- **Re-embedding**: Changing embedding models mid stream usually means re-embedding your corpus. This can be a significant maintenance event. 

My rule of thumb: solid chunking + a decent embedding model + a reasonable index get you most of the way. After that, domain-specific tweaks and hybrid retrieval matter more than endlessly swapping embedding models.