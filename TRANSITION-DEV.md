# Transitions.dev — Tüm Markdown Dosyaları Analiz Paketi

Kaynak repo: `Jakubantalik/transitions.dev`  
Kapsam: Repo içindeki ana `README.md`, agent skill dokümantasyonu `SKILL.md` ve `skills/transitions-dev/` altındaki 12 geçiş referans Markdown dosyası.

---

## 1. Genel Değerlendirme

`transitions.dev`, web arayüzlerinde sık kullanılan mikro-etkileşimler için hazırlanmış, framework bağımsız CSS transition / animation katalogudur. Repo iki ana kullanım senaryosuna hizmet eder:

1. **Showcase site:** `index.html`, `prototypes.html`, `skill.html`, `example.html` üzerinden geçişleri canlı gösterir.
2. **Agent skill paketi:** `skills/transitions-dev/` altındaki Markdown referansları, AI coding araçlarının bu geçişleri projelere doğru şekilde uygulamasını sağlar.

Markdown dokümantasyonunun ana fikri şudur:

- Her transition bağımsız ve taşınabilir olmalı.
- Selector’lar `t-*` namespace’iyle çakışma riskini azaltmalı.
- Değerler semantic CSS custom property’ler üzerinden yönetilmeli.
- Her snippet `prefers-reduced-motion` guard içermeli.
- JS gereken yerlerde küçük orchestration snippet’i verilmeli.
- Demo’ya özgü markup, sizing, renk veya framework bağımlılığı olmamalı.

---

## 2. Markdown Envanteri

| Dosya | Rol | İçerik Tipi |
|---|---|---|
| `README.md` | Repo tanıtımı | Proje amacı, canlı site, 12 transition listesi, skill kullanımı, dosya yapısı, lokal çalıştırma |
| `skills/transitions-dev/SKILL.md` | Agent skill ana dosyası | Skill metadata, karar kuralları, komutlar, universal install, output format, common mistakes |
| `01-card-resize.md` | Transition referansı | Kart / container boyut geçişi |
| `02-number-pop-in.md` | Transition referansı | Sayı / digit giriş animasyonu |
| `03-notification-badge.md` | Transition referansı | Notification badge slide + pop |
| `04-text-states-swap.md` | Transition referansı | Metin state değişimi |
| `05-menu-dropdown.md` | Transition referansı | Anchor-aware dropdown aç/kapa |
| `06-modal.md` | Transition referansı | Modal scale open / close |
| `07-panel-reveal.md` | Transition referansı | Panel reveal / slide |
| `08-page-side-by-side.md` | Transition referansı | İki sayfa arasında side-by-side slide |
| `09-icon-swap.md` | Transition referansı | Aynı slotta ikon değişimi |
| `10-success-check.md` | Transition referansı | Success check appear + SVG path draw |
| `11-avatar-group-hover.md` | Transition referansı | Avatar/chip stack hover falloff |
| `12-error-state-shake.md` | Transition referansı | Form error shake + auto revert |

---

## 3. Ortak Mimari

### 3.1 Namespace yaklaşımı

Tüm transition selector’ları `t-*` prefix’iyle gelir:

- `.t-resize`
- `.t-digit-group`, `.t-digit`
- `.t-badge`, `.t-badge-dot`
- `.t-text-swap`
- `.t-dropdown`
- `.t-modal`
- `.t-panel-slide`
- `.t-page-slide`, `.t-page`
- `.t-icon-swap`, `.t-icon`
- `.t-success-check`
- `.t-avatar-group`, `.t-avatar`
- `.t-input-wrap`, `.t-input`, `.t-error-msg`

Bu yaklaşım, mevcut projelerdeki class isimleriyle çakışma riskini düşürür. Dokümantasyon, snippet’lerin selector’larının değiştirilmemesini özellikle vurgular.

### 3.2 Semantic CSS değişkenleri

Skill, live demo’daki `--pX-*` token’larını kullanıcıya açmak yerine semantic adlara dönüştürür:

- `--badge-*`
- `--dropdown-*`
- `--modal-*`
- `--panel-*`
- `--page-*`
- `--icon-swap-*`
- `--check-*`
- `--avatar-*`
- `--shake-*`

Bu sayede kullanıcı projede geçişleri daha okunabilir ve sürdürülebilir değişken adlarıyla yönetir.

### 3.3 Accessibility yaklaşımı

Her transition snippet’i `@media (prefers-reduced-motion: reduce)` bloğu içerir. Bu bloklar şu amaçlara hizmet eder:

- Kullanıcının işletim sistemi düzeyindeki azaltılmış hareket tercihini dikkate almak.
- Transition / animation’ı tamamen kapatmak veya statik son duruma almak.
- Hareket hassasiyeti olan kullanıcılar için daha güvenli bir UI davranışı sağlamak.

