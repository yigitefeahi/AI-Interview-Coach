---
name: AI Interview Coach plan
overview: "PRD'den türetilmiş kullanıcı hikayeleri ve teknik adımlar. Durumlar Progress.md ile senkronize edilmiştir."
todos:
  - id: confirm-scope
    content: MVP kapsamini netlestir (Interview Coach core + mini learning modulu opsiyonel)
    status: completed
  - id: define-stack
    content: Web teknoloji yigini ve backend mimarisini kesinlestir
    status: completed
  - id: setup-foundation
    content: Proje iskeleti, auth, temel navigasyon ve izin akislarini kur
    status: completed
  - id: build-cv-analysis
    content: CV upload, parsing ve profil cikarim akisni gelistir
    status: completed
  - id: build-interview-engine
    content: Role + CV baglamli soru/follow-up motorunu gelistir
    status: completed
  - id: build-voice-video
    content: Ses/video cevap alma ve temel analiz pipeline'ini ekle
    status: completed
  - id: build-feedback-scoring
    content: Geri bildirim ve skorlama sistemini tamamla
    status: completed
  - id: build-retry-analytics
    content: Retry/iyilestirme dongusu ve temel analitik olaylarini ekle
    status: completed
  - id: release-mvp
    content: Test, kalite kontrolleri ve MVP release hazirligini tamamla
    status: in_progress
isProject: true
---

# Plan — AI Interview Coach

Kaynak PRD: `PRD.md`  
İlerleme günlüğü: `Progress.md`

## Ürün hedefi

Kullanıcının CV'si ve hedef rolüne göre gerçekçi bir mülakat simülasyonu üretmek; ses/video sinyallerini de kullanarak içerik, iletişim ve özgüven odaklı geri bildirim vermek.

---

## Kullanıcı hikayeleri ve teknik adımlar

### Epic 1 — Hesap ve onboarding (Hafta 1–2) ✅

| User Story | Teknik adımlar | Durum |
|------------|----------------|-------|
| Aday olarak kayıt olup giriş yapabilmek istiyorum | `POST /auth/register`, `POST /auth/login`, JWT + HttpOnly cookie, CSRF | ✅ Tamamlandı |
| Uygulamada gezinmek istiyorum | Dashboard, Practice, Roadmap, Stories, Settings nav | ✅ Tamamlandı |
| Oturumum güvenli kalsın | `X-CSRF-Token`, `CORS_ORIGINS`, `credentials: include` | ✅ Tamamlandı |

### Epic 2 — CV analizi (Hafta 3–4) ✅

| User Story | Teknik adımlar | Durum |
|------------|----------------|-------|
| PDF CV yükleyip rol önerisi almak istiyorum | `pypdf` extract, embedding fusion, `POST /cv/suggest` | ✅ Tamamlandı |
| CV'me göre güçlü/zayıf yönlerimi görmek istiyorum | `CV_LLM_SCREENING`, `cv_screening.py` | ✅ Tamamlandı |

### Epic 3 — Mülakat motoru (Hafta 3–4) ✅

| User Story | Teknik adımlar | Durum |
|------------|----------------|-------|
| Hedef role göre mülakat başlatmak istiyorum | `POST /interview/start`, role/company packs | ✅ Tamamlandı |
| CV'me uygun dinamik sorular almak istiyorum | LLM + RAG `question_kb`, follow-up engine | ✅ Tamamlandı |
| Metin cevabımı gönderip anında geri bildirim almak istiyorum | `POST /interview/answer/text`, `interview_evaluation.py` | ✅ Tamamlandı |
| Takıldığımda hint istemek istiyorum | `POST /interview/hint`, `user_memory_kb` | ✅ Tamamlandı |

### Epic 4 — RAG ve kişiselleştirme (Hafta 5–6) ✅

