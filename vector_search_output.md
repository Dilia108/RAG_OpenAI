# Results: 
All three queries returned highly relevant chunks — the retrieved text visibly matches what each query is asking about. The pipeline is working correctly end to end.

Similarity score obtained:
* > 0.7 Strong match — the chunk is very likely relevant
* 0.5 – 0.7Good match — related and useful

* **Query 1** (trustworthy AI principles): Ranks 1–2 both score ~0.74, with nearly identical scores — two chunks from different parts of the document that define the same concept. Clean retrieval.
* **Query 2** (personal data): Scores range 0.69 → 0.61, a slightly wider spread but still all above 0.6. Rank 5 at 0.6066 is the weakest but still contextually relevant.
* **Query 3** (human oversight): Rank 1 at 0.766 is the strongest single match across all queries — the chunk literally contains "Human oversight:" as a heading, a near-perfect hit.

**Conclusion:** scores consistently above 0.6 with no noise retrievals below 0.3 means your chunking, embedding, and search are all well-calibrated.


Loaded 209 chunks | Matrix shape: (209, 1536)

Query: 'What are the principles of trustworthy AI?'
────────────────────────────────────────────────────────────
[Rank 1] Chunk  24 | Similarity: 0.7403 | Tokens: 201
entire life cycle.  
Trustworthy  AI has three components , which should be met throughout the system's entire life cycle :  
1. it should be lawful , complying with  all applicable laws and regulations ; 
2. it should be ethical , ensuring adherence to  ethical principles and values;  and  
3. it s...

[Rank 2] Chunk 188 | Similarity: 0.7387 | Tokens: 205
confidently and fully reap its benefits when the technology, including the processes and people behind the 
technology, are trustworthy. When drafting these G uidelines, Trustworthy  AI has, therefore, been our foundational 
ambition.  
Trustworthy  AI has thre e components: (1) it should be l awful...

[Rank 3] Chunk  40 | Similarity: 0.7172 | Tokens: 191
[Page 11]
9 
 I. Chapter I: Foundations of Trustworthy  AI  
This Chapter sets out the foundations of Trustworthy  AI, grounded in fundamental rights and reflected by four 
ethical principles that should be adhered to in order to ensure ethical and robust AI. It draws heavily on the field of 
ethics...

[Rank 4] Chunk 128 | Similarity: 0.7080 | Tokens: 204
capabilities and limitations, enabling realistic expectation setting, and about the manner in which the 
requirements are implemented. Be transparent about the f act that they are dealing with an AI system.  
 Facilitate the traceability and auditability of AI systems, particularly in critical cont...

[Rank 5] Chunk  13 | Similarity: 0.7031 | Tokens: 212
stakeholders  are aware of and trained in Trustworthy  AI. 
 Be mindful that there  might be fundamental tensio ns between different principles and requirements . 
Continuously i dentify, evaluate, document and communicate these trade -offs and their solutions . 
III. Chapter III provid es a concre...


Query: 'How should AI systems handle personal data?'
────────────────────────────────────────────────────────────
[Rank 1] Chunk  90 | Similarity: 0.6855 | Tokens: 187
protocols and the capability to process data in a manner that protects privacy .  
Privacy and data p rotection . AI systems must guarantee privacy and data protection throughout a system’s entire 
lifecycle .41 This includes the information initially provided by the user, as well as the information...

[Rank 2] Chunk  91 | Similarity: 0.6751 | Tokens: 206
will not be used to unlawfully or unfairly discriminate against them .  
Quality and integrity of d ata. The quality of the data sets used is paramount to the performance of AI systems. 
When data is gathered, it may cont ain socially constructed biases, inaccuracies, errors and mistakes. This needs...

[Rank 3] Chunk 149 | Similarity: 0.6381 | Tokens: 219
3. Privacy and data governance  
Respect for privacy and data Protection:  
 Depending on the use case , did you establish a mechanism  allowing others to flag issues related to 
privacy or data p rotection in the AI system’s processes  of data collection (for training and operation) 
and data proc...

[Rank 4] Chunk  92 | Similarity: 0.6162 | Tokens: 131
not), d ata protocols governing data access should be put in place. These protocols should outline who can access 
data and under which circumstances. O nly duly qualified personnel with the competence and need to access 
individual’s data should be allowed to do so.  
                              ...

[Rank 5] Chunk  72 | Similarity: 0.6066 | Tokens: 187
always prefer public sector data to personal data). Reference can also be made to the proportionality between user and 
deployer, considering the rights of companies (includ ing intellectual property and confidentiality) on the one hand, and the rights 
of the user on the other.  
32  Including by u...


Query: 'What is human oversight in AI systems?'
────────────────────────────────────────────────────────────
[Rank 1] Chunk 141 | Similarity: 0.7661 | Tokens: 214
between the AI system and humans for meaningful interactions and appropriate human oversight and 
control?  
 Does the AI system enhance or augment human capabilities?  
 Did you take safeguards to prevent overconfidence in or overreliance on the AI system for work 
processes?  
Human oversight:  ...

[Rank 2] Chunk  81 | Similarity: 0.7398 | Tokens: 209
which may threaten individual autonomy . The o verall principle of user autonomy  must be central to the system ’s 
functionality. Key to this is the right not to be subject to a decision based solely on automated processing when this 
produces legal effects on users or similarly significantly  affe...

[Rank 3] Chunk  78 | Similarity: 0.6580 | Tokens: 179
The above requirements include elements that are in some cases already reflected in existing laws. We reiterate 
that – in line with Trustworthy  AI’s first component – it is the responsibility of AI practitioners to ensure that they 
comply with their legal obligations, both as regards horizontally...

[Rank 4] Chunk  82 | Similarity: 0.6432 | Tokens: 199
system’s operation. HIC refers to the capability to oversee the overall activity of the AI system (including its broader 
economic, societal, legal  and ethical impact) and the ability to decide when and how to use the system in any 
particular situation. This can include the decision not to use an ...

[Rank 5] Chunk  62 | Similarity: 0.6357 | Tokens: 195
determination over themselves, and be able to partake in the d emocratic process . AI systems should not 
unjustifiably subordinate, coerce, deceive, manipulate, condition or herd humans. Instead,  they  should be designed 
to augment, complement and empower human cognitive, social and cultural skil...

