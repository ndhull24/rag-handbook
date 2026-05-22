## What is it?
Corrective RAG, often called **CRAG**, is a RAG method that does not blindly trust retrieved documents. It adds a checking step before generation so the system can judge whether the retrieved context is good enough, fix weak retrieval, and then answer with cleaner evidence.

In simple terms, it is RAG with a built-in "quality-control" layer. Instead of sending whatever was retrieved straight into the model, CRAG first evaluates the retrieval and corrects it when needed.

## Why it exists?
Standard RAG can fail when the retriever brings back irrelevant, incomplete, or misleading documents. If the model is given bad context, it may still sound confident while producing a wrong answer.

CRAG exists to reduce that risk by catching weak retrieval early, improving the context, and making the final generation more reliable. This is especially important in high-stakes domains where mistakes are costly, such as finance, legal, or medical workflows.

## How it works?
CRAG usually starts like normal RAG: a user asks a question, the system retrieves documents, and the pipeline prepares to generate an answer. The difference is that CRAG inserts a retrieval evaluator that scores how good the retrieved documents are for the query.

If the retrieved context looks strong, the system can process with generation or refine the context lightly. If it looks weak, CRAG can trigger corrective actions such as retrieving more documents, expanding the search to the web, reranking the results, or filtering out irrelevant passages.

A useful way to think about it is:
- Retrieve.
- Check quality.
- Correct weak context.
- Generate the answer.

## Architecture
A typical CRAG architecture includes:
- User query input.
- Initial retriever over a knowledge base.
- Retrieval evaluator or confidence scorer.
- Correction or refinement branch.
- Optional web search or external retrieval.
- Document filtering or reranking.
- Decompose-then-recompose step for cleaning mixed evidence.
- Final generation model.

The important design idea is that retrieval is not a single one-shot step. CRAG treats retrieval as something that can be inspected and corrected before the LLM ever sees it.

## Strengths:
CRAG is strong because it makes the system robust when retrieval quality is uneven. It can detect poor evidence, reduce hallucinations caused by bad context, and improve trust in the final answer.

It is also useful because it is plug-and-play in many architectures, meaning it can often be added on top of an existing RAG system instead of requiring a complete redesign. the original paper describes it as compatible with various RAG-based approaches.

## Weaknesses:
CRAG adds extra steps, so it can increase latency and implementation complexity. You now need a retrieval evaluator, a correction strategy, and rules for when to keep, expand, or replace context.

It can also be harder to tune than basic RAG because the evaluator itself must be reliable. If the evaluator is weak, it may reject good context or accept bad context, which reduces the benefit of the whole system.

## When to use it?
Use CRAG when retrieval errors are a major source of bad answers and you want the system to self-check before generating. It is a strong fit for production systems where accuracy matters and where you cannot afford to let weak retrieval pass through silently.

It is especially helpful when:
- The knowledge base is noisy or incomplete.
- Queries are ambiguous or difficult.
- You need strong factual grounding.
- The system may need to fall back to web search or broader retrieval.
- You want a more reliable version of a standard RAG pipeline.

## When not to use it?
Do not reach for CRAG first if your retrieval is already simple, clean, and highly accurate. If your use case is small or low-risk, the extra evaluator and correction logic may be more complexity than value.

It is also less attractive when:
- Latency must be extremely low.
- You do not have a good way to judge retrieval quality.
- Your corpus is tiny and easy to search.
- The added engineering effort would outweigh the improvement.

## Implementation notes:
A practical CRAG system often uses a lightweight evaluator to estimate whether the retrieved documents are relevant, sufficient, and trustworthy. That evaluator can trigger different paths depending on confidence: keep, refine, expand, or replace.

Useful implementation patterns include:
- Use confidence thresholds to decide when to correct retrieval.
- Retrieve from the local knowledge base first, then expand to web search if needed.
- Rerank candidate passages before generation.
- Remove irrelevant text using a decompose-then-recompose strategy.
- Log evaluator decisions so you can debug false positives and false negatives.

A beginner-friendly mental model is: standard RAG says “find documents and answer,” while CRAG says “find documents, inspect them, fix them if needed, and then answer.”

## Evaluation:
CRAG should be evaluated on both retrieval quality and answer quality, but also on how well the corrective layer behaves. You want to know not only whether the final answer is better, but also whether the system correctly detected weak retrieval in the first place.

Useful evaluation measures include:

- Retrieval relevance.
- Final answer accuracy.
- Faithfulness to source documents.
- Hallucination rate.
- Correction precision and recall.
- Latency and cost impact.
- Success rate when retrieval is intentionally noisy.

## Example use cases:
Imagine a finance assistant answering internal policy questions. A user asks about an unusual expense rule, and the initial retrieval returns an outdated policy plus one irrelevant travel note. A CRAG system can notice that the retrieval is weak, fetch a better policy section, remove the noisy passage, and then generate a more accurate answer.

That makes CRAG especially helpful in settings where the difference between “kind of relevant” and “actually correct” matters a lot.

## Related concepts

CRAG is closely related to:
- Standard RAG, which retrieves context and then generates directly.
- Advanced RAG, which adds retrieval improvements like reranking and query rewriting.
- Hybrid RAG, which combines keyword and semantic retrieval.
- Retrieval evaluation, which scores the quality of retrieved evidence.
- Web augmentation, which adds external search when local documents are insufficient.
- Context assembly, which packages the final evidence for the model.

A simple way to remember it is: **CRAG is RAG with a correction checkpoint**. It helps the system notice bad retrieval before the model commits to a bad answer.