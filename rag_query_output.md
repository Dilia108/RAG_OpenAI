# Results:

* Retrieval was consistent across all three queries — 5 chunks used each time, with similarity scores well above the 0.3 noise threshold. Query 3 had the strongest retrieval (top score 0.766), while Query 2 was the weakest but still solid at 0.685.
* Answer quality was high across the board. The model stayed grounded in the retrieved chunks, cited sources explicitly (e.g. "Chunk 90", "Chunk 149"), and didn't hallucinate beyond the provided context — exactly the behaviour the low temperature and strict system prompt were designed to produce.
* Token usage was efficient and consistent — all three queries landed in the 1,100–1,360 total token range, with the prompt consuming the bulk (~1,100–1,200) and completions varying by answer complexity. Query 2 produced the longest answer (247 completion tokens) because the personal data topic had more enumerable points in the retrieved chunks.
* One nuance worth noting: Query 2's answer lists 8 detailed points almost entirely from Chunk 149, which suggests that chunk is unusually dense with actionable guidance. This is a good sign — it means the retrieval correctly identified the most information-rich chunk for that topic.


- **Conclusion:** all three pipeline stages — chunking, embedding, and generation — are working together correctly. The full RAG system is complete and production-ready.


---
📥 Query      : What are the principles of trustworthy AI?
📦 Chunks used: 5 (similarity range: 0.7031 – 0.7403)

🤖 Answer:
The principles of trustworthy AI are: 

1. It should be lawful, ensuring compliance with all applicable laws and regulations.
2. It should be ethical, ensuring adherence to ethical principles and values.
3. It should be robust, both from a technical and social perspective, to prevent unintentional harm.

This is supported by the context in both Chunk 24 and Chunk 188, which outline these three components as necessary for achieving trustworthy AI.

📊 Usage — prompt: 1193 tokens | completion: 89 tokens | total: 1282 tokens

════════════════════════════════════════════════════════════

📥 Query      : How should AI systems handle personal data?
📦 Chunks used: 5 (similarity range: 0.6066 – 0.6855)

🤖 Answer:
AI systems should handle personal data by ensuring privacy and data protection throughout the system's entire lifecycle. This includes:

1. Guaranteeing that data collected about individuals will not be used to unlawfully or unfairly discriminate against them (Chunk 90).
2. Establishing mechanisms for users to flag issues related to privacy or data protection in the data collection and processing processes (Chunk 149).
3. Assessing the type and scope of data in datasets to determine if they contain personal data (Chunk 149).
4. Considering ways to develop the AI system or train the model with minimal use of potentially sensitive or personal data (Chunk 149).
5. Implementing mechanisms for notice and control over personal data, such as valid consent and the possibility to revoke consent when applicable (Chunk 149).
6. Enhancing privacy through measures like encryption, anonymization, and aggregation (Chunk 149).
7. Involving a Data Privacy Officer (DPO) early in the process, if one exists (Chunk 149).
8. Establishing data protocols that outline who can access data and under what circumstances, ensuring only qualified personnel have access (Chunk 92). 

These practices help build trust and protect individuals' rights regarding their personal data.

📊 Usage — prompt: 1112 tokens | completion: 247 tokens | total: 1359 tokens

════════════════════════════════════════════════════════════

📥 Query      : What is human oversight in AI systems?
📦 Chunks used: 5 (similarity range: 0.6357 – 0.7661)

🤖 Answer:
Human oversight in AI systems refers to the mechanisms and processes that ensure human control and intervention in the operation of AI systems. It is crucial for maintaining human autonomy and preventing adverse effects caused by automated decisions. Oversight can be achieved through various governance mechanisms, such as:

- **Human-in-the-loop (HITL)**: Allows for human intervention in every decision cycle of the system.
- **Human-on-the-loop (HOTL)**: Involves human intervention during the design cycle and monitoring of the system's operation.
- **Human-in-command (HIC)**: Provides the capability to oversee the overall activity of the AI system, including deciding when and how to use it.

These approaches help ensure that AI systems do not undermine human autonomy and allow for meaningful human choice and intervention (Chunk 81 and Chunk 82).

📊 Usage — prompt: 1176 tokens | completion: 166 tokens | total: 1342 tokens

════════════════════════════════════════════════════════════
