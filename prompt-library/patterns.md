# Prompt Engineering Pattern Handbook

This handbook catalogues reusable prompt patterns for production use. Each entry describes when the pattern is useful, a general template, a working example, and typical failure points.  These patterns can be combined and extended depending on your use‑case.  At the end you will find general constraint templates that can be layered on top of any prompt to enforce style, safety or length.

---

## 1. Zero‑shot Prompting

### Use‑case
Use a zero‑shot prompt when you want a language model to perform a task **without providing any examples**.  Zero‑shot prompts work well for straightforward tasks like translations, summaries or definitions where the instructions are clear and the model’s prior knowledge is sufficient.

### Prompt template
```
You are a helpful assistant.  {Task instruction}.  {Any additional constraints or formatting directions}.
```

### Working example
```
You are a helpful assistant.  Summarize the following paragraph in one sentence.

Paragraph: The rain forest is home to thousands of species.  It regulates climate and provides oxygen, yet deforestation threatens this delicate ecosystem.
```
**Model response:**
> The rain forest supports thousands of species and plays a vital role in climate regulation and oxygen production, but deforestation endangers this ecosystem.

### Typical failure points
- The instruction is **too vague**, leading to generic or irrelevant responses.
- The model may **hallucinate information** when the task requires specific knowledge absent in the context.
- Without examples, it may **misinterpret formatting expectations** (e.g. list vs. narrative).

---

## 2. Few‑shot Prompting

### Use‑case
Few‑shot prompting provides the model with **a handful of input–output examples** to guide it toward the desired behavior.  This is useful for classification, style mimicry or pattern‑matching tasks where the correct mapping isn’t obvious from the instruction alone.

### Prompt template
```
You are a {role or assistant}.  Here are some examples:
Input: {example_1_input}
Output: {example_1_output}
Input: {example_2_input}
Output: {example_2_output}
...
Now, given the new input, produce the corresponding output.
Input: {new_input}
Output:
```

### Working example
```
You are an assistant that classifies movie reviews as Positive or Negative.
Here are some examples:
Input: "The story dragged and the acting was poor."
Output: Negative
Input: "I loved the characters and the visual effects were stunning!"
Output: Positive
Now, given the new input, produce the corresponding output.
Input: "The pacing was off but the cinematography was beautiful."
Output:
```
**Model response:**
> Positive

### Typical failure points
- The model may **memorize the examples** and ignore new instructions if examples are not representative.
- Poorly chosen examples can **bias the output** (e.g., all positive examples yield positive predictions).
- Too many examples can exceed context limits or **overwhelm the model**, while too few may be insufficient.

---

## 3. Role Prompting

### Use‑case
Setting a **role or persona** can steer the model’s tone, voice or domain knowledge.  This is helpful for tasks where you want responses to resemble a particular character (e.g., a professor, a customer support agent, a pirate) or to emphasise certain expertise.

### Prompt template
```
You are {role/persona} who {description of behavior or domain}.  When responding, adopt this persona’s tone and perspective.  {Task instruction}.
```

### Working example
```
You are a medieval bard who speaks in rhyming couplets.  When responding, adopt this persona’s tone and perspective.  Describe how a smartphone works.
```
**Model response:**
> A crystal slab that lights and sings, with circuits fine and tiny things;  it sends your words through ether’s hum, so friends afar may hear them come.

### Typical failure points
- The persona can **conflict with the task**, causing content that is stylistically correct but factually wrong or incomplete.
- Overly restrictive roles may **limit the model’s ability** to answer accurately (e.g., a pirate persona may omit technical details).
- If the role is ambiguous, the model might **guess the wrong tone or context**.

---

## 4. Hidden Chain‑of‑Thought (CoT)

### Use‑case
For tasks requiring reasoning (e.g., math, logic, complex question answering), instruct the model to **think step‑by‑step internally**, but only output the final answer.  This improves accuracy without exposing sensitive intermediate reasoning to the user.

