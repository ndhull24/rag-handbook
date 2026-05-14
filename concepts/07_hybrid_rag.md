#Hybrid RAG

## What is hybrid RAG?

Hybrid RAG combines more than one retrieval strategy in a single system.

Common combinations are:
- Vector + Keyword (BM25 + embeddings)
- Vector + structure-aware (eg. search over a treee/ graph)
- Multi-stage retrieval: fast and coarse filter to a slower and more accurate retrieval. 

The goal is to get the precision of vectorless/ structured methods and the recall of vector-based methods.

## Patterns I find useful

1. **Parallel retrieval + re-ranking**
    - Run a vector search and a keyword/ lexical search in parallel. 
    - Merge or re-rank results using a scoring function or a small LLM. 
    - Good when some queries are highly lexical (IDs) and others are fuzzy. 

2. **Two-stage retrieval**
    - Stage 1: cheap filter (keyword, metadata, date ranges, source type).
    - Stage 2: vector search over the filtered subset.
    - This is great for big corpora when you can narrow the search space quickly. 

3. **Stucture-first, semantic-second**
    - Use structure (sections, document types, entities) to pick candidate documents.
    - Within those, run vector retrieval for relevant paragraphs.
    - Useful for policies, specs, financial reports, or hierarchically organized docs. 

## Design questions you want the RAG to answer:

- How do I decide which retriever to weight more for a given domain?
- How should relevance scoring work when mizing different retrieval signals?
- How do I keep hybrid retrieval maintainable as data and business rules change?