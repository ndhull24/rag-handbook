## Chunking and indexing

# Why chunking matters?

Most of the source documents (PDFs, web pages, reports) are too long and heavy to send directly to an LLM.
You can send them but:
1. It will take alot of time
2. It will not be able to take the context into account

Hence, we moved to chunking of the documents. 
Chunking means splitting the documents into smaller pieces that are:
- Big enough to preserve the context 
- Small enough to fit into the prompts and be retrieved efficiently

# Chunking is an important step.

Bad chunking leads to:
- Lost context (chunks too small are disconnected and too big are not able to manage the context) also called context stuffing. 
- Can also lead to higher cost or redundant chunks. 

## Common chunking strategies include:
- **Fixed-size chunks** - eg. 500 to 1000 words/tokens with an overlap of 50-200 words/ tokens.
- **Structure-aware chunks** - break by headings, sections, bullet lists, or table rows, etc. 
- **Semantic chunks**- use models to detect the natural topic boundaries. 

In practice, I start simple with fixed-size and overlap. I only introduce structure/ semantic chunking when I see clear retrieval or answer-quality issues. 

## Indexing the chunks 
Once the content is chunked, we build an index so we  can efficiently look up relevant chunks at query time. 

There are 2 broad indexing families:
- **Vector indexes**- store embeddings of each chunk and use similarity search (eg. FAISS, other vectors DBs (ChromaDB, PineCone, etc))
- **Non vector indexed**/ Also called vectorless- kyeword/ lexical/ BM25 style indexes, or more structured trees/graphs over sections. 

Index structure and retrieval strategy are coupled: how you index the data strongly influences which questions your system will answer well. 