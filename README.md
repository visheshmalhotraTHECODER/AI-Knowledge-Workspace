# AI Knowledge Workspace

A document-grounded question-answering application built with Next.js, LangChain and a hosted vector database. Upload supported documents, retrieve relevant passages with vector similarity search, and ask an LLM questions using the retrieved context.

> **Project status:** Source code is published. Provider credentials and an Upstash Vector index are required for document ingestion and AI chat. These integrations have not been configured in this repository's public deployment.

## Features

- Upload PDF, DOCX, TXT and Markdown files (the ingestion route enforces a 50 MB per-file limit).
- Parse and chunk documents before embedding and indexing them.
- Retrieve relevant chunks from Upstash Vector for a question.
- Stream generated answers to the browser.
- Return source filenames with answers.
- Select among the LLM providers supported by the application: Groq, Gemini, OpenAI, Anthropic and Ollama.

## How it works

```text
Document upload
    -> parse file
    -> split into chunks
    -> generate embeddings
    -> store vectors in Upstash

User question
    -> retrieve relevant chunks
    -> build a context-grounded prompt
    -> selected LLM
    -> streamed response + source filenames
```

The chat prompt instructs the model to answer from the retrieved document context and say when the answer is not present in that context.

## Tech stack

| Area | Technology |
|---|---|
| Web application | Next.js 15 App Router, React 19 |
| Language | TypeScript |
| Retrieval / orchestration | LangChain.js |
| Vector storage | Upstash Vector |
| LLM integrations | Groq, Google Gemini, OpenAI, Anthropic, Ollama |
| Document parsing | `pdf-parse`, `mammoth` |
| Styling | Tailwind CSS 4 |
| Deployment target | Vercel (requires configured environment variables) |

## Run locally (Windows PowerShell)

**Prerequisites:** Node.js 20 or later, npm, API credentials for at least one supported LLM provider, and an Upstash Vector index.

1. Clone this repository and enter the folder:

   ```powershell
   git clone https://github.com/visheshmalhotraTHECODER/AI-Knowledge-Workspace.git
   cd AI-Knowledge-Workspace
   ```

2. Install dependencies:

   ```powershell
   npm install --legacy-peer-deps
   ```

   The legacy peer-dependency option may be needed with the dependency versions currently pinned in this starter. It relaxes npm's peer-dependency resolution; it does not prove that every dependency combination is runtime-compatible.

3. Create your local environment file:

   ```powershell
   Copy-Item .env.example .env.local
   ```

4. Add the required provider and vector-store values to `.env.local`. Never commit this file.

5. Start the development server:

   ```powershell
   npm run dev
   ```

6. Open [http://localhost:3000](http://localhost:3000).

### Environment variables

| Variable | Purpose |
|---|---|
| `GROQ_API_KEY` | Groq chat model |
| `GEMINI_API_KEY` | Gemini model / embeddings |
| `OPENAI_API_KEY` | OpenAI model / embeddings |
| `ANTHROPIC_API_KEY` | Anthropic chat model |
| `OLLAMA_BASE_URL` | Optional local Ollama endpoint |
| `UPSTASH_VECTOR_REST_URL` | Upstash Vector REST endpoint |
| `UPSTASH_VECTOR_REST_TOKEN` | Upstash Vector REST token |

Configure only the providers you intend to use. Create the Upstash index with an embedding dimension that matches the selected embedding model. Consult the provider documentation and the application's implementation before changing dimensions.

## Useful commands

```powershell
npm run dev
npm test
npm run build
```

## Repository layout

```text
app/
  api/
    chat/       # Retrieval-augmented chat endpoint
    ingest/     # Document upload, parsing and indexing endpoint
    providers/  # Provider configuration endpoint
    suggest/    # Suggested questions endpoint
  components/   # Chat, document sidebar, upload UI and source cards
  lib/          # LLM, parsing, embeddings and vector-store helpers
  page.tsx      # Main application page
```

## Security and operational notes

- Keep all API keys and service tokens in `.env.local` or deployment environment settings.
- Do not expose Upstash credentials or private provider keys through client-side variables.
- The ingestion endpoint accepts PDF, DOCX, TXT and Markdown files and checks file size; deploy behind appropriate authentication, rate limits and abuse controls before exposing it to untrusted users.
- A successful page load does not confirm that ingestion or chat works. Verify both flows after configuring the providers and vector index.
- No public production URL is claimed here; deployment requires configuring the secrets in the hosting provider.

## Attribution and license

This repository is an adaptation of **[docuchat-ai](https://github.com/Dipanshu-js/docuchat-ai)** by **[Dipanshu Singh](https://github.com/Dipanshu-js)**. The upstream project and its MIT license are retained; see [LICENSE](LICENSE). The original project/package metadata and copyright notices remain part of the source.

**Portfolio maintainer:** [Vishesh Malhotra](https://github.com/visheshmalhotraTHECODER).

If you build on this adaptation, retain the upstream copyright and license notice as required by the MIT license.
