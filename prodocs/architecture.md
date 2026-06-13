# Mimari — AI Interview Coach

## Genel yapı

```
[Browser / Next.js frontend]
        │  HTTPS + cookies + CSRF
        ▼
[FastAPI backend API]
        │
        ├── SQLAlchemy → PostgreSQL / SQLite
        ├── OpenAI API (LLM, STT, TTS, embeddings)
        └── ChromaDB (RAG vector store)
```

## Katmanlar

### 1. Frontend (`frontend/`)
- **Rol:** UI, form, mülakat ekranları, sonuç görselleştirme.
- **API iletişim:** `src/lib/api.ts` — `NEXT_PUBLIC_API_BASE` üzerinden.
- **Auth:** JWT access token (memory/localStorage) + HttpOnly refresh cookie; mutating isteklerde `X-CSRF-Token`.

### 2. Backend (`backend/app/`)
- **Rol:** İş mantığı, AI orkestrasyonu, veri kalıcılığı.
- **Ana modüller:**
  - `main.py` — route'lar
  - `interview.py` — oturum ve soru akışı
  - `interview_evaluation.py` — LLM skorlama
  - `rag.py` — retrieval ve evidence
  - `cv_*.py` — CV parse, embedding, screening
  - `auth.py`, `csrf.py`, `rate_limit.py` — güvenlik

### 3. Veri
- **İlişkisel DB:** users, sessions, turns, preferences, drill completions.
- **Chroma:** role/company/question/evaluation KB koleksiyonları.
- **Markdown KB:** `backend/data/rag/**`, `backend/data/kb/**`.

## Kullanıcı akışı (mutlu yol)

1. Register / Login
2. Onboarding → CV upload (opsiyonel) → rol seçimi
3. Interview setup → mod seç (text / audio / video / presence)
4. `POST /interview/start` → sorular
5. Cevap gönder (`/interview/answer/text|audio|video`)
6. Backend: RAG context + LLM evaluation → skor + feedback
7. Oturum sonu: `/interview/session/{id}/report`
8. Dashboard'da progress analytics

## AI nerede devreye girer?

| Adım | AI kullanımı |
|------|----------------|
| CV upload | Embedding eşleştirme, LLM screening |
| Soru üretimi | LLM + RAG question_kb |
| Cevap değerlendirme | LLM + RAG evaluation_kb |
| Hint | LLM + user_memory_kb |
| TTS | OpenAI TTS |
| Audio cevap | OpenAI transcribe |

## Güvenlik özeti

- API key sadece backend'de.
- Rate limiting (Redis / DB / memory).
- Origin allowlist mutating isteklerde.
- Audit log: `backend/data/audit.log` (gitignore'da).

## Deploy

- **Render:** `render.yaml` — backend web + frontend web + PostgreSQL.
- **Lokal:** `make run-backend` + `make run-frontend` veya `docker compose up`.
