# NPC Dialogue A/B Evaluation – Report 01

This report evaluates two sets of NPC dialogue (`A` and `B`) using a rubric that scores lines from 1–5 across five criteria: **Lore‑fit**, **Tone**, **Variety**, **Brevity** and **Safety**.  A score of 1 means the criterion fails badly (e.g., modern language in a medieval fantasy world), 3 is average, and 5 represents excellent adherence.  Brief comments summarise notable issues per line.

## Evaluation rubric (1 = poor, 3 = average, 5 = excellent)

| Criterion | 1 – Poor | 3 – Average | 5 – Excellent |
|---|---|---|---|
| **Lore‑fit** | Mentions modern items or concepts that break immersion | Generally fits the setting but may contain minor anachronisms | Fully consistent with fantasy lore and world rules |
| **Tone** | Tone clashes with the character’s role | Mostly correct tone but sometimes inconsistent | Matches NPC’s personality/profession consistently |
| **Variety** | Very repetitive wording or ideas | Some repetition, limited variation | Distinct wording and ideas across lines |
| **Brevity** | Exceeds the 15‑word limit or rambles | Within limit but occasionally verbose | Concise; always ≤15 words |
| **Safety** | Contains taboo topics, profanity or sensitive content | Mostly safe but hints at modern ideas | Free of inappropriate or sensitive content |

## Datasets

### Dataset A – Baseline prompt (20 lines)
These lines were generated with a loose prompt.  Many contain modern terms or concepts (e.g., **bike**, **smartphone**, **wifi**) that clash with a medieval fantasy setting.  Scores reflect these issues.

| ID | NPC type | Lore‑fit | Tone | Variety | Brevity | Safety | Comments |
|---|---|---|---|---|---|---|---|
| A1 | Blacksmith | 2 | 3 | 3 | 4 | 5 | Mentions *bike* – breaks lore |
| A2 | Blacksmith | 3 | 3 | 3 | 4 | 5 | “Latest tools” hints at modernity |
| A3 | Blacksmith | 1 | 2 | 3 | 4 | 5 | Uses *lasers* – not medieval |
| A4 | Blacksmith | 1 | 1 | 3 | 4 | 5 | *Texting* friend breaks immersion |
| A5 | Blacksmith | 1 | 1 | 3 | 4 | 5 | Refers to a *smartphone* |
| A6 | Apothecary | 1 | 1 | 3 | 4 | 5 | Mentions *wifi* |
| A7 | Apothecary | 2 | 2 | 3 | 4 | 5 | References the *internet* |
| A8 | Apothecary | 3 | 3 | 3 | 5 | 5 | *Antibiotics* feel modern |
| A9 | Apothecary | 3 | 3 | 3 | 4 | 5 | Mixes potion with *taxes* |
| A10 | Apothecary | 2 | 2 | 3 | 4 | 5 | Requests a review – meta |
| A11 | Blacksmith | 5 | 4 | 3 | 4 | 5 | Solid smithing line |
| A12 | Blacksmith | 4 | 3 | 3 | 4 | 5 | Humorous “burnt toast” reference |
| A13 | Apothecary | 5 | 5 | 3 | 4 | 5 | Good lore and tone |
| A14 | Apothecary | 4 | 4 | 3 | 4 | 5 | Fits setting; casual tone |
| A15 | Blacksmith | 2 | 2 | 3 | 4 | 5 | Mentions *credit cards* |
| A16 | Apothecary | 2 | 2 | 3 | 4 | 5 | Refers to an *app* |
| A17 | Blacksmith | 4 | 3 | 3 | 4 | 4 | Mentions being *drunk* |
| A18 | Apothecary | 2 | 3 | 3 | 4 | 5 | *Therapist* is modern |
| A19 | Blacksmith | 2 | 3 | 3 | 4 | 5 | “Medieval Kevlar” – modern |
| A20 | Apothecary | 5 | 4 | 3 | 4 | 5 | Strong fantasy brew line |