### 3.4 JS orchestration modeli

Bazı transition’lar sadece CSS state toggle ile çalışır. Bazılarında animasyonu yeniden başlatmak, closing state’i temizlemek veya inline custom property yazmak için JS gerekir.

| Transition | JS Gerekir mi? | Sebep |
|---|---:|---|
| Card resize | Hayır | Width/height state değişimi CSS transition ile yeterli |
| Number pop-in | Evet | Digit DOM yeniden üretimi ve reflow gerekir |
| Notification badge | Hayır | `data-open` toggle yeterli |
| Text states swap | Evet | Çıkış, text swap, giriş fazları gerekir |
| Menu dropdown | Evet | `.is-closing` cleanup gerekir |
| Modal | Evet | `.is-closing` cleanup gerekir |
| Panel reveal | Hayır | `data-open` toggle yeterli |
| Page side-by-side | Basit JS | `data-page` değiştirilir |
| Icon swap | Hayır | `data-state` toggle yeterli |
| Success check | Evet | Replay için reset + reflow gerekir |
| Avatar group hover | Evet | Distance falloff ve inline timing gerekir |
| Error state shake | Evet | Shake replay, timer, auto revert gerekir |

---

## 4. README.md Analizi

### Amaç

README, projeyi kısa ve pratik şekilde tanıtır. Ana mesaj: `Transitions.dev`, tekrar kullanılabilir CSS transition koleksiyonudur; her kart bir interaction pattern gösterir ve kopyalanabilir portable CSS snippet üretir.

### Öne çıkan noktalar

- Canlı site: `https://transitions.dev/`
- 12 transition tek tabloda listelenir.
- Copy button mantığı anlatılır.
- Snippet’lerin self-contained olduğu vurgulanır.
- Skill olarak kurulabileceği belirtilir:

```bash
npx skills add Jakubantalik/transitions.dev
```

### Teknik yapı

README’ye göre skill içeriği `skills/transitions-dev/` altında bulunur. `build/extract.mjs`, `index.html` içindeki `PROTO_TEMPLATES` ve `:root { --pX-* }` bloklarını parse ederek skill dosyalarını üretir. Bu, showcase site ile skill referanslarının aynı kaynak üzerinden güncel kalmasını sağlar.

### Dosya yapısı

README şu ana dosyaları açıklar:

- `index.html`: Showcase page ve copy CSS butonları.
- `prototypes.html`: Canlı tuning kontrolleri.
- `skill.html`: Agent skill landing page.
- `example.html`: Generic AI output vs skill output karşılaştırması.
- `skills/transitions-dev/`: Yayınlanan skill payload.
- `build/extract.mjs` + `build/templates/`: Skill üretim sistemi.
- `assets/`: Görseller ve ikonlar.
- `site.webmanifest`, `robots.txt`, `sitemap.xml`: PWA / SEO metadata.

---

## 5. SKILL.md Analizi

### Rolü

`SKILL.md`, agent skill’in ana kontrol dosyasıdır. AI coding araçlarına ne zaman bu skill’i kullanacaklarını, hangi transition’ı seçeceklerini ve projeye nasıl uygulayacaklarını öğretir.

### Metadata

Dosya YAML frontmatter ile başlar:

- `name: transitions-dev`
- `description:` içinde skill’in kullanım alanları ve trigger phrase’leri yer alır.

Trigger örnekleri:

- “add a transition”
- “animate the dropdown”
- “make the modal open smoothly”
- “swap icon”
- “page slide”
- “shake on invalid”
- “avatar stack hover”

### Quick reference

12 transition, “When to use” ve referans dosyasıyla listelenir. Bu tablo skill’in ana katalog görünümüdür.

### Decision rules

En önemli bölüm budur. Agent’ın transition seçerken izlemesi gereken kuralları verir:

- Trigger üstünde küçük dot → notification badge.
- Trigger’dan büyüyen surface → dropdown.
- Centered overlay → modal.
- Region içine kayan panel → panel reveal.
- Width/height değişimi → card resize.
- Metin değişimi → text states swap.
- Aynı slotta iki ikon → icon swap.
- Sayı değişimi → number pop-in.
- Confirmation / done moment → success check.
- Horizontal stack hover → avatar group hover.
- Form validation error → error state shake.

Belirsizlikte “tahmin etme” mantığı vardır: net eşleşme yoksa kullanıcıya katalog gösterilir.

### Komutlar

Skill üç komutu tarif eder:

1. `transitions reveal`  
   Tüm transition katalogunu listeler.

2. `transitions review`  
   Projedeki mevcut animation / transition adaylarını tarar, uygun transition önerir, değişiklik yapmaz.

3. `transitions apply`  
   Bağlama göre en uygun transition’ı seçer, kullanıcı onayıyla projeye uygular.

