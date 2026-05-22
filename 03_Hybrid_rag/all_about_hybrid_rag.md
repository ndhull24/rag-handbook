## What it is?
Hybrid RAG is a Retrieval-Augmented Generation approach that combines **two retrievals styles**:
- Keyword-based search: Looks for exact or near-exact terms
- Semantic search: looks for meaning

**Hybrid RAG tires to get the best of both**

## Why it exists?
Plain RAG can miss useful information when the question uses different wording than the source text, and keyword-only search can miss meaning when the user's words are vague or paraphrased. Hybrid RAG exists to reduce those misses by balancing exact matching with meaning-based matching, which usually makes answers more acccurate and more complete.

## How it works?
A user asks a question, and the system sends that question to two retrieval paths at the same time: one path searches by keywords, and the other searches by embeddings or vectors that capture meaning. The results are then merged, filtered, or reranked so the most useful documents are passed to the language model for answer generation. 

In simple terms, it is like asking two librarians for help at once: one who is great at finding exact words in books, and one who is great at finding ideas that mean the same thing even when the wording changes.

## Architecture
A common Hybrid RAG architecture includes:
- User question input.
- Query processing, such as normalization or rewriting.
- Sparse retrieval, usually keyword search like BM25.
- Dense retrieval, usually vector or embedding search.
- Fusion or merging step, often ranking-based.
- Optional reranking step for better precision.
- Context assembly.
- LLM generation.

The important idea is that Hybrid RAG does not depend on only one retrieval method. It keeps two different retrieval signals and combines them so one method can cover the blind spots of the other.

## Strengths:
Hybrid RAG is strong because it handles both exact wording and conceptual meaning. That makes it useful when users ask questions in unpredictable ways or when documents contain technical terms, names, codes, or policy language that must match exactly.

It also tends to improve retrieval quality, reduce missed context, and make the final answer more reliable. In many real systems, this can lower the chance of giving a confident but wrong answer because the model sees a broader and better-ranked set of source passages.

## Weaknesses:
Hybrid RAG is more complex than a simple vector-search RAG setup because it maintains two retrieval systems instead of one. That means more engineering work, more tuning, and usually more latency than a single retrieval path.

It can also become harder to explain and debug because you have to understand how the keyword side, the semantic side, and the merge step are interacting. If the fusion logic is poor, the system may still surface weak results even if both retrieval methods are individually good.

## When to use it?
Use Hybrid RAG when your data contains both natural language and exact terminology, such as technical documentation, legal text, medical content, internal company knowledge, product manuals or support articles. It is also a strong choice when users may phrase the same question in many different ways.

It is especially useful when:
- Exact terms matter, like product names, error codes, clause numbers, or policy IDs.
- Semantic similarity matters, like paraphrases or concept-level questions.
- You want stronger retrieval than vector search alone.
- Your knowledge base has mixed document types and mixed query styles.

## When not to use it?
Do not use Hybrid RAG first if your problem is very small or your content is simple enough that one retrieval method already works well. If your users ask short, obvious questions and your documents are clean and consistent, the extra complexity may not be worth it.

It is also less attractive when:
- You need extremely low latency.
- You do not have the resouces to tune retrieval quality.
- Your data is too small to benefit from dual retrieval.
- You are still experimenting and want the simplest possible prototype.

## Implementation notes
A good implementation usually starts with a sparse retriever such as BM25 and a dense retriever based on embeddings. The two set of results are then combined using a fusion strategy, and Reciprocal Rank Fusion is a common approach because it is robust when score scales are different.

### Practical tips include:
- Normalize queries before search.
- Use metadata filters when relevant.
- Over-retrieve a little, then rerank.
- Keep document chunks meaningful and not too large.
- Track which retrieval path contributed each result so debugging is easier.

A beginner-friendly way to think about it is: keyword search catches the exact words, semantic search catches the meaning, and the merge step decides which results deserve to be shown ot the model. 

## Evaluation
Hybrid RAG should be evaluated at two levels:
- Retrieval- Checks whether the right documents are being found
- Generation- checks whether the final answer is correct, grounded, and useful.

Useful metrics and checks include:
- Recall of relevant documents.
- Precision of top-ranked results.
- Answer faithfulness to the retrieved context.
- Latency
- Cost
- Human judgement on whether the answer actually solves the question.

## Example use case:
Imagine a customer support assistant for a software company. A user asks, "How do I fix error 0x47394 when installing the app?". Keyword search is great because it can match the exact error code, while semantic search helps if the user describes the problem differently, such as "the app fails because of a certificate issue". Hybrid RAG combines both so the assistant is more likely to find the right help article and explain the solution clearly.

This makes Hybrid RAG especially useful in support and knowledge-base systems, where users may either quote exact terms or describe the same problem in everyday language.

## Related concepts:
Hybrid RAG is closely related to:
- Basic RAG, which usually uses one retrieval method.
- Naive RAG, which is the simplest retrieve-then-generate setup.
- Dense retrieval, which uses embeddings to find semantically similar content.
- Sparse retrieval, which uses keyword matching such as BM25.
- Hybrid search, the retrieval strategy that combines dense and sparse search.
- Reranking, which improves the ordering of retrieved results.
- Advanced RAG, which often includes hybrid retrieval as one of its techniques.

A simple way to remember it is: **Hybrid RAG = keyword search + semantic search**. That combination makes it easier for beginners to understand and easier for real systems to retrieve the right evidence.