| User Story | Teknik adımlar | Durum |
|------------|----------------|-------|
| Değerlendirmenin role/şirkete göre ground edilmesini istiyorum | Chroma multi-collection RAG, `ingest_kb.py` | ✅ Tamamlandı |
| Geçmiş cevaplarımdan öğrenen bir koç istiyorum | `user_memory_items`, `user_memory_kb` | ✅ Tamamlandı |
| Demo/debug için RAG kanıtını görmek istiyorum | RAG Inspector panel, `/rag/inspector/session/{id}` | ✅ Tamamlandı |

### Epic 5 — Ses, video, Presence (Hafta 7) ✅

| User Story | Teknik adımlar | Durum |
|------------|----------------|-------|
| Sesli cevap vermek istiyorum | `POST /interview/answer/audio`, OpenAI transcribe | ✅ Tamamlandı |
| Kamera ile pratik yapmak istiyorum | `POST /interview/answer/video`, temel non-verbal sinyal | ✅ Tamamlandı |
| Avatar ile mülakat deneyimi yaşamak istiyorum | Presence modu, TalkingHead, `POST /tts` | ✅ Tamamlandı |

### Epic 6 — Skor, retry, analytics (Hafta 8) ✅

| User Story | Teknik adımlar | Durum |
|------------|----------------|-------|
| Oturum sonunda detaylı rapor görmek istiyorum | `/interview/session/{id}/report`, results sayfası | ✅ Tamamlandı |
| İlerlememi takip etmek istiyorum | `/analytics/progress`, dashboard grafikleri | ✅ Tamamlandı |
| Haftalık drill'lerle gelişmek istiyorum | Roadmap, weekly drills, drill completions | ✅ Tamamlandı |
| STAR hikâyelerimi kaydetmek istiyorum | Story Vault (`/stories`) | ✅ Tamamlandı |

### Epic 7 — Teslim ve release (devam ediyor) 🔄

| User Story | Teknik adımlar | Durum |
|------------|----------------|-------|
| Uygulama canlıda çalışsın | Render blueprint, env ayarları, health check | 🔄 Devam ediyor |
| Teslim dokümanları tam olsun | PRD, Plan, tech-stack, Progress, prodocs | ✅ Tamamlandı |
| Demo videosu hazır olsun | Loom/YouTube kayıt | ⏳ Bekliyor |

---

## Uygulanan teknik mimari

- **Frontend:** Next.js 16 + React 19 + TypeScript + Tailwind CSS 4
- **Backend:** FastAPI + SQLAlchemy + Alembic
- **Veritabanı:** SQLite (lokal), PostgreSQL (production)
- **AI:** OpenAI API (LLM, embedding, STT, TTS) — tüm çağrılar backend'de
- **RAG:** ChromaDB, multi-collection knowledge base
- **Deploy:** `render.yaml`, `docker-compose.yml`

---

## Geliştirme fazları (özet)

| Faz | Kapsam | Durum |
|-----|--------|-------|
| Faz 1 | Auth, navigasyon, onboarding | ✅ |
| Faz 2 | CV upload ve profil çıkarımı | ✅ |
| Faz 3 | Dinamik mülakat motoru | ✅ |
| Faz 4 | Ses ve video pipeline | ✅ |
| Faz 5 | Feedback ve skorlama | ✅ |
| Faz 6 | Retry, analitik, release | 🔄 (canlı deploy + demo) |

---

## Başarı kriterleri (ilk teslim)

- [x] Kullanıcı CV yükleyip hedef rol seçerek mülakat başlatabiliyor
- [x] Sistem follow-up içeren oturumu tamamlatabiliyor
- [x] Sesli cevaplar transcript'e çevriliyor ve değerlendirmede kullanılıyor
- [x] Video/ses sinyalleri temel feedback'e dahil ediliyor
- [x] Kullanıcı skorlarını ve iyileştirme önerilerini görüyor
- [x] Retry sonrası güncel performans farkı gösteriliyor
- [ ] Canlı URL'de uçtan uca akış çalışıyor

---

## MVP sonrası (opsiyonel)

- Şirket bazlı mülakat simülasyonları (kısmen RAG rubric'lerde mevcut)
- Gelişmiş davranış analizi
- Çoklu dil desteği
- Mini-LMS öğrenme patikası
- Koçluk paneli