### Universal install

`SKILL.md`, tüm semantic CSS değişkenlerini içeren geniş bir `:root` bloğu sunar. Bu blok projeye bir kez eklenmelidir. Her transition aynı kök değişken setinden beslenebilir.

### Output format

Transition eklerken agent’ın izlemesi gereken 5 temel kural:

1. Universal `:root` bloğunu yalnızca bir kez ekle.
2. Seçilen transition CSS’ini birebir ekle.
3. HTML hook’larını doğru bağla.
4. Reduced motion guard’ı koru.
5. JS gereken transition’larda timing değerlerini CSS değişkenlerinden oku.

### Common mistakes

Bu bölüm pratik kalite kontrol listesi gibidir. Öne çıkan uyarılar:

- Dropdown / modal close cleanup silinmemeli.
- Replay gereken animasyonlarda reflow unutulmamalı.
- `transition: all` kullanılmamalı.
- Success check path length hardcoded bırakılmamalı.
- Avatar hover’da timing-function CSS’te sabitlenmemeli.
- Error shake için `.is-error` ve `.is-shaking` ayrı tutulmalı.

---

## 6. Transition Dosyaları Detaylı Analiz

## 6.1 `01-card-resize.md` — Card resize

### Kullanım amacı

Container width veya height değiştiğinde yumuşak resize geçişi sağlamak için kullanılır. Compact / expanded kart, açılır detay satırı veya küçük panel boyut değişimleri için uygundur.

### HTML hook

```html
<div class="t-resize">…</div>
```

### State modeli

Bu transition doğrudan class veya style üzerinden width/height değişimine tepki verir. Özel `data-*` attribute gerekmez.

### CSS mantığı

- `width` ve `height` ayrı ayrı transition edilir.
- `will-change: width, height` kullanılır.
- Reduced motion’da transition kapatılır.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--resize-dur` | `300ms` |
| `--resize-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |

### Güçlü yan

En düşük maliyetli transition’dır. JS gerektirmez ve mevcut layout state değişimlerine kolay eklenir.

### Dikkat edilmesi gerekenler

`width` / `height` animasyonu bazı layout’larda performans maliyeti yaratabilir. Çok büyük listelerde veya sık tetiklenen resize senaryolarında ölçülü kullanılmalı.

---

## 6.2 `02-number-pop-in.md` — Number pop-in

### Kullanım amacı

Sayılar güncellendiğinde her karakterin ayrı ayrı blur + translate ile yeniden girmesini sağlar. Counter, fiyat, bakiye, skor veya metrik kartları için uygundur.

### HTML hook

```html
<span class="t-digit-group is-animating">
  <span class="t-digit">1</span>
  <span class="t-digit">2</span>
  <span class="t-digit" data-stagger="1">.</span>
  <span class="t-digit" data-stagger="2">3</span>
</span>
```

### State modeli

- `.is-animating` animasyonu başlatır.
- Her karakter `.t-digit` olarak ayrı span içinde olmalıdır.
- `data-stagger` ile gecikme verilebilir.

### CSS mantığı

- `@keyframes t-digit-pop-in` kullanır.
- Başlangıçta translate + opacity 0 + blur vardır.
- Son durumda translate 0 + opacity 1 + blur 0 olur.
- Direction, `--digit-dir-x` ve `--digit-dir-y` ile ayarlanır.

### JS mantığı

- `.is-animating` kaldırılır.
- Digit span’leri yeniden oluşturulur.
- Reflow zorlanır: `void group.offsetHeight`.
- `.is-animating` tekrar eklenir.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--digit-dur` | `500ms` |
| `--digit-distance` | `8px` |
| `--digit-stagger` | `70ms` |
| `--digit-blur` | `2px` |
| `--digit-ease` | `cubic-bezier(0.34, 1.45, 0.64, 1)` |
| `--digit-dir-x` | `0` |
| `--digit-dir-y` | `1` |

### Güçlü yan

Dashboard ve metrik kartlarında algılanan canlılığı artırır. Her digit ayrı olduğu için daha rafine bir hareket hissi verir.

### Dikkat edilmesi gerekenler

Metni doğrudan tek node olarak değiştirmek yeterli değildir; karakterleri ayrı span’lere bölmek gerekir.

---

## 6.3 `03-notification-badge.md` — Notification badge

### Kullanım amacı

Bell, inbox veya button gibi trigger üzerinde küçük bir badge göstermek için kullanılır. Badge wrapper çapraz slide eder, iç dot bağımsız scale / opacity / blur ile pop eder.

### HTML hook

```html
<button class="your-trigger" style="position: relative">
  <span class="t-badge" data-open="false">
    <span class="t-badge-dot">1</span>
  </span>
