# Evaluating RAG systems

## Why evaluation is different for RAG

RAG has two moving parts:
- **Retrieval**- Are we pulling the right chunks?
- **Generation**- Is the answer grounded, relevant, and well-structured?

Good evaluation separates these components so you know what to fix:
- If retrieval is weak, tune chunking/ indexing/ retrievers.
- If retrieval is strong but answers are bad, tune prompts or the model. 

## Core evaluation dimensions

Typical RAG evaluation covers:
- **Context relevance**
    Are the retrieved chunks actually about the user's questions?

- **Context sufficiency**
    Do the chunks contain enough information to answer correctly.

- **Answer relevance**
    Does the answer directly address the question?

- **Answer correctness/ hallucination**
    Is the answer factually correct given the provided context?

- **Faithfullness**
    Does the answer stay within the retrieved evidence rather than inventing facts?

## Retrieval metrics

When you have a labelled test set (questions + expected relevant chunks), you can use:

- **Recall@k**- Fraction of questions where at least one relevant chunk appears in the top k results

- **Precision@K**- Fraction of retrieved chunks in the top k that are actually relevant.

- **Ranking quality**- Whether the most relevant chunks are ranked near the top. 

These metrics help you compare:
- Different chunk sizes/ overlaps.
- Different retrivers (vector vs keyword vs hybrid).
- Different embedding models.

## Generation metrics

For answers, you can mix human review and automated checks:

- **Human scoring** on a small but high quality test set(eg. 1 to 5 for correctness, clarity and usefulness).
- **LLM-as-a-judge** to approximate human labeling at scale (with careful prompt design).
- **Custom metrics** based on your domain:
    - "No hallucinated tickers" for finance.
    - "No unsafe medical claims" for health.
    - "Cites at least one source snippet" for explanability.

## Practical evaluation loop I'd follow:

1. **Create a small test set**
    20-100 representative questions from the domain, with expected answers and, if possible, known relevant documents.

2. **Evalue the retrieval**
    Inspect retrieved chunks and compute simple metrics (recall@k, precision@k) to ensure relevant evidence is found.

3.  **Then evaluate the generation**
    Use a checklist: grounded, correct, clear, complete. Optionally, add an LLM judge to speed things up.

4. **Track a few key metrics over time**
    Each time you change chunking, retrieval, or prompts, re-run tests and compare. Focus on a small, stable set of metrics instead of chasing everything. 

My goal is to keep the evaluations realistic: small, targeted tests that fit on the device, but that still reveal whether a change actually improved the system. 