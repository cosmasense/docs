hi chat we are making a startup product called CosmaSense, which is a open source local file indexing and search engine and app (currently avaliable though alpha testing) and can be dowloand though github and we are working on macOS app. Currently we have three components: Backend, CLI, GUI in swift ui. 



Cosma - AI-Powered Local File Search

  What It Is

  Local semantic search engine that finds your files using natural
  language queries. Runs 100% locally. Like google for your local files. 

  Tech Stack

  Backend (packages/cosma-backend):
  - Quart async web framework + Uvicorn
  - SQLite with sqlite-vec (vector search) + FTS5 (keyword search)
  - LiteLLM/Ollama for AI summarization
  - sentence-transformers for embeddings
  - MarkItDown for parsing 20+ file types

  TUI (packages/cosma-tui):
  - Textual framework for terminal UI
  - Real-time SSE updates from backend
  - 0.5s throttled search

  CLI (root): Click-based orchestrator for cosma serve and cosma
  search

  How It Works

  Processing Pipeline (pipeline.py:56-174)

  Files go through 4 stages:

  1. Discovery: Recursively scan directories, collect file metadata
  2. Parsing: Extract text from 20+ formats (PDF, DOCX, images,
    etc.) using MarkItDown
  3. Summarization: AI generates title, summary (≤100 words), and
    3-5 keywords
  4. Embedding: Create 768-dim vectors using e5-base-v2 model

  Each stage checks if file already processed (by modified timestamp
   & content hash) to avoid redundant work. Failed files marked in
  DB with error.

  Hybrid Search (searcher.py:91-220)

  Combines two search methods:

  Semantic (vector similarity):
  - Embed query → cosine similarity against file embeddings
  - Score: exp(-distance) scaled to 0-0.5 range

  Keyword (FTS5 full-text):
  - SQLite FTS5 searches content/title/keywords
  - Score: relevance score scaled to 0-0.5 range

  Combined scoring: semantic_score + keyword_score (additive, not
  weighted average)

  Results sorted by combined score descending.

  TUI Flow (tui.py)

  1. On mount: Connect to backend, start indexing directory, listen
    to SSE updates
  2. User types query → throttled to 0.5s → backend search → display
      results bottom-to-top
  3. User selects file (Enter/click) → prints file path to stdout
    (fzf-style)

  Real-time Updates

  - Backend watches directories via watchdog
  - File changes → auto-process through pipeline
  - SSE stream pushes status updates to TUI status bar
  - Updates types: file_parsing, file_summarizing, file_embedding,
    file_complete, file_failed

  Architecture

  Monorepo with 3 packages sharing dependencies via workspace.
  Backend runs as daemon (cosma serve), TUI connects via HTTP/SSE on
   port 60534.

  Database (schema.sql):
  - files: Core data (path, content, title, summary, keywords, hash,
      status)
  - file_embeddings: Virtual vec table for similarity search
  - files_fts: Virtual FTS5 table for keyword search
  - Triggers keep indexes in sync

  Key Code Logic

  - Skip logic (pipeline.py:180): Skip if modified timestamp
    unchanged
  - Hash check (pipeline.py:200): Reprocess if content hash changed
  - Search throttling (tui.py:325): Debounce searches with pending
    query queue
  - Score calculation (searcher.py:178-196): Exponential decay for
    semantic + direct keyword score





