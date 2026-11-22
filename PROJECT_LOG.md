# Project log: DTUnator
<!-- last-updated: 2026-11-22T09:46:19Z (maintained by /updaterepo) -->

## Current state
- `index.py`: PyMuPDF + Tesseract OCR + faster-whisper ingestion, SemanticChunker (all-MiniLM-L6-v2), embedded Weaviate
  (`vector_db/`, collection DtuDoubt), top-12 → MS MARCO cross-encoder top-5, LLM via OnDemand (`predefined-openai-gpt4.1-nano`),
  Streamlit UI with file upload and rate limiting.
- No requirements.txt; dependency list is in the README.
- Next stages: `rag-prototype`, `dtunator-rag-sft-rl`.

## Tried: don't repeat
| Approach | Last tried | Result | Why it failed / note |
|---|---|---|---|
| Describing DTUnator as using HyDE, query rewriting, a verification pass and Gemini 2.5 Flash | 2026-11-22 | ❌ not in code | Old README was the hackathon pitch; README, CV and PROFILE corrected |

## What worked
- _none yet_

## Decisions
- 2026-11-22: README now describes what the code does, and lists the pitched features as 'planned'

## Open questions / next steps
- [ ] Add requirements.txt
- [ ] Implement HyDE / query rewriting if still wanted (then the CV can claim them)

## History
### 2026-11-22: README rewrite
- Did: rewrote README from the code; remote now github.com/keshav-talwar
- Learned: the original README overstated the implementation
