# Folder Map — AI Interview Coach

```
AI-Interview-Coach/
├── frontend/                 # Next.js web arayüzü
│   ├── src/app/              # Sayfalar (App Router)
│   ├── src/components/       # UI bileşenleri
│   ├── src/lib/              # api.ts, TTS, session helpers
│   ├── e2e/                  # Playwright testleri
│   └── public/               # Statik asset'ler, talkinghead
│
├── backend/                  # FastAPI API
│   ├── app/                  # Uygulama kodu
│   │   ├── main.py           # Tüm HTTP route'lar
│   │   ├── interview.py      # Mülakat oturum mantığı
│   │   ├── interview_evaluation.py
│   │   ├── rag.py            # RAG retrieval
│   │   └── cv_*.py           # CV pipeline
│   ├── data/
│   │   ├── kb/               # Rol bilgi dosyaları
│   │   └── rag/              # Rubric, soru seed, örnek cevaplar
│   ├── migrations/           # Alembic DB migration
│   ├── scripts/              # ingest_kb, CI gate scriptleri
│   └── tests/                # pytest
│
├── prodocs/                  # AI ajan / dev referansları (bu klasör)
├── docs/                     # OPERATIONS checklist
├── tech-stack.md             # Teknoloji dokümanı
├── DesignSystem.md           # UI tasarım kuralları
├── Progress.md               # Geliştirme günlüğü
├── DEPLOYMENT.md             # Canlı deploy notları
├── render.yaml               # Render blueprint
├── docker-compose.yml        # Lokal Docker
├── .env.example              # Kök env şablonu
└── README.md                 # Onepager
```

## Önemli dosyalar

| Dosya | Ne için |
|-------|---------|
| `frontend/src/lib/api.ts` | Tüm backend çağrıları |
| `frontend/src/app/globals.css` | Design system CSS |
| `backend/app/openai_client.py` | OpenAI client |
| `backend/scripts/ingest_kb.py` | RAG KB yükleme |
| `Makefile` | `run-backend`, `run-frontend`, `test` |