</button>
```

### State modeli

- `.t-badge[data-open="true"]` gösterim durumudur.
- `.t-badge[data-open="false"] .t-badge-dot` kapalı durumdur.
- Trigger hareket etmez; sadece badge hareket eder.

### CSS mantığı

- Wrapper: `position: absolute`, `top`, `right` ile anchor edilir.
- `t-badge-slide-in` keyframe ile wrapper konuma gelir.
- Dot scale 0 → 1 ve opacity 0 → 1 davranışı gösterir.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--badge-slide-dur` | `260ms` |
| `--badge-pop-dur` | `500ms` |
| `--badge-pop-close-dur` | `180ms` |
| `--badge-fade-dur` | `400ms` |
| `--badge-fade-close-dur` | `180ms` |
| `--badge-blur` | `2px` |
| `--badge-offset-x` | `-8.2px` |
| `--badge-offset-y` | `12.4px` |
| `--badge-slide-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| `--badge-pop-ease` | `cubic-bezier(0.34, 1.36, 0.64, 1)` |
| `--badge-close-ease` | `cubic-bezier(0.4, 0, 0.2, 1)` |

### Güçlü yan

Trigger’dan bağımsız hareket ettiği için button / icon layout’u bozulmaz.

### Dikkat edilmesi gerekenler

Trigger element `position: relative` olmalıdır. Aksi halde badge beklenen noktaya anchor olmaz.

---

## 6.4 `04-text-states-swap.md` — Text states swap

### Kullanım amacı

Aynı yerde duran bir metnin state değişimini yumuşatır. Örnekler: “Processing…” → “Done”, “Save” → “Saved”.

### HTML hook

```html
<span class="t-text-swap">Processing…</span>
```

### State modeli

Üç fazlıdır:

1. `.is-exit`: Eski metin yukarı çıkar, blur olur, opacity düşer.
2. Text içerik değiştirilir ve `.is-enter-start` eklenir.
3. Reflow sonrası `.is-enter-start` kaldırılır, yeni metin aşağıdan yerine gelir.

### CSS mantığı

- `transform`, `filter`, `opacity` transition edilir.
- `.is-enter-start` transition’ı geçici olarak kapatır.
- Bu sayede yeni text başlangıç pozisyonuna “zıplar”, sonra animate edilir.

### JS mantığı

- CSS değişkeninden duration okunur.
- Eski metin exit’e alınır.
- Timeout sonrası text değiştirilir.
- Reflow ile yeni giriş animasyonu başlatılır.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--text-swap-dur` | `150ms` |
| `--text-swap-translate-y` | `4px` |
| `--text-swap-blur` | `2px` |
| `--text-swap-ease` | `ease-in-out` |

### Güçlü yan

Kısa metin state değişimlerinde sade ve temiz bir mikro-etkileşim sağlar.

### Dikkat edilmesi gerekenler

Text değişimi transition’ın ortasında yapılmalıdır; aksi halde eski / yeni metin fazları ayrışmaz.

---

## 6.5 `05-menu-dropdown.md` — Menu dropdown

### Kullanım amacı

Trigger’dan açılan dropdown, contextual menu veya popover için kullanılır. Origin-aware olması sayesinde yüzeyin hangi noktadan büyüdüğü kontrol edilir.

### HTML hook

```html
<div class="t-dropdown" data-origin="top-center">
  <!-- menu contents -->
</div>
```

### State modeli

- `.is-open`: Açık durum.
- `.is-closing`: Kapanış animasyonu.
- Kapanış sonrası `.is-closing` timeout ile temizlenir.

### Origin seçenekleri

- `top-left`
- `top-center`
- `top-right`
- `bottom-left`
- `bottom-center`
- `bottom-right`

### CSS mantığı

- Kapalı başlangıç scale: `--dropdown-pre-scale`
- Açık scale: `1`
- Kapanış scale: `--dropdown-closing-scale`
- Opacity ve pointer-events state’e göre değişir.

### JS mantığı

- Open: `.is-closing` kaldır, `.is-open` ekle.
- Close: `.is-open` kaldır, `.is-closing` ekle, süre sonunda `.is-closing` kaldır.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--dropdown-open-dur` | `250ms` |
| `--dropdown-close-dur` | `150ms` |
| `--dropdown-pre-scale` | `0.97` |
| `--dropdown-closing-scale` | `0.99` |
| `--dropdown-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |

### Güçlü yan

Anchored yüzeylerde modal’a göre daha hafif ve doğru seçimdir.

### Dikkat edilmesi gerekenler

`.is-closing` cleanup olmazsa sonraki açılış yanlış scale state’inden başlayabilir.

---

## 6.6 `06-modal.md` — Modal open / close

### Kullanım amacı

Sayfanın üstünde duran centered dialog veya overlay surface için kullanılır. Dropdown’dan farkı trigger’a anchor olmaması, merkezden scale etmesidir.

