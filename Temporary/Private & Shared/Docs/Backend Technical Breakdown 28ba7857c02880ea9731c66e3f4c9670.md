# Backend Technical Breakdown

# Watching

API route: `/api/watch/directory`

When this is called, we first need to check if the directory (or a parent directory) is in the database. If it’s truly a new directory, we add it to our `watched_directories` table, and begin indexing. We also create a background task to monitor the filesystem for changes to directory content.

# Indexing

Indexing occurs when:

1. An API request to `/api/index/directory` is made
2. A new directory is watched
3. The backend starts

When we receive a directory to index, we must re-discover every file within to check for updates.

## Pipeline

For individual files or each file discovered, we go through a pipeline to index and process the file.

File → Parsed → Summarized → Embedded

This process is not queued at the moment. Each file is taken through one at a time. Later we’ll probably want to consider how to make this more efficient.

### File discovery

The file discoverer uses the database and the last known modified time to filter files that we’ve already indexed. So only files that have been modified since the database was updated or aren’t in the database are returned. Basic file metadata is stored to the database at this step.

At some point we’ll want to make use of MacOS FSEvents for detecting whether files have been changed, skipping the parser when not needed.

### File parser

The file parser takes a file and uses MarkItDown to parse the file to markdown. It then calculates a hash for that file and checks it against the database to see if the file content has changed. If it has, it moves to the next step. Files hashes are stored to the database at this step.

### File summarizer

The file summarizer takes the parsed file and uses an LLM to summarize it (and extract keywords). File summaries and keywords are stored to the database at this step.

### File embedder

The file embedder takes the summarized file and created an embedding for it. File embeddings are stored to the database at this step.

`/api/updates`

→ Stream of events

e.x. processed file asdjasdj 50%

`/api/search/stream`

→ Stream of search results

GET `/api/watched-directories`

POST `/api/watch/directory` 

file path

`/Downloads`

files : `/Downloads/file.txt`