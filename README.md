# RAG with OpenAI (Native Implementation) LAB

Complete RAG pipeline using OpenAI's native APIs. The system chunks a pdf processed document, embeds it with OpenAI, stores vectors locally, and retrieves grounded answers via GPT-4o-mini.

---

## Prerequisites

- The source PDF placed in the project root:
  ```
  ethics_guidelines_for_trustworthy_ai.pdf
  ```

---

## Main

- The mail file to run: **rag_openai_native.ipynb**

*Steps:*
#    Cell 1 → PDF extraction
#    Cell 2 → Chunking + filtering -> Results:** 206 chunks · avg 179.8 tokens · max 282 tokens · 0 oversized chunks
#    Cell 3 → Embedding -> Results:** 209 embeddings · norms mean 1.0000 · min 0.9994 · max 1.0006
#    Cell 4 → Vector search functions -> review file: vector_search_output.md
#    Cell 5 → RAG query (Option A ) -> review file: rag_query_output.md 
---

## File & Folder Map

```
project-root/
│
├── ethics_guidelines_for_trustworthy_ai.pdf        ← Source document (you provide this)
│
├── ethics_guidelines_for_trustworthy_ai_text.txt   ← Step 1 output: raw extracted text
│                                                      (~N pages, [Page N] prefixed)
│
├── ethics_guidelines_chunks.json                   ← Step 2 output: chunked text
│                                                      Array of objects:
│                                                      { chunk_id, char_count,
│                                                        token_count, text }
│
├── ethics_guidelines_embeddings.json               ← Step 3 output: chunks + vectors
│                                                      Array of objects:
│                                                      { chunk_id, char_count,
│                                                        token_count, text,
│                                                        embedding: [1536 floats] }
│
└── rag_openai_native.ipynb                          ← Main notebook (all 5 steps)
```

---

## Key Results

| Metric | Value |
|---|---|
| Total chunks | 206 (filtered) / 209 (embedded) |
| Avg tokens per chunk | 179.8 |
| Embedding dimensions | 1,536 |
| Embedding norm (mean) | 1.0000 |
| Typical similarity range | 0.60 – 0.77 |
| Avg total tokens per RAG query | ~1,330 |
| Token budget (prompt / completion) | ~1,160 / ~170 |