### HTML hook

```html
<div class="t-modal" role="dialog">…</div>
```

### State modeli

- `.is-open`: Modal açık.
- `.is-closing`: Kapanış animasyonu.
- Timeout sonrası `.is-closing` temizlenir.

### CSS mantığı

- `transform-origin: center`
- Kapalı başlangıç: `scale(var(--modal-scale))`, opacity 0.
- Açık: scale 1, opacity 1, pointer-events auto.
- Kapanış: `scale(var(--modal-scale-close))`, opacity 0.

### JS mantığı

Dropdown ile aynı close-cleanup pattern’i kullanır.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--modal-open-dur` | `250ms` |
| `--modal-close-dur` | `150ms` |
| `--modal-scale` | `0.96` |
| `--modal-scale-close` | `0.96` |
| `--modal-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |

### Güçlü yan

Merkezden açılan yüzeyler için net ve kontrollü bir geçiş sağlar.

### Dikkat edilmesi gerekenler

Modal erişilebilirliği için bu snippet dışında focus management, ESC close ve aria labeling gibi davranışlar ayrıca uygulanmalıdır.

---

## 6.7 `07-panel-reveal.md` — Panel reveal

### Kullanım amacı

Var olan bir container içinde panelin içeri kayarak görünmesi için kullanılır. Detail panel, expanding section, card içi reveal gibi durumlar için uygundur.

### HTML hook

```html
<div class="t-panel-slide" data-open="false">
  <!-- panel contents -->
</div>
```

### State modeli

- `data-open="false"`: Panel aşağıda, opacity 0, blur’lu.
- `data-open="true"`: Panel yerinde, opacity 1, blur 0.

### CSS mantığı

- TranslateY + opacity + blur birlikte çalışır.
- Açılış ve kapanış duration ayrı değişkenlerle kontrol edilir.
- Container’da `overflow: hidden` kullanılırsa kapalı state tam kırpılabilir.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--panel-open-dur` | `400ms` |
| `--panel-close-dur` | `350ms` |
| `--panel-translate-y` | `100px` |
| `--panel-blur` | `2px` |
| `--panel-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |

### Güçlü yan

Modal/dropdown kadar ağır olmayan, sayfa içinde akışa bağlı reveal hissi verir.

### Dikkat edilmesi gerekenler

Panelin nereden / ne kadar kayacağı `--panel-translate-y` ile tasarım bağlamına göre ayarlanmalıdır.

---

## 6.8 `08-page-side-by-side.md` — Page side-by-side

### Kullanım amacı

Yan yana duran iki screen / page arasında geçiş yapmak için kullanılır. List-detail veya wizard step geçişleri için uygundur.

### HTML hook

```html
<div class="t-page-slide" data-page="1">
  <section class="t-page" data-page-id="1">…</section>
  <section class="t-page" data-page-id="2">…</section>
</div>
```

### State modeli

- Container’daki `data-page` aktif sayfayı belirler.
- `data-page-id="1"` sayfa 1’i, `data-page-id="2"` sayfa 2’yi temsil eder.
- Page 1 sola, page 2 sağa doğru exit davranışı alır.

### CSS mantığı

- Container relative konumlanır.
- Page’ler absolute olarak üst üste yerleştirilir.
- Aktif olmayan page opacity 0, pointer-events none, blur ve translate ile çıkar.
- `--page-exit-enabled` 0 yapılırsa slide çıkışı devre dışı bırakılabilir.

### JS mantığı

```js
slider.setAttribute("data-page", String(n));
```

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--page-slide-dur` | `200ms` |
| `--page-fade-dur` | `200ms` |
| `--page-slide-distance` | `8px` |
| `--page-blur` | `3px` |
| `--page-stagger` | `0ms` |
| `--page-exit-enabled` | `1` |
| `--page-slide-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| `--page-fade-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |

### Güçlü yan

Çok küçük mesafeli slide + blur ile sayfa değişimini algılanabilir ama abartısız yapar.

### Dikkat edilmesi gerekenler

Page’ler absolute olduğu için container yüksekliği proje tarafından yönetilmelidir.

---

## 6.9 `09-icon-swap.md` — Icon swap

### Kullanım amacı

Aynı alanda iki ikon arasında geçiş yapmak için kullanılır. Hamburger-close, sun-moon, play-pause, expand-collapse gibi durumlar için uygundur.

### HTML hook

```html
<div class="t-icon-swap" data-state="a">
  <span class="t-icon" data-icon="a">…</span>
  <span class="t-icon" data-icon="b">…</span>
