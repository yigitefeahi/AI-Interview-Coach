# Tech Stack — AI Interview Coach

Bu doküman, projede kullanılan teknolojileri, servis seçim gerekçelerini ve geliştirme sürecinde yapay zekanın nasıl kullanıldığını özetler.

## Özet

| Katman | Teknoloji |
|--------|-----------|
| Frontend | Next.js 16, React 19, TypeScript, Tailwind CSS 4 |
| Backend | FastAPI, Python 3.11, SQLAlchemy, Alembic |
| Veritabanı | SQLite (lokal), PostgreSQL (production) |
| AI / LLM | OpenAI API (`gpt-4o-mini`, embeddings, STT, TTS) |
| RAG | ChromaDB + çok katmanlı knowledge base |
| Auth | JWT + HttpOnly cookie + CSRF token |
| Deploy | Render (Blueprint), Docker Compose (lokal) |
| Test | pytest, Playwright, mypy |

---

## Frontend

### Next.js + React + TypeScript
- **Neden:** App Router ile sayfa bazlı routing, SSR/CSR dengesi ve production build kolaylığı.
- **Kullanım:** Dashboard, mülakat akışı (text/audio/video/presence), sonuç raporları, roadmap, settings.

### Tailwind CSS 4
- **Neden:** Hızlı UI geliştirme, tutarlı spacing/renk sistemi, dark mode desteği.
- **Kullanım:** `globals.css` içindeki design token'lar + utility class'lar.

### Diğer frontend kütüphaneleri
| Paket | Amaç |
|-------|------|
| `framer-motion` | Sayfa ve kart animasyonları |
| `recharts` | İlerleme grafikleri (results/dashboard) |
| `zustand` | Hafif client state |
| `react-hook-form` | Form yönetimi (login, register, setup) |
| `axios` | HTTP istekleri (api.ts üzerinden) |
| `@met4citizen/talkinghead` + Three.js | Presence modunda avatar / lip-sync |
| `lucide-react` | İkon seti |
| Playwright | E2E smoke testleri |

---

## Backend

### FastAPI
- **Neden:** Hızlı API geliştirme, otomatik OpenAPI (`/docs`), async uyumu, tip güvenliği.
- **Kullanım:** Auth, mülakat motoru, CV analizi, RAG, analytics, TTS proxy.

### SQLAlchemy + Alembic
- **Neden:** ORM ile session/turn/user modelleri; şema evrimi için migration.
- **Kullanım:** Kullanıcılar, mülakat oturumları, drill tamamlama, rate limit events.

### PostgreSQL / SQLite
- **SQLite:** Lokal geliştirme ve hızlı demo.
- **PostgreSQL:** Render production (eşzamanlı yazma, kalıcılık).

### Passlib + PyJWT
- **Neden:** Şifre hash (bcrypt) ve stateless JWT oturum yönetimi.

### Redis (opsiyonel)
- **Neden:** Çok instance deploy'da paylaşımlı rate limiting.
- **Fallback:** DB veya memory tabanlı rate limit.

---

## Yapay Zeka Katmanı

### OpenAI API
Tüm LLM çağrıları **backend üzerinden** yapılır; API anahtarı frontend'e gitmez.

| Model / servis | Kullanım |
|----------------|----------|
| `gpt-4o-mini` | Soru üretimi, cevap değerlendirme, hint, CV screening |
| `text-embedding-3-small` | RAG embedding, CV eşleştirme |
| `gpt-4o-mini-transcribe` | Sesli cevap → metin |
| `gpt-4o-mini-tts` | Mülakatçı sesi (TTS) |

**Neden OpenAI:** Tek provider ile LLM + embedding + STT + TTS birleşimi; MVP için düşük entegrasyon maliyeti.

### ChromaDB + RAG
- **Neden:** Role/company/question/evaluation knowledge base'lerini vektör aramasıyla bağlamak.
- **Kullanım:** Cevap skorlama, hint, soru üretimi, CV role-fit, roadmap drill önerileri, Story Vault.
- **Ek:** Graph-RAG relation layer + `user_memory_kb` ile kişiselleştirme.

### pypdf + OpenCV (headless)
- **pypdf:** CV PDF metin çıkarımı.
- **OpenCV:** Video modunda temel görüntü sinyalleri.

---

## Altyapı ve Deploy

| Araç | Amaç |
|------|------|
| `render.yaml` | Backend + frontend + PostgreSQL blueprint |
| `docker-compose.yml` | Lokal production-like ortam |
| `Makefile` | `run-backend`, `run-frontend`, test komutları |
| GitHub | Kaynak kod ve teslim |

---

## Geliştirme Sürecinde AI Kullanımı

Bu proje AI destekli geliştirme yaklaşımıyla üretildi:

| Araç / yöntem | Nasıl kullanıldı |
|---------------|------------------|
| **Cursor (AI IDE)** | Endpoint tasarımı, RAG pipeline, React bileşenleri, test iskeletleri |
| **LLM ile planlama** | PRD'den epic/story/task kırılımı (`Jiraiçinplanlama.md`) |
| **AI code review** | CSRF, rate limit, evaluation methodology gözden geçirme |
| **RAG tasarımı** | Rubric ve question seed dosyalarının yapılandırılması |

### Geliştirici kararları (AI ile birlikte)
- LLM skorlarının "işe alım kararı" değil **koçluk sinyali** olduğu netleştirildi.
- Tüm AI çağrıları backend'de toplandı (güvenlik + maliyet kontrolü).
- MVP'de video analizi "basic" tutuldu; uçtan uca akış önceliklendi.
- Multi-collection RAG, tek koleksiyon yerine şirket/rol/soru katmanlarına bölündü.

---

## Neden bu stack?

1. **Hız:** FastAPI + Next.js ile 8 haftalık MVP süresine uygun iterasyon.
2. **Ayrık mimari:** Frontend ileride mobil istemciye de aynı API'yi sunabilir.
3. **AI merkezli:** LLM sadece süs değil; soru, skor, hint ve kişiselleştirmenin çekirdeği.
4. **Gözlemlenebilirlik:** RAG Inspector, golden scoring benchmark, CI evaluation gate.
5. **Deploy edilebilirlik:** Render blueprint ile tek repo'dan canlıya çıkış.

---

## İlgili dosyalar

- `backend/requirements.txt` — Python bağımlılıkları
- `frontend/package.json` — Node bağımlılıkları
- `render.yaml` — Production deploy şablonu
- `backend/docs/EVALUATION_METHODOLOGY.md` — Skorlama metodolojisi
- `DEPLOYMENT.md` — Canlı ortam notları
