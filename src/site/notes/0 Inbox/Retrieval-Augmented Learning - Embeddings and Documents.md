---
{"dg-publish":true,"permalink":"/0-inbox/retrieval-augmented-learning-embeddings-and-documents/"}
---

tags:: [[0 Inbox/GPT\|GPT]] [[3 Resources/AI\|AI]]

Augment a large language model by first retrieving related data before letting it execute its instructions.

Best way is to retrieve related documents through **embeddings** which capture the "meaning" of a string into an n-dimensional vector. You will use a [[Vector Store\|Vector Store]] to store this data and retrieve documents through maximizing the cosine similarity between the vectors in the database and the vector of the query.

## Supabase
With [[Supabase\|Supabase]] you can put this into a postgres database using pgvector.

My questions:
- How does it handle data updates? I don't want to pay to generate new embeddings for my entire knowledgebase if I just added one file.
- How is the content split up?

So apparently Supabase Vecs automatically splits data into chunks.

They have a GitHub Action [embeddings-generator](https://github.com/supabase/embeddings-generator) that uploads the Markdown files in a repository into a Supabase database.
- The action loads all Md files from a repo
- Action uses checksum to prevent pages from refreshing
- It looks focused on their static site generator's structure but also looks flexible enough to take everything. I'm a bit confused by the "parent path" stuff
- Splits into sections by headers