</div>
```

### State modeli

- `data-state="a"`: `data-icon="a"` görünür.
- `data-state="b"`: `data-icon="b"` görünür.

### CSS mantığı

- Container `inline-grid` kullanır.
- İki ikon aynı grid cell’e yerleşir.
- Görünen ikon opacity 1, blur 0, scale 1.
- Gizlenen ikon opacity 0, blur, küçük scale.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--icon-swap-dur` | `200ms` |
| `--icon-swap-blur` | `2px` |
| `--icon-swap-start-scale` | `0.25` |
| `--icon-swap-ease` | `ease-in-out` |

### Güçlü yan

Layout shift yaratmadan ikon state değişimi sağlar.

### Dikkat edilmesi gerekenler

İki ikon DOM’da birlikte bulunmalıdır; tek ikonun path’ini değiştirmek bu snippet’in modeline uymaz.

---

## 6.10 `10-success-check.md` — Success check

### Kullanım amacı

Başarılı tamamlanma anlarını vurgulamak için kullanılır. Örnekler: payment processed, file uploaded, message sent, form saved.

### HTML hook

```html
<span class="t-success-check" data-state="out" aria-hidden="true">
  <svg viewBox="0 0 48 48" fill="none">
    <!-- path -->
  </svg>
</span>
```

### State modeli

- `data-state="out"`: Cold-load, görünmez.
- `data-state="in"`: Appear animasyonu başlar.

### CSS mantığı

Aynı anda birkaç animasyon çalışır:

- Fade in
- Rotate upright
- Blur removal
- Y-bob settle
- SVG path stroke draw

### JS mantığı

Replay için:

1. `data-state="out"` yap.
2. Reflow zorla: `void check.offsetWidth`.
3. `data-state="in"` yap.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--check-opacity-dur` | `550ms` |
| `--check-rotate-dur` | `550ms` |
| `--check-rotate-from` | `80deg` |
| `--check-bob-dur` | `450ms` |
| `--check-y-amount` | `40px` |
| `--check-blur-dur` | `500ms` |
| `--check-blur-from` | `10px` |
| `--check-path-dur` | `550ms` |
| `--check-path-delay` | `80ms` |
| `--check-ease-out` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| `--check-ease-opacity` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| `--check-ease-rotate` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| `--check-ease-bob` | `cubic-bezier(0.34, 1.35, 0.64, 1)` |
| `--check-ease-path` | `cubic-bezier(0.22, 1, 0.36, 1)` |

### Özel not: stroke-dasharray

CSS’te `stroke-dasharray: 20` placeholder olarak gelir. Gerçek SVG path uzunluğu `path.getTotalLength()` ile ölçülmeli ve CSS’e yazılmalıdır. Aksi halde çizgi erken görünebilir veya fazla çizim hissi verebilir.

### Güçlü yan

Tek bir check ikonuna premium ve tamamlanmış hissi veren çok katmanlı hareket ekler.

### Dikkat edilmesi gerekenler

Bu snippet sadece appear transition sağlar. Hide / exit davranışı ayrıca tasarlanmalıdır.

---

## 6.11 `11-avatar-group-hover.md` — Avatar group hover

### Kullanım amacı

Horizontal avatar, chip, badge, segmented button veya tag pill grubunda hover edilen item’ın yükselmesi ve komşularının mesafeye göre daha az yükselmesi için kullanılır.

### HTML hook

```html
<div class="t-avatar-group">
  <div class="t-avatar">...</div>
  <div class="t-avatar">...</div>
</div>
```

### State modeli

Class state yerine JS inline custom property yazar:

- `--shift`
- `--scale-active`
- `transitionTimingFunction`

### CSS mantığı

- Her `.t-avatar`, `translateY(var(--shift)) scale(var(--scale-active))` ile hareket eder.
- Hover-in ve mouseleave için farklı easing gerekir.
- Reduced motion’da transform ve transition kapatılır.

### JS mantığı

- Hover edilen index hesaplanır.
- Tüm item’lar gezilir.
- Aktif item tam lift + scale alır.
- Komşular `falloff^distance` formülüyle daha az lift alır.
- Mouseleave’de bouncy ease-out ile hepsi sıfırlanır.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--avatar-lift` | `-4px` |
| `--avatar-dur` | `320ms` |
| `--avatar-scale` | `1.05` |
| `--avatar-falloff` | `0.45` |
| `--avatar-ease-in` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| `--avatar-ease-out` | `cubic-bezier(0.34, 3.85, 0.64, 1)` |

### Güçlü yan

Grup etkileşimine fiziksel ve organik his katar. Özellikle chip/tag/avatar cluster’larında etkili olur.

### Dikkat edilmesi gerekenler

`transition-timing-function` inline olarak değişkenler yazılmadan önce set edilmelidir. CSS’te sabit kalırsa hover-in ve return aynı easing’i kullanır.

---

## 6.12 `12-error-state-shake.md` — Error state shake

