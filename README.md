# AI Interview Coach

Monorepo: **FastAPI** backend and **Next.js** frontend for mock interviews, scoring, and coaching.

## Layout

| Path | Role |
|------|------|
| `backend/` | API, auth, interview engine, RAG, rate limits, audit log |
| `frontend/` | Web UI (App Router) |

## Prerequisites

- Python 3.10+ and `pip` (or use existing venv under `backend/.venv` if present)
- Node 18+ and npm
- OpenAI (or configured provider) API keys as in `backend/.env.example`

## Quick start

From repository root:

```bash
# Backend API (default http://127.0.0.1:8000)
make run-backend

# Frontend (default http://127.0.0.1:3000)
make run-frontend
```

Set `NEXT_PUBLIC_API_BASE` in `frontend/.env.local` if the API is not on port 8000.

Full deployment notes: `DEPLOYMENT.md`. Operations checklist: `backend/docs/OPERATIONS.md`.

## Architecture (short)

- **Auth**: JWT in JSON + **HttpOnly** cookie; responses include a **stateless CSRF token** (`csrf_token`) — send `X-CSRF-Token` on authenticated **POST/PUT/PATCH/DELETE**. `GET /auth/me` for session checks.
- **Interview flow**: sessions and turns in SQLAlchemy/SQLite (default); answers evaluated via LLM + purpose-built RAG (`backend/app/interview_evaluation.py`, `rag.py`).
- **RAG layer**: Chroma + OpenAI embeddings now power answer evaluation, question generation context, hints, CV role-fit evidence, roadmap/drills, Story Vault semantic rerank, final report evidence, and RAG-vs-no-RAG comparison. It is structured as physical multi-collection RAG: `role_kb`, `company_kb`, `question_kb`, `answer_kb`, `cv_kb`, `evaluation_kb`, `roadmap_kb`, `user_memory_kb`, plus a lightweight Graph-RAG relation layer. The router queries the needed collections per purpose, fuses scores, and keeps legacy `knowledge_base` as fallback. Ingest docs with `cd backend && python3 scripts/ingest_kb.py`.
- **Personal coaching memory**: Interview answers and CV analysis create private `user_memory_items` signals such as weak dimensions, strengths, CV gaps, missing metrics, and red flags. Later hints/questions/evaluations retrieve these signals for personalization.
- **RAG observability**: `GET /rag/inspector/session/{id}` returns query debug info, evidence layers, graph hits, memory signals, retrieval quality, and faithfulness/coverage proxy metrics. The results UI includes a RAG Inspector panel for demos.
- **Quality signals**: reliability / RAG-compare / benchmark routes; methodology: `backend/docs/EVALUATION_METHODOLOGY.md`; golden score bands: `backend/data/golden_scoring_benchmark.json`; reference scenarios: `backend/data/evaluation_reference_scenarios.json`. CI runs deterministic gates + golden JSON validation. Operational notes: `backend/QUALITY.md`.
- **CV**: PDF text via `pypdf`; **keyword + OpenAI embedding fusion** when `CV_USE_EMBEDDING=true` (default); coarse **section detection** (`cv_structure`); optional LLM screening; RAG evidence over role-fit signals is returned as advisory grounding.

## API discovery

- OpenAPI: `http://127.0.0.1:8000/docs` (when backend is running).

## Security & product notes

- **Rate limiting**: Sliding window; **Redis** (`REDIS_URL`) for multi-instance, else DB (`rate_limit_events`) or memory.
- **Origin** allowlist on mutating requests (browser `Origin` header).
- **CSP** and security headers: `frontend/next.config.ts`.
- **Audit**: JSONL in `backend/data/audit.log`.
- **LLM**: Scores are not hiring truth — see `backend/docs/EVALUATION_METHODOLOGY.md`. For variance by question type, append rows to a JSONL and run `python scripts/generate_error_analysis_report.py`.

## Tests & CI

```bash
make test
make typecheck
make lint
# E2E (local only by default — see frontend/e2e/smoke.spec.ts)
cd frontend && npm run test:e2e
```

CI: **pytest** (incl. CSRF + golden regression), **`ci_evaluation_gate.py`**, **`ci_golden_file.py`**, **mypy**, frontend **lint**. Workflow: `.github/workflows/ci.yml`.
