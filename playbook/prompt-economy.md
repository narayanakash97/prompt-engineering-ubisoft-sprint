# Prompt Economy Guide

Production systems using large language models often need to balance **quality**, **latency** and **cost**.  Prompt design plays a key role because every additional token increases latency and API cost.  This guide outlines principles and trade‑offs to help optimise prompts.

## 1 Context Length: Short vs Long

- **Short prompts (few hundred tokens)** are cheaper and faster.  They are well suited to straightforward classification or extraction tasks where the model requires little context.  However, short prompts may lack domain grounding, leading to generic outputs and hallucinations.
- **Long prompts (≥1 k tokens)** provide more grounding through detailed instructions, examples and context paragraphs.  They improve quality on complex tasks such as reasoning or narrative generation.  The downside is increased token usage which raises costs and can hit context limits on some models.
- **Recommendation:** start with the minimal context needed for the model to understand the task.  Add supporting information only where quality gains justify the extra cost.

## 2 Few‑Shot Count vs Quality

Few‑shot prompting uses examples to illustrate the desired behaviour.  The number of examples (shots) influences both quality and cost:

| Few‑shot count | Pros | Cons |
|---|---|---|
| **Zero‑shot** | Lowest token cost; simple to maintain | Quality varies; may misunderstand edge cases |
| **1–2 shots** | Significant quality improvement over zero‑shot with modest cost; clarifies format | May still miss some nuances |
| **3 or more shots** | Better generalisation and reduced variance; helps with style adherence | Higher token usage; diminishing returns beyond ~5 shots |

- **Recommendation:** use **1–3 shots** for most tasks.  Evaluate whether adding more examples materially improves quality.  For classification or structured extraction, a single high‑quality example often suffices.

## 3 Temperature and Top‑p Tuning for Consistency

Sampling parameters control the randomness of the model’s outputs.  Appropriate tuning can reduce the need for retries while maintaining diversity:

- **Temperature:** scales the logits before sampling.  A lower temperature (<0.5) produces more deterministic outputs, while a higher temperature (>0.8) encourages variation.  In production, set temperature between **0.2–0.5** for consistent responses.
- **Top‑p (nucleus sampling):** limits sampling to the most probable tokens whose cumulative probability ≤ *P*.  Lowering top‑p (e.g. 0.9) reduces extreme outliers and hallucinations.

- **Recommendation:** combine a moderate temperature (0.3–0.5) with a top‑p cutoff (0.9–0.95) for reliable yet non‑repetitive answers.  For deterministic tasks like data extraction, temperature can be set to **0**.

## 4 Caching and Prompt Reuse Strategies

Reducing duplicate work is an effective way to save tokens:

- **Reusable system prompts:** define a single system prompt containing policies, style instructions and safety guidelines.  For each conversation, send this prompt once and cache it.  Only send user messages afterwards.  In stateful APIs (e.g. ChatGPT), the system prompt can persist across calls.
- **Context caching:** when the same knowledge context is used repeatedly (e.g. static product descriptions), store embeddings and retrieved passages locally.  Send only the relevant excerpts rather than re‑retrieving the entire document.
- **Response caching:** cache high‑value responses (e.g. FAQ answers) keyed by user intent.  If the same question appears, return the cached answer without invoking the model.

- **Token budgeting:** instrument your pipeline to log prompt and completion token counts.  Identify frequent high‑cost prompts and optimise them (e.g. removing redundant words or trimming examples).

## 5 Putting It Together

Balancing quality and cost is an iterative process:

1. **Define metrics:** decide what “quality” means for your use case (accuracy, tone adherence, factuality).  Establish acceptable latency and cost budgets.
2. **Baseline:** start with a concise zero‑ or one‑shot prompt and moderate sampling parameters.  Measure quality and token usage.
3. **Incrementally adjust:** add examples, context or tune temperature/top‑p one at a time.  Evaluate improvements against additional token cost.
4. **Cache smartly:** reuse system prompts and context wherever possible.  Monitor for repetitive queries that can be served from a cache.

Prompt economy is not about minimising tokens at all costs; it is about **spending tokens where they matter most**.