### Kullanım amacı

Form validation hatalarında kullanılır. Input sağ-sol shake yapar, border error state’e geçer, hata mesajı görünür; belirli süre sonra otomatik neutral state’e dönebilir.

### HTML hook

```html
<div class="t-input-wrap">
  <div class="t-input">
    <input type="text">
  </div>
  <p class="t-error-msg">Please enter a valid email.</p>
</div>
```

### State modeli

- `.is-error` wrap üzerinde: mesaj görünürlüğü.
- `.is-error` input üzerinde: border state.
- `.is-shaking` input üzerinde: shake animasyonu.

Bu üç state’in ayrılması bilinçli bir tasarımdır. Shake replay edilirken error state flicker yapmaz.

### CSS mantığı

- Border-color transition input üzerinde tanımlanır.
- Error message opacity + visibility transition ile görünür / gizlenir.
- Shake keyframe çok segmentlidir.
- Her keyframe stop kendi `animation-timing-function` değerini taşır.

### JS mantığı

1. Wrap ve input’a `.is-error` eklenir.
2. Input’tan `.is-shaking` kaldırılır.
3. Reflow yapılır.
4. `.is-shaking` tekrar eklenir.
5. Shake süresi hesaplanır.
6. Auto-revert timer kurulur.
7. Kullanıcı input’a yazarsa timer iptal edilir ve error state temizlenir.

### Değişkenler

