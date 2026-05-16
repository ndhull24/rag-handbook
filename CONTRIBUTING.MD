# Contributing to RAG Handbook

Thanks for your interest in improving the RAG Handbook! This project is a living notebook of RAG concepts, architectures, and trade‑offs, with an emphasis on real‑world analytics and finance use cases. Contributions that make the ideas clearer, more accurate, or more practical are especially welcome.

## How to contribute:
There are several easy ways to contribute:

- Fix typos, grammar, or unclear wording in the concept notes
- Add small examples, diagrams, or checklists for existing sections
- Propose new concepts or patterns that fit the current outline
- Improve structure, cross‑linking, or references between notes

If you are unsure whether an idea fits, feel free to open an issue first and start a short discussion.

## Project structure:
The repo is organized as:

- README.md: high‑level overview and index
- concepts/: core RAG concepts (short, opinionated notes)
- patterns/: concrete patterns and case studies (currently being filled)
- Each concept file is meant to be readable in isolation and also as part of a larger sequence.
- When adding or editing content, please:
- Keep notes focused and concise rather than exhaustive
- Favor practical trade‑offs and design choices over theory for its own sake
- Use headings and short paragraphs so both humans and LLMs can parse the content easily

## Workflow for contributions
- Fork this repository to your own GitHub account.
- Create a branch for your change:
  - Example: feat/context-assembly, docs/fix-typos-embeddings, etc.
  - Make your edits in the relevant Markdown files under concepts/ or patterns/.
  - Preview the Markdown locally (or via GitHub’s preview) to ensure formatting looks good.
- Commit with a clear message, for example:
  - docs: add context assembly section
  - docs: clarify hybrid rag definition

Open a Pull Request against main with a short description of:
- What you changed
- Why it improves the handbook
- Any open questions or follow‑ups you’re proposing
- Please keep each PR focused on one theme (e.g., one concept file or one new pattern) so it is easy to review.

Style guidelines to keep the handbook coherent:
- Tone: conversational, pragmatic, and opinionated (explain trade‑offs and “it depends” cases)
- Audience: practitioners who already know basic LLMs and want to design or debug RAG systems
- Formatting:
  - Use ##, ### headings to structure ideas
  - Prefer short paragraphs and bullet points
  - Use inline code for APIs and config names (e.g., top_k, hybrid_search)

Examples:
- Favor realistic, production‑flavored scenarios (analytics, finance, internal tools)
- When mentioning code or pseudo‑code, keep it minimal and language‑agnostic unless necessary
- If you add new terminology or acronyms, define them briefly the first time they appear.

## Adding new concepts or patterns
When proposing a new concept or pattern:
- Check the existing concepts/ index to avoid overlaps in scope.
- Briefly explain in the PR description where this fits in the mental model of the RAG pipeline.
- If helpful, add a short “Related reading” section at the bottom that links to other concept files.

For new pattern notes under patterns/, try to cover:
- Problem context and constraints
- Data / document characteristics
- Retrieval and context‑assembly design decisions
- Evaluation considerations and common pitfalls

## Issues and discussions
If you run into ambiguities, missing topics, or have ideas for larger structural changes:
- Open an Issue with a clear title and bullet‑point description
- Optionally propose a rough outline or example of the change you have in mind
- Tag issues with labels like docs, concept, pattern, or question (once labels exist)

## Code and notebooks (future)
As the project evolves, some concept notes may link to example notebooks or small reference implementations. For code contributions:
- Keep examples small and self‑contained
- Prefer readability and pedagogy over micro‑optimizations
- Document how the example relates back to specific concept files
