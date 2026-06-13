# API Overview — AI Interview Coach

Base URL: `NEXT_PUBLIC_API_BASE` (ör. `http://localhost:8000`)

OpenAPI (canlı): `/docs`

## Health

| Method | Path | Açıklama |
|--------|------|----------|
| GET | `/health` | Basit sağlık kontrolü |
| GET | `/health/deps` | Bağımlılık durumu |

## Auth

| Method | Path | Açıklama |
|--------|------|----------|
| POST | `/auth/register` | Yeni kullanıcı |
| POST | `/auth/login` | Giriş |
| GET | `/auth/me` | Oturum kontrolü |
| POST | `/auth/logout` | Çıkış |

**Not:** POST/PUT/PATCH/DELETE isteklerinde `X-CSRF-Token` header gerekir.

## CV

| Method | Path | Açıklama |
|--------|------|----------|
| POST | `/cv/suggest` | CV'den rol önerisi |

## Interview

| Method | Path | Açıklama |
|--------|------|----------|
| POST | `/interview/start` | Yeni oturum başlat |
| POST | `/interview/answer/text` | Metin cevap |
| POST | `/interview/answer/audio` | Sesli cevap |
| POST | `/interview/answer/video` | Video cevap |
| POST | `/interview/pass` | Soruyu pas geç |
| POST | `/interview/hint` | AI hint al |
| GET | `/interview/session/{id}` | Oturum durumu |
| GET | `/interview/session/{id}/report` | Final rapor |

## Roadmap & drills

| Method | Path | Açıklama |
|--------|------|----------|
| GET | `/interview/roadmap` | Kişisel yol haritası |
| GET | `/interview/weekly-drills` | Haftalık drill listesi |
| GET/PUT | `/interview/drill-completions` | Tamamlama durumu |

## RAG & kalite

| Method | Path | Açıklama |
|--------|------|----------|
| GET | `/rag/inspector/session/{id}` | RAG debug (auth gerekli) |
| GET | `/rag/memory` | Kullanıcı memory sinyalleri |
| POST | `/interview/evaluate/rag-compare` | RAG vs no-RAG karşılaştırma |

## Analytics & hesap

| Method | Path | Açıklama |
|--------|------|----------|
| GET | `/analytics/progress` | İlerleme metrikleri |
| GET | `/account/summary` | Hesap özeti |
| GET/PUT | `/account/preferences` | Kullanıcı tercihleri |

## Yardımcı

| Method | Path | Açıklama |
|--------|------|----------|
| GET | `/professions` | Rol listesi |
| GET | `/sectors` | Sektör listesi |
| POST | `/tts` | Text-to-speech proxy |
| GET | `/stories` | Story Vault listesi |
| POST | `/stories` | Yeni hikâye ekle |

## Frontend sayfa eşlemesi

| Sayfa | İlgili API |
|-------|------------|
| `/login`, `/register` | `/auth/*` |
| `/onboarding` | `/cv/suggest` |
| `/interview/setup` | `/interview/start` |
| `/interview/live` | `/interview/answer/text`, `/hint` |
| `/interview/video` | `/interview/answer/video` |
| `/interview/presence` | `/tts`, audio/video endpoints |
| `/results/[sessionId]` | `/interview/session/{id}/report` |
| `/dashboard` | `/analytics/progress` |
| `/roadmap` | `/interview/roadmap`, drills |