### Prompt template
```
{Question or problem}.  Think about this step by step internally, but only write the final answer.
```

### Working example
```
Question: If a train travels 60 km at 30 km/h and then 40 km at 20 km/h, what is its average speed for the whole journey?  Think about this step by step internally, but only write the final answer.
```
**Model response:**
> 24 km/h

### Typical failure points
- The model may **ignore the instruction** and expose its reasoning, which can reveal sensitive chain‑of‑thought.
- If the question is poorly posed, the model might provide an incorrect answer even with hidden reasoning.
- Repeated use can increase latency since the model is generating additional internal tokens.

---

## 5. Self‑Consistency Prompting

### Use‑case
Self‑consistency builds on chain‑of‑thought by asking the model to **generate multiple reasoning paths** and then select the most common or confident answer.  This can improve reliability on complex problems but is more resource‑intensive.

### Prompt template
```
{Question}.  Provide {n} different ways of reasoning to solve the problem, each leading to an answer.  After generating the {n} answers, choose the answer that appears most often and present only that as your final answer.
```

### Working example
```
Question: A bookstore sells books at a 10% discount.  If a book originally costs $50 and the store also charges 5% tax after the discount, what is the final price?  Provide three different ways of reasoning to solve the problem, each leading to an answer.  After generating the three answers, choose the answer that appears most often and present only that as your final answer.
```
**Model response:**
> \$47.25

### Typical failure points
- Requires more tokens, which can **increase cost and latency**.
- If the reasoning paths all converge on the **same wrong answer**, self‑consistency does not help.
- It may be overkill for simple tasks and **confuse stakeholders** expecting a direct answer.

---

## 6. Style Transfer Prompting

### Use‑case
Style transfer prompts instruct the model to **rewrite or generate content in a specified style or voice** while preserving the original meaning.  This is useful for localization, branding, parody or tonal adjustments.

### Prompt template
```
Rewrite the following text in the style of {target style or author}.  Preserve the original meaning.

Text: {original text}
```

### Working example
```
Rewrite the following text in the style of a noir detective novel.  Preserve the original meaning.

Text: The city is lively during the day, but at night it hides its secrets beneath neon lights.
```
**Model response:**
> By day the city buzzes, but when the sun dips, it cloaks its sins in neon and shadow.

### Typical failure points
- The model may **sacrifice factual content** to match the requested style.
- If the style isn’t well known, the response might be **inconsistent or generic**.
- Overly long passages can lead to **loss of coherence** after transformation.

---

## 7. JSON‑Schema Output Prompting

### Use‑case
When building systems that expect structured output, instruct the model to **produce a JSON object matching a given schema**.  This is common for data extraction, API calls or integration with downstream programs.

### Prompt template
```
Extract the required information and return it as JSON following this schema exactly (no additional keys or comments):
{
  "field1": <type>,
  "field2": <type>,
  ...
}

Input: {text or query}
```

### Working example
```
Extract the required information and return it as JSON following this schema exactly (no additional keys or comments):
{
  "name": string,
  "age": integer,
  "city": string
}

Input: "Maria Sanchez, aged 32, recently moved to Barcelona from Madrid."
```
**Model response:**
```json
{
  "name": "Maria Sanchez",
  "age": 32,
  "city": "Barcelona"
}
```

### Typical failure points
- The model may **introduce extra fields** or commentary, breaking the strict JSON format.
- Numeric values might be returned as strings, causing **type mismatches**.
- Complex inputs can cause the model to **omit fields** or misinterpret data.

---

## 8. Refusal with Guardrails

### Use‑case
Guardrail prompts enforce **safety policies** by instructing the model to politely refuse disallowed requests (e.g., harmful content, personal data).  They are crucial for moderation and compliance in production systems.

### Prompt template
```
You are an assistant that adheres to the following policy: {brief description of disallowed content}.  If the user’s request violates this policy, respond with a refusal that explains why you cannot comply; otherwise, provide the requested information.

User: {user query}
Assistant:
```

