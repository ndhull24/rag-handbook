## What is RAG?
- RAG stands for 'Retrieval Augmented Generation (RAG).
- It is a way to answer questions or perform taks by combining a language model with an external knowledge source.
- Instead of relying only on parameters, the model retrieves relevant documents or snippets at query time and uses them as additional context when generating an answer.

## What it really means:
    - It is a process to optimize the output of LLMs. 
    # How?
        - Traditionally, LLMs are trained on a vast amount of data. This data is good for the training purpose but it is not trained on your company's or office's data via which you want the LLM to generate answers. 
        - RAG come in to the picture here by giving the LLM a base from which it can get all the answers, information for the questions asked you or the people. 
        - Ex. A chatbot specifically designed for your company's website. 
        - LLM is like getting the book for the test and RAG is getting to know that questions will be asked from which chapter.
        - RAG is an extension of the powers of LLMs to specific domains or on an organization’s internal knowledge base, without the need for retraining the model. 

## RAG is useful when:
- You need a model that needs up-to-date or domain-specific knowledge.
- You want traceability, that is, being able to show where an answer came from.
- You want to avoid retraining a model every time your data changes.

## High-level pipeline:
I like to think of a RAG system as four stages:

1. **Ingestion**: Collect raw data (PDFs, web pages, CSVs, slides, internal docs, and so on)
2. **Chunking and indexing**: Break the content into pieces and building an index, so that we can search easily.
3. **Retrieval**: Given a user query, finding the closest/ most relevant chunks.
4. **Generation**: Feeding the question and retrieved chunks into the LLM to produce the answer. 
(More on these in the upcoming parts)

At a system level, we mostly tune:
- How we chunk and index the data.
- How we retrieve (vector, keyword, hybrid).
- How we prompt the model and structure the final answer.

## Why the hype of RAG/ the use cases
- Question and answering over documentation (internal wikis, APIs, policies)
- Domain copilots (finance, legal, customer support)
- Search-then-summarize workflows for research

## The core idea is to separate the knowledge storage from the reasoning, so that we can allow both the segments to perform as effectively as possible. 