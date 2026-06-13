# Progress — AI Interview Coach

Geliştirme sürecinde alınan kararlar, tamamlanan işler ve karşılaşılan sorunların kaydı.

---

## Hafta 1–2: Temel iskelet ve auth

### Yapılanlar
- Monorepo yapısı kuruldu: `frontend/` (Next.js) + `backend/` (FastAPI).
- JWT auth, register/login/logout akışı tamamlandı.
- HttpOnly cookie + stateless CSRF token modeli seçildi.
- Temel navigasyon: Dashboard, Practice, Roadmap, Stories, Settings.

### Kararlar
- Frontend doğrudan OpenAI'ye bağlanmayacak; tüm LLM çağrıları backend'de.
- SQLite ile hızlı lokal geliştirme; production'da PostgreSQL.

### Sorunlar
- CORS + cookie kombinasyonu ilk denemede session kaybına yol açtı.
- **Çözüm:** `CORS_ORIGINS` explicit ayarlandı, `credentials: include` frontend'de sabitlendi.

---

## Hafta 3–4: CV analizi ve mülakat motoru

### Yapılanlar
- PDF CV upload ve metin çıkarımı (`pypdf`).
- Keyword + embedding fusion ile rol önerisi.
- Dinamik soru üretimi ve follow-up akışı.
- Text cevap gönderme ve LLM tabanlı değerlendirme.

### Kararlar
- CV screening için tek LLM pass: güçlü/zayıf yön + role fit.
- Skorlar rubric tabanlı; "işe alım kararı" değil koçluk sinyali olarak konumlandırıldı.

### Sorunlar
- İlk skorlama prompt'ları soru tipine göre çok değişken sonuç veriyordu.
- **Çözüm:** `EVALUATION_METHODOLOGY.md` ve golden scoring benchmark eklendi.

---

## Hafta 5–6: RAG ve kişiselleştirme

### Yapılanlar
- ChromaDB ile multi-collection RAG: `role_kb`, `company_kb`, `question_kb`, vb.
- Şirket/rol rubric ve question seed dosyaları ingest edildi.
- `user_memory_kb` ile geçmiş cevaplardan kişiselleştirme sinyalleri.
- RAG Inspector paneli (demo ve debug için).

### Kararlar
- Tek knowledge base yerine amaç bazlı koleksiyonlar (evaluation vs hint vs question gen).
- Graph-RAG relation layer hafif tutuldu; ana ağırlık vektör retrieval'da.

### Sorunlar
- Filtreli retrieval bazen boş dönüyordu.
- **Çözüm:** Metadata filter + safe fallback (legacy `knowledge_base`).

---

## Hafta 7: Ses, video ve Presence modu

### Yapılanlar
- Audio cevap: STT → metin → aynı evaluation pipeline.
- Video modu: temel kamera kaydı ve non-verbal sinyal.
- Presence modu: TalkingHead avatar, TTS, lip-sync.
- TTS endpoint (`POST /tts`) backend proxy olarak eklendi.

### Kararlar
- Video analizi MVP'de "basic" seviyede; mükemmel CV/face tracking yerine uçtan uca akış.
- Browser TTS fallback, OpenAI TTS başarısız olursa.

### Sorunlar
- Render free tier'da cold start backend timeout.
- **Çözüm:** Health check path, kullanıcıya "backend başlatılıyor" mesajı (planlanan).

---

## Hafta 8: Analytics, roadmap ve teslim hazırlığı

### Yapılanlar
- Dashboard progress analytics (`/analytics/progress`).
- Roadmap ve weekly drills tamamlama takibi.
- Story Vault (STAR hikâyeleri).
- Results sayfası: skor grafikleri, RAG evidence, export.
- `render.yaml` + `DEPLOYMENT.md` ile deploy şablonu.
- pytest + Playwright smoke testleri.

### Kararlar
- Teslim repo adı: `AI-Interview-Coach`.
- Dokümanlar repo kökünde standart isimlerle toplanacak.

### Devam eden / teslim öncesi
- [x] Canlı Render deploy (frontend + backend uçtan uca)
- [ ] Demo videosu
- [ ] Teslim formu

---

## Teknik borç (bilinçli)

| Konu | Durum | Not |
|------|-------|-----|
| Chroma persist on Render free disk | İzleniyor | KB ingest deploy sonrası job |
| `Plan.md` todo statüleri | Tamamlandı | Progress.md ile senkron (2026-06-13) |
| CI workflow | Eklenebilir | README'de referans var |

---

## Öğrenilenler

1. AI ürününde en zor kısım prompt değil, **güvenilir veri akışı** (auth, session, RAG context).
2. RAG'i erken "gözlemlenebilir" yapmak (Inspector) demo ve debug'u çok hızlandırdı.
3. LLM skorları için golden benchmark olmadan regresyon kontrolü zor.
4. Ayrı frontend/backend, mobil istemci eklemeyi kolaylaştırır.

---

## Son güncelleme

- **Tarih:** 2026-06-13
- **Son commit odağı:** Results report UX, Presence modu, roadmap iyileştirmeleri
