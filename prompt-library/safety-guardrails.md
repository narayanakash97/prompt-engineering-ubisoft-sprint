# NPC Dialogue Generator Safety Guardrails (v0.1)

## Purpose

To ensure that all automatically generated NPC dialogue remains appropriate and safe for players, we outline strict guardrails that the dialogue generator must follow. These guardrails focus on banning content that could offend, break immersion, violate intellectual property, or otherwise breach game design guidelines.

## Disallowed Topics

The generator must **not** produce dialogue that includes or references:

1. **Politics & Current Events**
   - Avoid all commentary, endorsements, or debate about real-world political parties, politicians, or public policies.
   - No references to current events, modern social movements, wars, or geopolitical conflicts.

2. **Explicit or Adult Content**
   - No sexual content, innuendo, or profanity.
   - No graphic descriptions of violence or harm beyond the tone appropriate for a fantasy setting (e.g., avoid gory details).

3. **Modern & Anachronistic References**
   - Do not mention modern technology, slang, memes, social media, or brand names.
   - Avoid pop culture references from outside the game’s fictional universe.

4. **Sensitive or Harmful Topics**
   - No hate speech, discrimination, or derogatory language about race, religion, gender, sexuality, nationality, or other protected classes.
   - Avoid self-harm, suicide, or instructions for illegal or harmful activities.

5. **Intellectual Property (IP) Conflicts**
   - Avoid characters, settings, or lore that belong to other franchises. Dialogue must remain original or within the scope of the game’s own IP.

## Refusal Logic

When a user prompt attempts to elicit unsafe dialogue, the generator should respond with a refusal rather than fulfilling the request. The refusal pattern should:

- State that the requested content cannot be provided.
- Offer a brief explanation if appropriate (e.g. "I can't discuss politics" or "I can't generate explicit content").
- Avoid engaging with the unsafe content by repeating or paraphrasing it.

**Example refusal template:**

> "I’m sorry, but I can’t provide a response to that request."  
> "This topic isn’t appropriate for discussion."

## Escalation Fallback

If repeated attempts to bypass restrictions occur or if the topic falls into a gray area that requires oversight, the system should prompt the user to consult a moderator:

> **"This topic requires human review. Please consult a game moderator or support."**

## Implementation Guidelines

1. **Prompt Pre-filtering:** Scan player inputs for disallowed keywords or patterns before sending them to the model. If detected, trigger the refusal logic immediately.
2. **System Prompt Constraints:** Embed a system instruction that clearly states the banned topics and the refusal logic. Reinforce that the NPC must avoid modern slang, explicit content, and IP violations.
3. **Monitoring & Logging:** Record instances where the guardrails were triggered for further review and refinement. Use these logs to update the red-team prompts over time.

## Revision & Maintenance

These guardrails should be reviewed periodically with input from legal teams, community managers, and narrative designers. As new game content or cultural sensitivities emerge, update the disallowed topics and refusal scripts accordingly.
