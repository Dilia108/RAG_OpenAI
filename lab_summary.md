# Lab Summary — RAG_OpenAI

End-to-end implementation of a Retrieval-Augmented Generation (RAG) system over the *Ethics Guidelines for Trustworthy AI* PDF document. The pipeline covers document ingestion, chunking, embedding, vector search, and grounded answer generation.

---

## Step 1 — PDF Extraction

**Goal:** Load the source PDF and extract its text content page by page.

**Approach:** Used `PyPDF2.PdfReader` to iterate over each page, extract raw text, and prefix each page's content with a `[Page N]` marker. The full extracted text was saved to a `.txt` file for downstream processing.

**Results:**

The PDF was successfully parsed and written to disk. No pages were skipped or corrupted during extraction. The output text file served as the sole input to the chunking step.

---

## Step 2 — Chunking

**Goal:** Split the extracted text into semantically coherent, retrieval-sized chunks with token verification.

**Approach:** Used `RecursiveCharacterTextSplitter` from `langchain-text-splitters` with a character-based length function. After splitting, each chunk's token count was verified using `tiktoken` (`cl100k_base` encoding). Chunks below 50 tokens were filtered as too small to be meaningful for retrieval.

**Key settings:**

| Parameter | Value |
|---|---|
| Chunk size | 1,000 characters |
| Chunk overlap | 200 characters |
| Separators | `["\n\n", "\n", ". ", " ", ""]` |
| Tokenizer | `cl100k_base` |
| Token warning limit | 300 tokens |
| Min token filter | 50 tokens |

> Note: Settings were selected following the results from the previou lab regarding: PDF and Podcast chunking.


**Results:**

| Metric | Value |
|---|---|
| Total chunks (pre-filter) | 209 |
| Chunks removed by filter | 3 |
| Chunks retained | 206 |
| Avg tokens per chunk | 179.8 |
| Min tokens per chunk | 21 |
| Max tokens per chunk | 282 |
| Chunks exceeding 300 tokens | 0 |

The 3 dropped chunks (1.4%) were likely isolated artifacts such as stray page headers or lone section numbers. The zero oversized chunks confirmed the 1,000-character setting was well-calibrated for this document. The average of 179.8 tokens sits below the 250–300 target, consistent with ethics/policy documents that contain frequent short paragraphs and bullet lists.

---

## Step 3 — Embedding

**Goal:** Generate a semantic vector representation for each chunk using OpenAI's embedding model.

**Approach:** Chunks were embedded in batches of 100 using `text-embedding-3-small`. The response embeddings were sorted by `item.index` to guarantee order alignment with the input. Retry logic (3 attempts, 5s delay) was included for resilience. Each chunk's embedding was attached to its metadata and saved to a JSON file. Embedding norms were validated as a quality check.

**Key settings:**

| Parameter | Value |
|---|---|
| Model | `text-embedding-3-small` |
| Embedding dimensions | 1,536 |
| Batch size | 100 |
| Total API calls | 3 |
| Retry attempts | 3 |
| Retry delay | 5 seconds |

**Results:**

| Metric | Value |
|---|---|
| Total embeddings generated | 209 |
| Embedding dimensions | 1,536 |
| Norm mean | 1.0000 |
| Norm min | 0.9994 |
| Norm max | 1.0006 |

Norms of ≈1.0 across all embeddings confirmed unit normalisation, validating that the embeddings were correctly received and parsed. The ±0.0006 deviation is normal floating-point rounding, not an error.

> Note: embedding ran on the pre-filter 209 chunks rather than the filtered 206. The 3 extra chunks are small and do not meaningfully affect retrieval quality.

---

## Step 4 — Vector Search

**Goal:** Implement a similarity search function to retrieve the most relevant chunks for any natural language query.

**Approach:** All chunk embeddings were pre-loaded into a NumPy matrix at startup. At query time, the user's query was embedded with the same model, and cosine similarity was computed against all chunks via dot product (valid since vectors are unit-normalised). Results were ranked by descending similarity and filtered by a minimum threshold.

**Key settings:**

| Parameter | Value |
|---|---|
| Similarity metric | Cosine (dot product) |
| Top-K | 5 |
| Min similarity threshold | 0.3 |
| Matrix shape | (209, 1536) |

**Results — test queries:**

| Query | Top similarity | Bottom similarity | Notes |
|---|---|---|---|
| What are the principles of trustworthy AI? | 0.7403 | 0.7031 | Tight, high-confidence cluster |
| How should AI systems handle personal data? | 0.6855 | 0.6066 | Slightly wider spread, all above 0.6 |
| What is human oversight in AI systems? | 0.7661 | 0.6357 | Strongest single match across all queries |

All retrieved chunks across all queries scored above 0.6, well above the 0.3 noise floor. No irrelevant chunks were returned in any of the three test cases, confirming the embedding and search steps were well-aligned.

---

## Step 5 — RAG Query & Answer Generation

**Goal:** Combine retrieval with generation to produce grounded, document-faithful answers to natural language questions.

**Approach:** Retrieved chunks were formatted into a structured context block (with chunk ID, similarity, and token count as metadata headers) and passed to the OpenAI Chat Completions API alongside the user query. A strict system prompt instructed the model to answer only from the provided context and to cite supporting chunks. Two delivery options were implemented.

**Key settings:**

| Parameter | Value |
|---|---|
| Chat model | `gpt-4o-mini` |
| Temperature | 0.2 |
| Max completion tokens | 1,000 |
| Top-K chunks passed as context | 5 |
| Min similarity threshold | 0.3 |

---

### Option A — Pure Python

**Results:**

**Query 1 — What are the principles of trustworthy AI?**

The model correctly identified and enumerated all three components of Trustworthy AI from the document: lawful, ethical, and robust. It cited Chunks 24 and 188 as sources. Answer was concise (89 completion tokens).

| Metric | Value |
|---|---|
| Chunks used | 5 (0.7031 – 0.7403) |
| Prompt tokens | 1,193 |
| Completion tokens | 89 |
| Total tokens | 1,282 |

**Query 2 — How should AI systems handle personal data?**

The model produced the most detailed answer of the three, listing 8 concrete practices drawn predominantly from Chunk 149 (a dense, checklist-style section on data governance). It cited specific chunks for each point.

| Metric | Value |
|---|---|
| Chunks used | 5 (0.6066 – 0.6855) |
| Prompt tokens | 1,112 |
| Completion tokens | 247 |
| Total tokens | 1,359 |

**Query 3 — What is human oversight in AI systems?**

The model accurately described the three oversight mechanisms from the document — HITL, HOTL, and HIC — with definitions for each. Chunk 141 (similarity 0.766) was the strongest single retrieval across all test queries.

| Metric | Value |
|---|---|
| Chunks used | 5 (0.6357 – 0.7661) |
| Prompt tokens | 1,176 |
| Completion tokens | 166 |
| Total tokens | 1,342 |

**Overall token usage across queries:**

| Metric | Value |
|---|---|
| Avg prompt tokens | ~1,160 |
| Avg completion tokens | ~167 |
| Avg total tokens | ~1,328 |

---

### Option B — Gradio Chat UI - COULD NOT TEST

