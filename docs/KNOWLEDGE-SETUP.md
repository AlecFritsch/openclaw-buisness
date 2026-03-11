# Knowledge / RAG Setup

The Knowledge Hub uses MongoDB Atlas Vector Search for semantic search. You must create the index manually.

## Atlas Vector Search Index

1. Go to [MongoDB Atlas](https://cloud.mongodb.com) → your cluster → **Search** → **Create Search Index**.

2. Choose **JSON Editor** and create an index on the `knowledge_chunks` collection with:

```json
{
  "fields": [
    {
      "type": "vector",
      "path": "embedding",
      "numDimensions": 3072,
      "similarity": "cosine"
    },
    {
      "type": "filter",
      "path": "organizationId"
    }
  ]
}
```

3. Name the index **`knowledge_vector_index`** (must match the code).

4. Embeddings are generated via `GEMINI_API_KEY` (gemini-embedding-001). Ensure the backend has a valid key, or organizations must provide their own embedding-capable provider (OpenAI, Google, Voyage) in Settings.
