# Problem Statement: Generating Lore-Safe NPC Dialogue

## Background
In narrative-driven games, non-player characters (NPCs) need to deliver dialogue that enriches the game world without conflicting with the established lore. As players interact with NPCs repeatedly, the lines should remain fresh and engaging rather than repetitive.

## Objective
This project aims to develop prompt-engineering techniques to generate NPC dialogue that:

- Is consistent with the game’s lore and world-building constraints.
- Avoids repetition across interactions, providing varied lines.
- Allows designers to control the tone (e.g., friendly, ominous) and length of the generated lines for different contexts.

## Challenges
Generating NPC dialogue must account for:
- **Lore consistency**: prompts must condition the model to respect game-specific facts, names, events, and tone; any deviation could break immersion.
- **Non-repetitiveness**: repeated phrasing can reduce player engagement; prompts and model outputs need to incorporate variation strategies.
- **Tone and length control**: designers may require short, medium, or long lines and specific emotional undertones; prompts must include controllable parameters.

## Scope
This problem statement outlines a prompt-engineering sprint that will explore methods such as zero-shot, few-shot, and chain-of-thought prompts, as well as evaluation metrics to ensure lore fidelity. The deliverables include a prompt library, evaluation reports, and a playbook documenting effective strategies for generating lore-safe, non-repetitive NPC dialogue with controllable tone and length.
