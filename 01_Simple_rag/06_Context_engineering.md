# Context Engineering and Assembly in RAG

In production RAG, retrieval quality is not only half the story. The other half is what actually enterns the model's context window. Even with perfect retrieval, poor context assembly(bad ordering, redundancy, over-long prompts) will tank answer quality, latency and cost. 

This section covers the "context engineering assembly" layer that sits between retrieval and the final prompt:
- Selecting and ordering chunks
- Deduplication and conflict handling
- Token budgeting and truncation
- Context compression (summarization and pruning)
- Prompt construction patterns

1. **Role of the Context Assembly Layer**
The context assembly layer takes a raw set of retrieved chunks and turns it into a tighly controlled prompt segment that is:
- Relevant: contains only content useful for the current query.
- Dense: high information per token, low redundancy.
- Structured: consistently ordered and labeled so the LLM can "scan" it effectively. 

In other words, retrieval answers "what could be useful?", while context assembly answers "what exactly do we show the model, in what order and how compactly?"

2. **Chunk Selection Ordering**
After initial retrieval, most systems apply a re-ranking and ordering pass. This often matters more than the choice of vector store. 

Common practices:
    - Re-rank by relevance:
        - Use a cross-encoder or reranker model to score each chunk against the queryand keep only the top-K.
        - This usually beats "top-K by cosine similarity" alone and allows more agressive compression downstream.

    - Order by "narrative usefulness"
        - Within the selected set, order chunks in a consistent way the model can exploit, for example:
            - By reranker score descending (most relevant first)
            - Then, within each source document, by original document order to preserve local coherence. 
        - For procedural or time series data, ordering chronologically can reduce contradictions.

    - Group by source
        - Many systems group chunks by document and label them with titles, URLs, or section headers. This helps the LLM attribute fatcs and resolve conflicts. 

A simple heuristic that works well in practice:
    1. Retrieve top N chunks (dense + sparse).
    2. Re-rank with a cross-encoder. Keep top N.
    3. Group by document ID and sort each group by original position. 
    4. Concatenate groups in descending order of best chunk score. 

3. **Deduplication and Conflict Resolution**
Raw retrieval commonly returns near-duplicates or overlapping sections, especially using sliding window chunking or hybrid retrieval. 
Useful strategies:
- Exact/ near-exact deduplication:
    - Normalize content (lowercase, strip whitespace/ markup) and remove exact duplicates.
    - For near-duplicates, hash sentences or use a cheap embedding and drop chunks above a similarity threshold. 

- Overlap handling
    - If chunks are created with overlap, re-merge overlapping segments at assembly time, or keep only the "central" chunk for each region.

- Conflict surfacing
    - Prefer the most recent source (metadata-based), or
    - Include both, but label clearly ("Source A (2021)", "Source B ( 2024)") so the LLM can reason about recency. 

Goal is to maximise unique, non-overlapping information while keeping the number of chunks small. 

4. **Token Budgeting**
The context assembly layer should treat tokens like a hard resource budget, not an afterthought.
Typical pattern:
- Define explicit budgets:
    - Total context window (ex. 32k tokens).
    - Reserved tokens for:
        - System + tool instructions
        - User query + history
        - Output headroom (some models degrade if the input nearly fills the window)
    - The remaining capacity is the "context budget" available for retrieved content. 

- Estimate cost per chunk:
    - Precompute approximate token counts for each chunk at indexing time, or tokenize on the fly. 
    - When assembling, walk ordered chunks in sequence and include them until the budget is nearly exhausted. 

- Priortize by marginal value
    - Ask: "If I add this chunk, how much new information does it bring per token?"
    - Favor high relevance, high novelty chunks. Drop loew relevance or redundant ones. 

As enforce_token_budget(chunks, budget) step that takes ordered chunks and returns a pruned subset is a core part of production RAG codebases.

5. **Context Compression Techniques**
When even the best assembly cannot fit everything, compression is the next line of defense.

Key approaches:
- Extractive compression:
    - Pick important sentences or spans from each chunk based on scoring model, TF-IDF, or attention weights.
    - Works well when wording matters (eg. legal, policy) and you want to preserve original phrasing.
- Abstractive summarization
    - Use an LLM to rewrite each chunk or group of chunks into a shorter summary while preserving key facts. 
    - Can be query-agnostic (offline summaries attached to each chunk) or query-aware (on-the-fly summaries conditioned on the user query).

- Token-level pruning
    - Techniques such as LLMLingua and similar methods remove low-information tokens based on model perplexity or attention scores, keeping key tokens and discarding filler.
    - Particularly useful when you need high compression ratios without fully re-summarizing everything.

- Hierarchical compression:
    - For very long contexts, compress in stages:
        - Summarize long documents into section summaries offline.
        - At query time, retrieve and compress only the relevant sections further if needed.

In practice, many production systems combine reranking + light extractive compression and only fall back to heavy abstractive summarization for edge-case queries.

6. Prompt Construction and Layout
Once chunks are selected, deduplicated, and compressed, they must be inserted into the final prompt in a consistent, model-friendly format.

Good patterns:
- Separate instructions from context:
    - Keep system and task instructions at the top. Keep-retrieved context in its own clearly marked section (ex. "Context:" with bullet-pointed or numbered documents).
- Label and segment context
    - Prefix each chunk with metadata such as source title, date, or section and separate chunks with clear delimiters. 
    - This makes it easier for the LLM to attribute facts adn to answer questions like "which document syas this?".
- Explicit grounding instruction
    - Include constraints like "Answer using only the context above. If the answer is not present, say you dont know." to reduce hallucinations. 
- Stable schema
    - Keep the structure of the prompt stable across requests (same headings and ordering) so that evaluation and optimization of the context layer is meaningful.

7. Putting it together as a pipeline
A typical production RAG context assembly pipleine looks like:
    - Retrieve an initial candidate set (dense + sparse).
    - Re-rank candidates with a cross-encoder.
    - Deduplicate and merge overlaps. 
    - Enforce token budget (drop low-value chunks).
    - Compress remaining context if needed (extractice, abstractive, or token-level).
    - Build the final prompt with a clear, stable, layout and metadata labels. 

Each of these steps is observable and tunable. They can be A/B tested and evaluated with metrics like answer relevance, faithfulness and "token per correct answer".