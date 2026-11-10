# youtube-rag-chatbot
Fully local RAG chatbot to query YouTube video transcripts using LangChain, FAISS, and Ollama (Gemma 3).


# YouTube Video Transcript Q&A (Local RAG)

A lightweight, on-device Retrieval-Augmented Generation (RAG) pipeline built to index and query YouTube video transcripts without relying on external cloud APIs or paid tokens.

The entire workflow runs locally: transcript extraction, text chunking, local vector embeddings, in-memory FAISS indexing, and local inference via Ollama.

---

## Architecture Flow

1. **Transcript Fetching**: Extracts raw subtitle streams and timestamps dynamically using `youtube-transcript-api`.
2. **Chunking & Overlap**: Splits continuous spoken text using `RecursiveCharacterTextSplitter` (chunk size: 500, overlap: 50) to prevent conversational thoughts from breaking across boundaries.
3. **Embeddings & Vector Store**: Encodes text segments with `nomic-embed-text` and indexes them into an in-memory `FAISS` index.
4. **Context-Grounded Inference**: Retrieves top-$k$ ($k=4$) semantic matches and injects them into a strict prompt template, where a local `gemma3` model answers queries grounded strictly in the video context.

---

## Tech Stack

* **Framework**: LangChain
* **Vector Store**: FAISS (`faiss-cpu`)
* **Embeddings**: `nomic-embed-text` (via Ollama)
* **LLM**: `gemma3` (via Ollama)
* **Data Extraction**: `youtube-transcript-api`

---
## Trade-offs (Pros & Cons)

### Pros
- **Zero Cost**: 100% free and open-source without any paid API dependencies.
- **Privacy-First**: No data leaves the local machine.
- **Fast Local Search**: In-memory FAISS provides instant vector similarity lookups.

### Cons & Limitations
- **Hardware Dependent**: Requires sufficient local RAM/VRAM to run local LLMs smoothly.
- **Volatile Storage**: The vector index lives in memory and resets once the session closes.
- **Transcript Dependent**: Works only on YouTube videos with available captions/transcripts.
- 
## Getting Started

### 1. Prerequisites
Make sure [Ollama](https://ollama.ai/) is installed and running locally. Pull the necessary models:

```bash
ollama pull nomic-embed-text
ollama pull gemma3
