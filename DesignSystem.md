# Design System — AI Interview Coach

Bu doküman, uygulamanın görsel dilini tanımlar: renk paleti, tipografi, spacing ve temel bileşen kuralları.

Kaynak dosya: `frontend/src/app/globals.css` ve `frontend/src/app/layout.tsx`.

---

## Tasarım ilkeleri

1. **Sakin ve odaklı:** Mülakat stresini artırmayan, sıcak nötr tonlar.
2. **Cam efekti (glass):** Kartlar hafif blur ve yumuşak gölge ile öne çıkar.
3. **Yuvarlak formlar:** Butonlar ve input'lar pill (999px radius) stilinde.
4. **Accent vurgusu:** Altın/bronz accent (`#d89d5b`) CTA ve gradient'lerde kullanılır.
5. **Dark mode:** `data-theme="dark"` ile tam tema desteği.

---

## Renk paleti

### Light mode (`:root`)

| Token | Değer | Kullanım |
|-------|-------|----------|
| `--background` | `#fbfaf7` | Sayfa arka planı |
| `--foreground` | `#1f2933` | Ana metin |
| `--card` | `rgba(255,255,255,0.92)` | Kart / panel yüzeyi |
| `--card-soft` | `#f7f3ed` | İkincil yüzey |
| `--card-border` | `rgba(31,41,51,0.1)` | Kenarlık |
| `--muted` | `#6b7280` | Yardımcı metin |
| `--primary` | `#111827` | Birincil buton / marka koyu |
| `--primary-hover` | `#2f3745` | Hover durumu |
| `--secondary` | `#f4efe7` | İkincil arka plan |
| `--accent` | `#d89d5b` | Vurgu rengi |
| `--danger` | `#dc2626` | Hata / uyarı |

### Dark mode (`:root[data-theme="dark"]`)

| Token | Değer |
|-------|-------|
| `--background` | `#0d111b` |
| `--foreground` | `#f5efe7` |
| `--card` | `rgba(20,25,38,0.92)` |
| `--card-soft` | `#171d2b` |
| `--primary` | `#f5efe7` |
| `--accent` | `#d89d5b` |
| `--danger` | `#fb7185` |

### Gradient
- **Marka gradient:** `#111827 → #9a6a37 → #d89d5b` (`.gradient-text`)
- **Sayfa arka planı:** Radial gradient + linear gradient (light/dark ayrı)

---

## Tipografi

| Öğe | Font | Not |
|-----|------|-----|
| Body | **Geist Sans** (`--font-geist-sans`) | Google Fonts, `layout.tsx` |
| Mono / kod | **Geist Mono** (`--font-geist-mono`) | Sayısal veriler, teknik etiketler |
| Fallback | Arial, Helvetica, sans-serif | Font yüklenmezse |

### Hiyerarşi (uygulama geneli)
- **Sayfa başlığı:** `text-xl` – `text-3xl`, `font-extrabold` / `font-semibold`
- **Bölüm etiketi:** `.step-label` — küçük, uppercase hissi
- **Gövde metni:** Varsayılan body, `--foreground`
- **Muted açıklama:** `--muted` rengi, daha küçük punto

---

## Spacing ve layout

| Kural | Değer |
|-------|-------|
| Container max genişlik | `1120px` (`.container`) |
| Yan padding | `32px` toplam (`calc(100% - 32px)`) |
| Kart padding | Genelde `16px` – `24px` |
| Buton padding | `12px 20px` |
| Input padding | `14px 16px` |
| Nav spacer | `.app-shell-spacer` — sabit üst boşluk |

---

## Bileşen kuralları

### Butonlar

**Primary (`.btn-primary`)**
- Arka plan: `var(--primary)`
- Border-radius: `999px` (pill)
- Font-weight: `600`
- Hover: `translateY(-1px)`, `primary-hover`
- Disabled: `opacity: 0.55`, pointer yok

**Secondary (`.btn-secondary`)**
- Beyaz / dark'ta `#171d2b` arka plan
- İnce border: `var(--card-border)`
- Aynı pill radius ve hover davranışı

### Input (`.input-dark`)
- Border-radius: `18px`
- Arka plan: `#fffdf9` (light), dark'ta koyu yüzey
- Focus: accent border vurgusu
- Tam genişlik form alanlarında

### Kart / panel (`.glass`)
- Yarı saydam arka plan
- `backdrop-filter: blur(18px)`
- Gölge: `0 24px 70px rgba(31,41,51,0.08)`
- Border: `1px solid var(--card-border)`

### Skor rozeti
- Bileşen: `frontend/src/components/score-badge.tsx`
- Skor bandına göre renk (düşük/orta/yüksek)

### Navigasyon (`app-nav.tsx`)
- Üst sabit shell: brand + link grubu
- Aktif link: `.app-shell-link-active`
- Gizlenen rotalar: `/`, `/login`, `/register`, `/onboarding`, `/interview/presence`

### Tema toggle
- `theme-toggle.tsx` — `localStorage` key: `aicoach-theme`
- `html` üzerinde `data-theme` attribute

---

## İkonografi

- **Kütüphane:** `lucide-react`
- **Boyut:** Nav'da `15px`, kartlarda `18–24px` arası
- **Stil:** Stroke, minimal — metinle aynı renk ailesi

---

## Motion

- **Framer Motion:** Sayfa girişleri, kart stagger
- **Transition süresi:** `0.18s ease` (buton/input hover)
- **Prensip:** Abartısız; mülakat ekranında dikkat dağıtmayan mikro animasyon

---

## Erişilebilirlik notları

- Nav'da `aria-label="Primary navigation"`
- Buton disabled state görsel + cursor ile belirtilir
- Kontrast: primary buton metni beyaz (light), dark'ta koyu metin

---

## Yeni bileşen eklerken

1. Önce mevcut token'ları kullan (`--primary`, `--accent`, `.glass`).
2. Yeni renk eklemeden önce palete uyumu kontrol et.
3. Border-radius: butonlarda `999px`, inputlarda `18px`, büyük panellerde `20–24px`.
4. Dark mode için `:root[data-theme="dark"]` override yaz.
5. Animasyon süresi `0.18s` civarında tut.
