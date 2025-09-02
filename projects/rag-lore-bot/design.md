# RAG System Design for Lore‑Grounded NPC Dialogue

## Document Collection
- **Sources**: Collect lore documents from official game design documents, quest scripts, world encyclopedias, and character biographies.
- **Formats**: Text files or Markdown; convert other formats (e.g., PDFs) to plain text.
- **Preprocessing**: Clean and normalise text (remove formatting, unify names, fix typos). Tag each document with metadata.

## Chunking Method
- Split documents into chunks of ~100 words with 20‑word overlap to preserve context across boundaries.
- Each chunk is stored with its metadata (era, location, character) and a unique identifier.
- Use sentence segmentation to avoid splitting sentences mid‑way.

## Metadata Fields
- `era`: The historical period (First Age, Second Era, Age of Ash, etc.).
- `location`: Geographic region or settlement relevant to the chunk.
- `character`: Primary character(s) mentioned, if any.
- `source`: Original document name or quest.

## Retrieval Prompt
```
You are a retrieval system for a fantasy RPG. Given a player query and optional metadata filters, return up to 3 lore chunks relevant to the query. Include the chunk text and its metadata fields (era, location, character). If nothing is relevant, return an empty list.
Query: {player_query}
Metadata filters: {filters}
```

## Generation Prompt
```
You are an NPC responding to a player in a medieval fantasy world. Use the provided lore snippets to craft a reply that is immersive and lore‑accurate. Do not invent facts beyond the retrieved context. Speak in character, referencing the snippets where appropriate. If there is no context, answer briefly and state that you lack knowledge of the topic.
Context:
{retrieved_chunks}

Player question: {player_question}

NPC response:
```

## Grounding Rule
- **Do not invent facts beyond retrieved context.** The NPC may only answer using information contained in the snippets. If the snippets do not cover the requested topic, the NPC should indicate ignorance.

## Retrieval and Ranking
- Encode chunks and queries with embeddings (e.g., using an OpenAI embedding model).
- Filter by metadata when specified.
- Rank by cosine similarity.
- Optionally apply a re‑ranker to emphasise matching keywords.

## System Flow
1. Receive player query and identify relevant era, location, or character if possible.
2. Retrieve top‑k lore chunks using vector search and metadata filtering.
3. Combine selected snippets and send to the generation prompt along with the player’s question.
4. Generate NPC dialogue grounded in retrieved lore.
5. Present response to the player.
