# Few‑Shot A/B Test – Compact vs Rich Prompting

This experiment evaluates how the number of few‑shot examples impacts both **quality** and **token usage**.  We compare a **3‑shot prompt** (A) with a **1‑shot prompt** (B) on five tasks.  Each prompt contained identical instructions and examples; only the number of illustrative examples differed.

## Experimental Setup

- **Model:** GPT‑3.5‑Turbo (September 2024), temperature 0.3, top‑p 0.9.
- **Tasks:** five short prompts spanning summarisation, classification and transformation.  Ground‑truth expectations were defined manually.
- **Evaluation criteria:** correctness (does the answer satisfy the instruction), fluency and style adherence.  Each response was rated **1–5** by a human evaluator.
- **Token counting:** OpenAI API usage logs were used to record prompt and completion tokens for each run.

## Prompt Designs

### A: Few‑Shot 3

The instruction was followed by **three** diverse examples demonstrating the input–output pattern.  Total prompt length averaged **260 tokens**.

### B: Few‑Shot 1

The same instruction was followed by **one** example.  Total prompt length averaged **140 tokens**.

## Results

| Task | Description | Quality (A) | Quality (B) | ΔQuality | Prompt tokens | Completion tokens | Token savings |
|---|---|---|---|---|---|---|---|
| **1** | Summarise a 2‑sentence news item | 5 / 5 | 4 / 5 | +1 | 262 (A), 142 (B) | 28 / 30 | **120 tokens** saved, minor drop in nuance |
| **2** | Categorise sentiment of a tweet | 4 / 5 | 4 / 5 | 0 | 259 (A), 137 (B) | 12 / 12 | **122 tokens** saved, equal accuracy |
| **3** | Rewrite a sentence in medieval style | 5 / 5 | 3 / 5 | +2 | 264 (A), 140 (B) | 36 / 33 | Rich prompt captured archaic diction better than single example |
| **4** | Extract named entities from a sentence | 4 / 5 | 4 / 5 | 0 | 258 (A), 138 (B) | 15 / 15 | Single example sufficed for a structured task |
| **5** | Convert bullet points into a short paragraph | 5 / 5 | 4 / 5 | +1 | 257 (A), 139 (B) | 42 / 41 | Multi‑shot improved coherence |

### Observations

- **Quality impact:** The 3‑shot prompt consistently matched or exceeded the 1‑shot prompt.  For style‑heavy tasks (Task 3 and Task 5), the multi‑shot prompt produced noticeably better outputs.
- **Cost impact:** On average, the 3‑shot prompt used **120 more prompt tokens** per call.  Completion tokens were similar across setups.  Over thousands of calls this difference magnifies.
- **Diminishing returns:** Some tasks (Tasks 2 and 4) saw no quality improvement from additional examples, suggesting that a single well‑chosen example is sufficient for deterministic extraction.

## Recommendations

1. **Tailor the number of examples to task complexity.** Use a single high‑quality example for structured classification/extraction; employ 2–3 examples for generative or stylistic tasks.
2. **Measure token cost per call.** A small quality gain may not justify a large token overhead, especially at scale.  Consider caching high‑cost examples or rotating them.
3. **Iterate with users.** Gather feedback on responses to determine whether the quality benefits of additional examples translate into user satisfaction.

By thoughtfully balancing few‑shot count against quality requirements, teams can reduce inference costs without compromising the user experience.
