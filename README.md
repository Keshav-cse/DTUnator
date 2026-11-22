# DTUnator

A doubt-solving assistant for DTU students that answers **only from verified DTU lecture
material**. If the answer isn't in the material, it says so instead of guessing. Students can
also upload their own PDFs or lecture recordings and ask questions about them.

Built for the **BrainWave 2.0** hackathon (Jan 2026). Later stages:
[`rag-prototype`](https://github.com/keshav-talwar/rag-prototype) →
[`dtunator-rag-sft-rl`](https://github.com/keshav-talwar/dtunator-rag-sft-rl) (v2).

## Why

Course information is scattered across many documents, and general chatbots aren't reliable for
DTU's specific curriculum. DTUnator grounds every answer in official course documents.

## How it works (`index.py`)

1. **Ingest**
   - **PDFs:** PyMuPDF text extraction; scanned pages (no text layer) are detected and OCR'd with Tesseract.
   - **Audio:** faster-whisper (`distil-medium.en`) transcripts with `[mm:ss]` timestamps.
   - Text is cleaned and split with LangChain's `SemanticChunker` over `all-MiniLM-L6-v2`
     embeddings. Chunks keep source, an `is_general` flag and audio start/end times.
2. **Store:** embedded **Weaviate**, persisted in `vector_db/` (collection `DtuDoubt`). The repo
   ships with a pre-built index of the DTU material.
3. **Retrieve:** top-12 vector search, then **cross-encoder rerank** to the top 5
   (`cross-encoder/ms-marco-MiniLM-L-6-v2`). Uploaded-file chunks are searched alongside the index.
4. **Answer:** the last 4 turns of history plus the retrieved notes go to the LLM through the
   **OnDemand API** (endpoint `predefined-openai-gpt4.1-nano`). The system prompt restricts it to
   the notes and tells it to say when the information isn't there.
5. **UI:** **Streamlit** chat with a sidebar for uploading a file as extra context, chat history
   in session state, and a per-request rate limit.

Planned and pitched for the hackathon but **not in `index.py` yet**: query rewriting, HyDE
(hypothetical-document) retrieval and a separate answer-verification pass.

## Run

```bash
python -m venv .venv && source .venv/bin/activate
pip install streamlit weaviate-client langchain-experimental langchain-huggingface \
            sentence-transformers faster-whisper pymupdf pytesseract pillow requests python-dotenv
# Tesseract binary: brew install tesseract  (or apt install tesseract-ocr)
export ONDEMAND_API_KEY=...        # never commit it
streamlit run index.py
```

## Layout

```
index.py      # whole app: ingestion, Weaviate, rerank, LLM call, Streamlit UI
vector_db/    # embedded Weaviate data (pre-built DTU index)
```

## Known gaps

- No `requirements.txt` yet (the list is above).
- All logic is in one file, and there are no tests.