| Değişken | Varsayılan |
|---|---|
| `--shake-distance` | `6px` |
| `--shake-overshoot` | `4px` |
| `--shake-dur-a` | `80ms` |
| `--shake-dur-b` | `60ms` |
| `--shake-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| `--revert-hold` | `3000ms` |
| `--revert-dur` | `280ms` |

### Özel not: keyframe yüzdeleri

Keyframe stop yüzdeleri `--shake-dur-a` ve `--shake-dur-b` oranına göre elle hesaplanmıştır:

- 0%
- 28.57%
- 57.14%
- 78.57%
- 100%

Eğer A/B duration oranı değiştirilirse bu yüzdeler de yeniden hesaplanmalıdır.

### Güçlü yan

Hata feedback’i hem görsel hem temporal olarak net verir. Auto-revert ve typing cancel davranışları UX açısından olgunlaştırılmıştır.

### Dikkat edilmesi gerekenler

`.is-error` ve `.is-shaking` tek class’a birleştirilmemelidir. Bu, animasyon replay davranışını bozar.

---

## 7. Transition Seçim Karar Ağacı

Aşağıdaki karar ağacı, projede hangi transition’ın seçileceğini hızlı belirlemek için kullanılabilir:

1. UI öğesi boyut mu değiştiriyor?
   - Evet → `01-card-resize`
2. Sayı / metrik güncelleniyor mu?
   - Evet → `02-number-pop-in`
3. Trigger üzerinde küçük rozet / bildirim mi çıkıyor?
   - Evet → `03-notification-badge`
4. Aynı yerde text state mi değişiyor?
   - Evet → `04-text-states-swap`
5. Trigger’dan açılan anchored surface mi?
   - Evet → `05-menu-dropdown`
6. Sayfanın üstünde centered overlay / dialog mu?
   - Evet → `06-modal`
7. Var olan bir container içinde panel mi reveal oluyor?
   - Evet → `07-panel-reveal`
8. İki screen / step arasında mı geçiliyor?
   - Evet → `08-page-side-by-side`
9. Aynı slotta iki ikon mu değişiyor?
   - Evet → `09-icon-swap`
10. Başarı / tamamlandı anı mı gösteriliyor?
   - Evet → `10-success-check`
11. Horizontal stack hover davranışı mı?
   - Evet → `11-avatar-group-hover`
12. Form validation / yanlış değer feedback’i mi?
   - Evet → `12-error-state-shake`

---

## 8. Uygulama Standardı

Bir projeye transition eklerken önerilen standart süreç:

1. Önce universal `:root` değişken bloğunu kontrol et.
2. Yoksa global stylesheet’e bir kez ekle.
3. İlgili transition dosyasındaki CSS’i birebir ekle.
4. HTML hook’larını component markup’ına uygula.
5. Gerekli state attribute / class toggle’larını bağla.
6. JS gerekiyorsa duration değerlerini CSS değişkenlerinden oku.
7. `prefers-reduced-motion` bloğunu silme.
8. Selector adlarını değiştirme.
9. `transition: all` kullanma.
10. Replay gereken animasyonlarda reflow adımını koru.

---

## 9. Teknik Güçlü Yönler

- **Framework bağımsız:** React, Vue, vanilla HTML/CSS veya herhangi bir stack ile kullanılabilir.
- **Low dependency:** Harici animation library gerektirmez.
- **Semantic token yapısı:** Değerler okunabilir isimlerle yönetilir.
- **Accessibility uyumlu:** Reduced motion guard standart olarak gelir.
- **Agent-friendly dokümantasyon:** AI coding agent’ları için karar kuralları, output format ve common mistakes net verilmiştir.
- **Küçük difflere uygun:** Skill, sadece gerekli dosyaların değiştirilmesini ister.
- **Demo ile skill senkronizasyonu:** Build script dokümanları kaynak siteden üretir.

---

## 10. Potansiyel Riskler / Dikkat Noktaları

| Alan | Risk | Önlem |
|---|---|---|
| Layout animation | Width/height animasyonu büyük layout’larda pahalı olabilir | Card resize’ı ölçülü kullan |
| Reflow bağımlılığı | Replay animasyonları reflow olmadan çalışmayabilir | `void el.offsetWidth/Height` adımını koru |
| State cleanup | Dropdown/modal closing class temizlenmezse sonraki açılış bozulur | Timeout cleanup’ı silme |
| SVG path draw | Stroke length yanlışsa check animasyonu bozulur | `getTotalLength()` ile ölç |
| Error shake tuning | Duration oranı değişirse keyframe yüzdeleri uyumsuzlaşır | Yüzdeleri yeniden hesapla |
| Avatar hover easing | Timing inline set edilmezse dönüş hissi kaybolur | Timing’i variable write öncesinde set et |
| Accessibility | Reduced motion bloğu kaldırılırsa hareket tercihi yok sayılır | Guard bloğunu koru |

---

## 11. Tek Paket Katalog Özeti

| # | Transition | En uygun kullanım | Ana hook | State hook | JS |
|---:|---|---|---|---|---|
| 1 | Card resize | Kart/container boyut değişimi | `.t-resize` | Width/height state | Yok |
| 2 | Number pop-in | Sayı/metrik güncelleme | `.t-digit-group`, `.t-digit` | `.is-animating`, `data-stagger` | Var |
| 3 | Notification badge | Trigger üstü bildirim rozeti | `.t-badge`, `.t-badge-dot` | `data-open` | Yok |
| 4 | Text states swap | Aynı yerde metin değişimi | `.t-text-swap` | `.is-exit`, `.is-enter-start` | Var |
| 5 | Menu dropdown | Trigger’dan açılan menü | `.t-dropdown` | `.is-open`, `.is-closing`, `data-origin` | Var |
| 6 | Modal | Centered dialog | `.t-modal` | `.is-open`, `.is-closing` | Var |
| 7 | Panel reveal | Container içi panel | `.t-panel-slide` | `data-open` | Yok |
| 8 | Page side-by-side | İki ekran/step geçişi | `.t-page-slide`, `.t-page` | `data-page`, `data-page-id` | Basit |
| 9 | Icon swap | Aynı slotta ikon değişimi | `.t-icon-swap`, `.t-icon` | `data-state`, `data-icon` | Yok |
| 10 | Success check | Başarı/tamamlandı anı | `.t-success-check` | `data-state` | Var |
| 11 | Avatar group hover | Horizontal stack hover | `.t-avatar-group`, `.t-avatar` | Inline CSS vars | Var |
| 12 | Error state shake | Form error feedback | `.t-input-wrap`, `.t-input`, `.t-error-msg` | `.is-error`, `.is-shaking` | Var |

---

## 12. Kaynak Markdown Dosya Listesi

Aşağıdaki Markdown dosyaları bu pakette analiz edilmiştir:

```text
README.md
skills/transitions-dev/SKILL.md
skills/transitions-dev/01-card-resize.md
skills/transitions-dev/02-number-pop-in.md
skills/transitions-dev/03-notification-badge.md
skills/transitions-dev/04-text-states-swap.md
skills/transitions-dev/05-menu-dropdown.md
skills/transitions-dev/06-modal.md
skills/transitions-dev/07-panel-reveal.md
skills/transitions-dev/08-page-side-by-side.md
skills/transitions-dev/09-icon-swap.md
skills/transitions-dev/10-success-check.md
skills/transitions-dev/11-avatar-group-hover.md
skills/transitions-dev/12-error-state-shake.md
```

---

## 13. Kısa Sonuç

Bu Markdown seti, sadece transition snippet koleksiyonu değil; aynı zamanda AI coding agent’larının doğru transition’ı seçip projeye minimum ve güvenli diff ile uygulaması için tasarlanmış bir karar sistemi sunar.

En değerli parçalar:

- `SKILL.md` içindeki decision rules.
- Her transition dosyasındaki HTML hook + CSS + JS orchestration ayrımı.
- Universal semantic `:root` değişkenleri.
- `prefers-reduced-motion` standardının her snippet’te korunması.
- Common mistakes bölümüyle gelen uygulama kalite kontrolü.