*Dataset A averages:* Lore‑fit ≈2.7, Tone ≈2.7, Variety = 3.0, Brevity ≈4.0, Safety ≈5.  Overall quality is low because many lines violate lore or tone.

### Dataset B – Improved prompt (20 lines)
These lines were generated using a tighter prompt with clear constraints on tone, word count and vocabulary.  All lines maintain medieval flavour and avoid modern references.

| ID | NPC type | Lore‑fit | Tone | Variety | Brevity | Safety | Comments |
|---|---|---|---|---|---|---|---|
| B1 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Good lore‑fit; concise |
| B2 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Strong forging imagery |
| B3 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Helm forging fits |
| B4 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Polishing shield; clear tone |
| B5 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Promises sharp weapons |
| B6 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Anvil “song” evokes lore |
| B7 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Warns of hot sword |
| B8 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Ready armour line |
| B9 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Dragon’s breath reference |
| B10 | Blacksmith | 5 | 5 | 4 | 5 | 5 | Hope for village |
| B11 | Apothecary | 5 | 5 | 4 | 5 | 5 | Healing tincture |
| B12 | Apothecary | 5 | 5 | 4 | 5 | 5 | Herbal gathering advice |
| B13 | Apothecary | 5 | 5 | 4 | 5 | 5 | Bitter taste implies efficacy |
| B14 | Apothecary | 5 | 5 | 4 | 5 | 5 | Wolfbane recipe |
| B15 | Apothecary | 5 | 5 | 4 | 5 | 5 | Lavender for dreams |
| B16 | Apothecary | 5 | 5 | 4 | 5 | 5 | Nightshade caution |
| B17 | Apothecary | 5 | 5 | 4 | 5 | 5 | Elder tree mention |
| B18 | Apothecary | 5 | 5 | 4 | 5 | 5 | Salve vs. spells |
| B19 | Apothecary | 5 | 5 | 4 | 5 | 5 | Thank the earth |
| B20 | Apothecary | 5 | 5 | 4 | 5 | 5 | Herb tea for strength |

*Dataset B averages:* Lore‑fit = 5.0, Tone = 5.0, Variety = 4.0, Brevity = 5.0, Safety = 5.0.  The improved prompt produces consistent high‑quality lines.

## Insights

- **Prompt constraints matter.**  Dataset A, generated from a loosely specified prompt, often includes modern references (e.g., *bike*, *smartphone*, *wifi*) and out‑of‑character tone.  These issues severely reduce Lore‑fit and Tone scores.  In contrast, Dataset B uses explicit constraints on vocabulary, tone and word count, leading to uniformly high scores across all metrics.
- **Best line:** *B6 – “The anvil rings; its song promises protection for brave warriors.”*  This line scores 5 across all criteria.  It evokes imagery tied to smithing, maintains proper tone and brevity, and contains no modern terms.
- **Worst line:** *A4 – “Come back tomorrow; I’m busy texting my friend about your order.”*  This line scores 1 for Lore‑fit and Tone because *texting* is anachronistic, breaking immersion.  Such modern language can pull players out of the experience.
- **Variety still matters.**  Even in Dataset B, lines could be diversified further.  All B lines follow similar structures; exploring different sentence patterns would improve the Variety score beyond 4.

## Annotated screenshot

The following screenshot shows the `npc-dialogue-v1` folder in the project repository after adding the dialogue samples and style guide.  It demonstrates the organisation of evaluation assets used in this report.  The clear folder structure under `/projects` simplifies access for manual review and automated testing.

---

### Conclusion

The evaluation harness shows that structured prompts with clear constraints yield dramatically better NPC dialogue.  The rubric helps quantify improvements by assessing lore adherence, tone, variety, brevity and safety.  Tightening prompts to enforce fantasy‑appropriate vocabulary and word limits improved average scores from ≈3 to a perfect 5.  Future iterations could expand the rubric to include emotional impact or player engagement and automate scoring for larger datasets.
