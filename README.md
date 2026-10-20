# LangGraph Project

Every useful question-answering system starts with something simple: giving it the right information to search. This project is at that first step. Its notebook takes text files, prepares them for semantic search, and saves a local FAISS index.

## What it does today

`Project.ipynb` reads `.txt` files from `docs_dir/`, groups their paragraphs into chunks of roughly 500 characters, and records each chunk's source filename. Gemini's `gemini-embedding-001` model turns those chunks into vectors, which LangChain stores in a FAISS index under `faiss_index/`.

That index is the groundwork for a document-search or RAG application. The current project does not yet search the index, answer questions, run a LangGraph workflow, or provide a Streamlit interface.

## The notebook, from start to finish

1. Loads environment variables from `.env`.
2. Finds and reads each `.txt` file in `docs_dir/`.
3. Splits the text at blank lines, then combines paragraphs into chunks without crossing file boundaries. Each chunk keeps its source filename as metadata.
4. Creates Gemini embeddings for the chunks.
5. Builds a FAISS vector store and saves it locally.

Chunk sizes are approximate: a single paragraph longer than 500 characters stays intact and can produce a larger chunk. The current chunking logic does not add overlap between chunks.

## Project layout

- `Project.ipynb` — document loading, chunking, embedding, and index creation.
- `docs_dir/` — place source `.txt` files here.
- `faiss_index/` — generated FAISS index files (`index.faiss` and `index.pkl`).
- `src/krish_naik_hindi_langraph_project/` — Python package source.
- `pyproject.toml` and `uv.lock` — project metadata and dependency versions.
- `.env` — local API credentials and configuration; keep it private.

## Get set up

You'll need Python 3.14 or newer, [uv](https://docs.astral.sh/uv/), and a Google Gemini API key.

From the project directory, install the dependencies:

```powershell
uv sync
```

Create a `.env` file in the project root and add your key:

```dotenv
GOOGLE_API_KEY=your_gemini_api_key
```

Replace the example value with your own key. Never put API keys in source code, notebook output, or a public repository.

## Build the index

Put one or more `.txt` files in `docs_dir/`. Open `Project.ipynb` in VS Code, select the project's `.venv` Python kernel, and run the notebook cells from top to bottom. It prints the number of chunks created and saves the index to `faiss_index/`.

The notebook also sets `KMP_DUPLICATE_LIB_OK` as a workaround for certain duplicate OpenMP runtime errors. It does not resolve the underlying library conflict, so it should only be kept if that error actually occurs.

## Dependencies in brief

- `langchain` and `langchain-community` connect the document workflow to FAISS.
- `langchain-google-genai` provides Gemini embeddings.
- `faiss-cpu` creates the local vector index.
- `python-dotenv` loads settings from `.env`.
- `langchain-groq`, `langgraph`, and `streamlit` are listed as project dependencies, but the current notebook does not use them yet.
