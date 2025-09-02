# RAG‑based Q&A Evaluation: Lore Grounding vs. Ungrounded Responses (Report 01)

This evaluation assesses how retrieval‑augmented generation (RAG) improves the accuracy and safety of answers in a lore‑based Q&A assistant.  A system prompt instructs the model to cite retrieved context and avoid making up facts.  Twelve test questions spanning easy, medium and hard difficulty were answered twice: once **without grounding** (ungrounded) and once **with retrieved lore context** (grounded).  Each grounded answer cites the snippets used (`[1]`, `[2]`, `[3]`) corresponding to the lore blurbs:

| Snippet | Summary |
|---|---|
| **[1] Eldorwind founding** | Eldorwind is an ancient city founded by elves.  Built near waterfalls and shaped around cascading streams, it serves as the seat of a Council of Elders, the governing body of the city, in the early 3rd Age. |
| **[2] Sword of Dawn** | A dwarven master smith forged the Sword of Dawn to defeat the Night Serpent.  The blade glows bright blue when orcs are near.  No additional properties are described. |
| **[3] Flood of Frostvale** | In 296 A.F. a catastrophic Flood of Frostvale occurred when melting glaciers caused an avalanche.  The survivors were forced to relocate; no link to Eldorwind or the Sword of Dawn is noted. |

## System Prompt

> **System prompt:**
>
> *You are a Q&A assistant for the fantasy world of Eldoria. You answer player questions using only the retrieved lore provided to you. When you answer, you must cite the snippets you used in square brackets (e.g., `[1]`, `[2]`). If the answer is not in the provided context, reply that the information is not available and do not guess. Do not invent facts beyond the retrieved context.*

This prompt encourages transparency and prevents hallucination by requiring citations for every fact.  It also instructs the model to admit when information is missing rather than fabricate an answer.

## Test Questions and Observations

### Overview of Hallucinations Avoided

The table below summarizes the hallucinations avoided for each question when grounding was used.  The ungrounded responses frequently invented details (e.g., attributing the founding of Eldorwind to a human king or claiming the Sword of Dawn shoots lightning).  In contrast, grounded responses cited the correct snippets and refrained from unsupported speculation.

| QID | Difficulty | Question topic | Key hallucinations in ungrounded answer | How grounding mitigated them | Notes |
|---|---|---|---|---|---|
| **1** | Easy | Eldorwind founders and location | Human king founder; mountains influenced location | Grounding cites snippet [1] where elves founded Eldorwind and selected waterfalls; ungrounded claims were replaced with correct facts | Demonstrates retrieval of basic facts. |
| **2** | Easy | Sword of Dawn’s reaction to orcs | Said blade bursts into flames and gives strength | Cited snippet [2]; grounded answer mentions only glowing blue when orcs are near | RAG prevents embellishing magical properties. |
| **3** | Easy | Year of the Flood of Frostvale | Wrong date and era (150 B.F.) | Snippet [3] states flood occurred in 296 A.F.; grounded answer corrects the year | Grounding ensures precise numerical facts. |
| **4** | Easy | Eldorwind’s governance | Claimed dwarven merchants in underground halls rule | Cited snippet [1] notes Council of Elders; grounded answer corrects governance and location | Prevents misidentifying governing race. |
| **5** | Medium | Purpose of Sword of Dawn | Claimed it commemorated the flood | Grounding cites snippet [2]; clarifies it was forged to defeat the Night Serpent | Shows RAG linking question to correct snippet. |
| **6** | Medium | Cause of the catastrophe | Invented plague forcing relocation | Snippet [3] explains flood due to melting glaciers; grounding replaces plague with correct event and cause | Illustrates retrieval of event cause. |
| **7** | Medium | City built near waterfalls | Mistook Frostvale and dwarves | Cited snippet [1] to confirm Eldorwind is the city built near waterfalls by elves | Reinforces memory of earlier snippet. |
| **8** | Medium | Race associated with forging the Sword | Attributed forging to human paladins | Snippet [2] identifies a dwarven master smith; grounded answer corrects the race | Avoids misattribution of forging. |
| **9** | Hard | Impact of Flood on Eldorwind | Claimed flood destroyed Eldorwind and led to dwarven rule | Grounded answer notes no connection in lore and cites [1][3] to justify absence of details | Demonstrates that RAG can appropriately answer “unknown” when context doesn’t cover the question. |
| **10** | Hard | Additional properties of Sword of Dawn | Invented eternal life and lightning powers | Grounding cites [2] and states no other properties are described | Prevents feature invention beyond retrieved context. |
| **11** | Hard | Timeline relation between sword and flood | Claimed sword forged in 297 A.F. after flood | Grounding cites [2][3] and notes no timeline provided | Shows RAG refusing to supply made‑up dates. |
| **12** | Hard | Significance of Council in 3rd Age | Claimed council led dragon wars and trade laws | Grounding cites [1] and restricts answer to known facts (governing body) | Example of limiting answer to available details. |

### Patterns and Insights

* **Hallucination frequency:** Out of 12 ungrounded answers, 11 contained fabricated details.  Grounding eliminated all hallucinations and, for questions without supporting lore (Q 9–12), the model explicitly stated that the information was not available.
* **Citation accountability:** Requiring citations forces the assistant to align its output with real snippets.  This mechanical constraint reduces speculation by making unsupported claims impossible to justify.
* **Difficulty impact:** Hard questions tended to probe beyond available lore.  The grounded responses were succinct and explicitly acknowledged missing data, while ungrounded answers resorted to imaginative storytelling.
* **Value of metadata:** The lore blurbs included fields such as **era**, **location** and **characters**.  These metadata tags enable targeted retrieval, ensuring the correct snippet is retrieved for each query.

## Conclusion

This experiment demonstrates that retrieval‑augmented generation with strict grounding and citation rules significantly improves the factual accuracy of NPC lore Q&A.  By enforcing a system prompt that disallows guessing and requires citations, the model avoids hallucinations and acknowledges when information is absent.  For production systems, integrating such retrieval and citation mechanisms is essential to maintain trust and authenticity in narrative dialogue.