### Working example
```
You are an assistant that adheres to the following policy: do not provide instructions on committing illegal acts or harm.  If the user’s request violates this policy, respond with a refusal that explains why you cannot comply; otherwise, provide the requested information.

User: "How can I pick a lock without anyone noticing?"
Assistant:
```
**Model response:**
> I’m sorry, but I can’t assist with that request.

### Typical failure points
- Policies must be **clear and comprehensive**; otherwise the model might not recognize a violation.
- If the refusal text isn’t specified, the model may **hallucinate incorrect guidance** or respond inconsistently.
- Overly strict guardrails can lead to **false positives**, frustrating legitimate users.

---

## 9. Critique‑then‑Rewrite Prompting

### Use‑case
This pattern asks the model to **evaluate a piece of text and suggest improvements**, then rewrite it incorporating the feedback.  It’s useful for editing, proofreading and refining drafts.

### Prompt template
```
You are an expert editor.  First, critique the following text for clarity, tone and grammar.  Then rewrite the text to address the issues you identified.

Text: {original text}
```

### Working example
```
You are an expert editor.  First, critique the following text for clarity, tone and grammar.  Then rewrite the text to address the issues you identified.

Text: "Our new product are very good and you gonna love it because it has many features that helps you."
```
**Model response:**
> **Critique:** The sentence has a subject–verb agreement error (“product are”), informal language (“gonna”), and vague phrasing (“many features that helps you”).
>
> **Rewrite:** Our new product is excellent and offers numerous features that will benefit you.

### Typical failure points
- The model may **embed the critique inside the rewrite**, which complicates post‑processing.
- If the original text contains technical jargon, the rewrite may **oversimplify** or distort meaning.
- Without clear instructions, the model might **omit important details** during rewriting.

---

## 10. Summarization‑then‑Generate Prompting

### Use‑case
Use this pattern when you need to **condense a long input** before generating derivative content (e.g., marketing copy, tweets, bullet points).  By summarizing first, the model focuses on salient information before creating new output.

### Prompt template
```
First, summarize the following text in {n} sentences.  Then, based on your summary, {generate/translate/rewrite} into {target format}.  Ensure your final output only contains the {target format} and not the intermediate summary.

Text: {long text}
```

### Working example
```
First, summarize the following text in two sentences.  Then, based on your summary, write a promotional tweet (280 characters max).  Ensure your final output only contains the tweet and not the intermediate summary.

Text: The EcoBottle is a reusable water bottle made from recycled materials.  It keeps drinks cold for 24 hours or hot for 12 hours.  For every bottle sold, the company plants a tree.  The design is sleek and available in multiple colors.
```
**Model response:**
> Stay hydrated sustainably with the EcoBottle!  Made from recycled materials, it keeps drinks cold for 24 hrs and hot for 12 hrs.  Plus, every purchase plants a tree.  Choose your color and join the eco movement today! #EcoBottle #Sustainability

### Typical failure points
- The model may **output the intermediate summary** along with the final text if not instructed to hide it.
- Summaries that are too short can **omit critical details**, leading to poor downstream generation.
- Without a strict character or word limit, generated content may **exceed the target format** (e.g., tweet length).

---

## Constraint Templates

Use these templates to enforce additional constraints on any of the patterns above.  They can be appended to the prompt to guide the model’s tone, length or content safety.

### Tone restriction
```
Respond using a {tone} tone (e.g., professional, friendly, confident).  Avoid deviating from this tone.
```
*Example:* “Respond using a professional tone.”

### Word or character limit
```
Limit your response to {N} words{ or characters}.  Do not exceed this limit.
```
*Example:* “Limit your response to 50 words.”

### No slang or profanity
```
Avoid slang, colloquialisms and profanity.  Use formal language.
```
*Example:* “Avoid slang and profanity; use formal language throughout your answer.”

These constraint templates can be combined.  For example: “Respond using a friendly tone.  Limit your response to 100 words.  Avoid slang and profanity.”

---

*End of handbook*
