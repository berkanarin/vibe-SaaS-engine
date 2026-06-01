# Design System: Duralux Admin — Light Mode Reference
**Kaynak Tema:** Duralux CRM Admin (kod adı: `nxl` / Nexel)
**Kapsam:** Yalnızca Gündüz (Light) Modu — Dark mod bu referansın dışındadır.
**Amaç:** Bu dosya, Duralux admin panelinin layout, renk, tipografi, yerleşim, bileşen ve animasyon
sistemini birebir yeniden üretmek için bir yapım spesifikasyonudur. Tüm değerler temanın SCSS
kaynak dosyalarından çıkarılmıştır.

---

## Configuration — Tema Karakteri
Bu kadranlar, yapay zeka kodlama araçlarının (Claude, Cursor) bu temayı yeniden üretirken
koruması gereken niteliği tanımlar. Duralux jenerik bir SaaS landing page değil; **veri yoğun,
kurumsal bir yönetim panelidir**.

| Dial | Level | Açıklama |
|------|-------|----------|
| **Creativity** | `3` | Sakin, kurumsal, tahmin edilebilir. Sanatsal cesaret yok; tutarlılık esastır. |
| **Density** | `7` | Veri yoğun kokpit düzeni. Çok sayıda kart, tablo, KPI ve grafik aynı ekranda. |
| **Variance** | `3` | Simetrik, hizalı Bootstrap grid. Asimetri yok; öngörülebilirlik öncelikli. |
| **Motion Intent** | `4` | İşlevsel mikro animasyonlar (collapse, slide, hover). Sinematik efekt yok. |

> Bu temada DESIGN.md'deki "premium vibe-coding" estetiği geçerli DEĞİLDİR. Duralux'un kimliği
> netlik, yoğunluk ve kurumsal güvenilirliktir. Aşağıdaki tüm kurallar bu çerçevede uygulanır.

---

## 1. Visual Theme & Atmosphere
Duralux, bir CRM/yönetim panelinin temiz, ferah ama bilgi yoğun atmosferini referans alır.
Arka plan çok hafif soğuk-gri bir tuvaldir (`#f0f2f8`); içerik bu tuval üzerinde **beyaz kartlar**
hâlinde, yumuşak ve düşük opaklıklı gölgelerle yüzer. Derinlik ucuz gölgelerle değil, beyaz yüzey
ile gri tuval arasındaki kontrastla kurulur.

Arayüz üç sabit bölgeye ayrılır: solda gizlenebilir **sidebar**, üstte sabit **header**, ortada
kaydırılabilir **içerik alanı**. Sidebar ve header light modda **beyazdır** (dark variant `#0f172a`
kullanılmaz). Tek bir mavi aksan rengi (`#3454d1`) tüm birincil aksiyonları, aktif menüleri ve
linkleri taşır. Renk, vurgu için değil işlev için kullanılır.

---

## 2. Color Palette & Roles (Light Mode)

### Marka & Durum Renkleri
- **Primary / Accent** `#3454d1` — Tek aksan rengi. Birincil butonlar, linkler, aktif menü, focus halkası, progress bar.
- **Secondary** `#727981` — İkincil butonlar, nötr aksiyonlar.
- **Success** `#25b865` — Pozitif durum, onay, "aktif" rozetleri.
- **Danger** `#d13b4c` — Hata, silme, negatif durum.
- **Warning** `#e49e3d` — Uyarı, beklemede durumu.
- **Info** `#02a0e4` — Bilgilendirme.
- **Teal** `#41b2c4` — Etiket/tag varyantı.
- **Indigo** `#6610f2` — Ek kategori rengi.
- **Cyan** `#3dc7be` · **Orange** `#fd7e14` · **Pink** `#e83e8c` · **Purple** `#6f42c1` — Tamamlayıcı/grafik renkleri.

### Yüzey & Zemin
- **Body / Canvas** `#f0f2f8` — Ana sayfa arka planı. Hafif soğuk-gri.
- **Surface / Card** `#ffffff` — Tüm kart, panel, dropdown ve modal iç dolgusu.
- **Brand Light** `#eaebef` — Menü ve dropdown item hover/aktif zemini.
- **Soft Backgrounds** — Durum renklerinin `rgba(renk, 0.075)` versiyonu. `bg-soft-primary`,
  `bg-soft-success`, `bg-soft-danger`, `bg-soft-warning`, `bg-soft-info`, `bg-soft-teal`,
  `bg-soft-indigo` sınıflarıyla; rozet ve yumuşak vurgu alanlarında kullanılır.

### Metin Renkleri
- **Heading / Strong** `#283c50` — Başlıklar, kart başlıkları, KPI rakamları, güçlü metin. (Asla saf siyah değil.)
- **Body** `#6b7885` — Gövde metni, açıklamalar.
- **Muted** `#7587a7` — Üçüncül metin, zaman damgaları, pasif durumlar, placeholder yardımcı metni.

### Kenarlık & Gri Skala
- **Border (standart)** `#e5e7eb` — Kart, tablo, input, ayraç çizgileri (1px).
- **Border-2** `#dcdee4` — Icon-button ve ikincil çerçeveler.
- **Gri Skala:** `gray-100 #eff0f6` · `gray-200 #e9ecef` · `gray-300 #e5e7eb` · `gray-400 #ced4da`
  · `gray-500 #91a1b6` · `gray-600 #64748b` · `gray-700 #495057` · `gray-800 #343a40` · `gray-900 #212529`.

### Yasaklı Renkler
- **Dark mod paleti** (`#0f172a`, `#121a2d`, `#1c2438` vb.) bu referansta kullanılmaz.
- Saf siyah `#000000` metin/zemin olarak kullanılmaz; yerine `#283c50` kullanılır.
- Mor/violet neon gradyanlar yasaktır. Tek aksan kuralı: mavi `#3454d1` dışında ikinci bir marka aksanı eklenmez.

---

## 3. Typography Rules

- **Font Ailesi:** `Inter`, sans-serif (varsayılan). Tüm arayüz tek aile ile kurulur.
- **Taban Gövde Boyutu:** `0.84rem` (≈ 13.4px), satır yüksekliği `1.6`.
- **Başlık Boyutları:** `h1 36px` · `h2 28px` · `h3 24px` · `h4 20px` · `h5 16px` · `h6 15px`.
  Kart başlıkları genelde `h5` (16px, ağırlık 700).
- **Font Boyut Skalası (utility):** `fs-10 … fs-30` — 10,11,12,13,14,15,16,17,18,19,20,22,24,26,28,30px.
  Meta/etiket metinleri `fs-11`–`fs-12`, gövde `fs-13`–`fs-14`.
- **Ağırlık Skalası:** `100 → 900`. Pratik kullanım: gövde `400`, vurgu/etiket `500`,
  yarı-kalın `600` (form label, dropdown item, rozet), başlık/kalın `700`, KPI rakamı `700`.
- **Harf Aralığı (letter-spacing):** Gövde `0`. Butonlarda `uppercase` + `0.5px`.
  Skala: `xs 0.15px · sm 0.25px · md 0.5px · lg 0.75px · xl 1px · xxl 1.5px`.
- **Hiyerarşi** boyutla değil, **ağırlık + renk** ile kurulur: başlık `#283c50 / 700`,
  gövde `#6b7885 / 400`, meta `#7587a7 / 400`.

### Yasaklı Yazı Tipleri
- Tarayıcı varsayılan serif fontları (`Times New Roman`, `Georgia`) kullanılmaz.
- Tema 20+ alternatif font destekler (Poppins, Roboto, Lato vb.) ama bu referansın varsayılanı
  ve önerisi `Inter`'dir; tutarlılık için tek aile korunur.

---

## 4. Spacing, Radius & Shadow

### Spacing
- **Kart iç dolgu (padding):** `25px` (X ve Y) — temel ölçü birimidir.
- **Kart alt boşluğu:** `24px` (`mb-4`).
- **Grid boşlukları (gutter):** Bootstrap `g-3` / `g-4` (16px / 24px).
- **Buton dolgusu:** standart `12px 16px`; `sm 7px 15px`; `lg 15px 20px`.
- **Form kontrol dolgusu:** `12px 15px`.

### Border Radius
- Skala: `xs 3px · sm 5px · md 10px · lg 15px · xl 20px · xxl 25px · pill 30px · circle 50px`.
- **Bileşen eşlemesi:** Kart & Modal `10px` · Form kontrol `5px` · Buton & Badge & Tooltip `3px`
  · Dropdown menü `10px` · Avatar (yuvarlak) `50%`.

### Box Shadow
- **Kart gölgesi (varsayılan):** `0 1px 3px 0 rgb(0 0 0 / .1), 0 1px 2px -1px rgb(0 0 0 / .1)` — çok hafif, iki katmanlı.
- **Dropdown gölgesi:** `0 10px 24px 0 rgba(62, 57, 107, 0.18)`.
- **Gölge skalası (utility):** `sm 0 1px 5px` · `md 0 5px 15px` · `lg 0 10px 25px` · `xl 0 15px 35px`
  · `xxl 0 20px 45px` — hepsi `rgba(40,60,80,0.15)` rengiyle.
- Gölgeler her zaman yumuşak ve düşük opaklıktadır; sert/koyu drop-shadow kullanılmaz.

---

## 5. Layout Skeleton (Yerleşim İskeleti)

Sayfa üç sabit bölgeye ayrılır. Üst düzey HTML iskeleti:

```html
<body>
  <nav class="nxl-navigation"> ... </nav>      <!-- Sol Sidebar -->
  <header class="nxl-header"> ... </header>     <!-- Üst Header -->
  <main class="nxl-container">
    <div class="nxl-content">
      <div class="page-header"> ... </div>      <!-- Sayfa başlığı + breadcrumb -->
      <div class="main-content"> ... </div>     <!-- Asıl içerik (grid) -->
    </div>
  </main>
  <footer class="footer"> ... </footer>
  <div class="theme-customizer"> ... </div>     <!-- Sağdan kayan ayar paneli -->
</body>
```

### Sabit Ölçüler
- **Sidebar genişliği:** `280px` (açık) / `100px` (daraltılmış / mini).
- **Header yüksekliği:** `80px`.
- **İçerik alanı:** Sidebar genişliği kadar sol margin alır; sidebar daraldığında bu margin
  `100px`'e geçer ve içerik `0.3s ease` ile yeniden yerleşir.

### Page Header Bloğu
Her sayfanın üstünde başlık + breadcrumb (sol) ve aksiyon butonları (sağ) bulunur:

```html
<div class="page-header">
  <div class="page-header-left d-flex align-items-center">
    <div class="page-header-title"><h5 class="m-b-10">Dashboard</h5></div>
    <ul class="breadcrumb">
      <li class="breadcrumb-item"><a href="#">Home</a></li>
      <li class="breadcrumb-item">Dashboard</li>
    </ul>
  </div>
  <div class="page-header-right ms-auto">
    <div class="page-header-right-items">
      <!-- tarih aralığı, filtre, export, "Create" butonu -->
    </div>
  </div>
</div>
```

### Footer
```html
<footer class="footer">
  <p class="fs-11 text-muted fw-medium text-uppercase mb-0 copyright">Copyright © 2024</p>
  <div class="d-flex align-items-center gap-4">
    <a class="fs-11 fw-semibold text-uppercase" href="#">Help</a>
    <a class="fs-11 fw-semibold text-uppercase" href="#">Terms</a>
    <a class="fs-11 fw-semibold text-uppercase" href="#">Privacy</a>
  </div>
</footer>
```

---

## 6. Sidebar Specification (`nxl-navigation`)

Sidebar bu temanın imza bileşenidir: beyaz zeminli, ikonlu, gizlenebilir ve animasyonlu.

### HTML Yapısı
```html
<nav class="nxl-navigation">
  <div class="navbar-wrapper">
    <div class="m-header">
      <a href="#" class="b-brand">
        <img src="logo-full.png" class="logo logo-lg" />   <!-- açıkken -->
        <img src="logo-abbr.png" class="logo logo-sm" />   <!-- mini iken -->
      </a>
    </div>
    <div class="navbar-content">
      <ul class="nxl-navbar">
        <li class="nxl-item nxl-caption"><label>Navigation</label></li>
        <li class="nxl-item nxl-hasmenu">
          <a href="javascript:void(0);" class="nxl-link">
            <span class="nxl-micon"><i class="feather-airplay"></i></span>
            <span class="nxl-mtext">Dashboards</span>
            <span class="nxl-arrow"><i class="feather-chevron-right"></i></span>
          </a>
          <ul class="nxl-submenu">
            <li class="nxl-item"><a class="nxl-link" data-route="crm">CRM</a></li>
            <li class="nxl-item"><a class="nxl-link" data-route="analytics">Analytics</a></li>
          </ul>
        </li>
        <!-- ... -->
      </ul>
    </div>
  </div>
</nav>
```

### Yapı Kuralları
- **İkon sistemi:** Feather Icons (`feather-*`). Her birinci seviye menü maddesinin bir ikonu vardır.
- **Bölüm başlığı:** `nxl-caption` — küçük, muted, uppercase grup etiketi (örn. "Navigation").
- **Menü maddesi:** `nxl-item`. Alt menülü ise `nxl-hasmenu`.
- **Link iç yapısı:** `nxl-micon` (ikon) + `nxl-mtext` (metin) + `nxl-arrow` (sağ chevron).
- **Alt menü:** `nxl-submenu`. İç içe 3 seviyeye kadar; her seviyede sol girinti artar
  (`margin-left` 35px → 45px → 55px).
- **Örnek menü grupları:** Dashboards (CRM, Analytics), Reports, Applications, Proposal, Payment,
  Customers, Leads, Projects, Widgets, Settings, Authentication, Help Center.

### Collapse / Mini Davranışı
- Header'daki `#menu-mini-button` (`feather-align-left`) sidebar'ı daraltır → `<html>`'e
  `minimenu` sınıfı eklenir; durum `localStorage`'a yazılır.
- `#menu-expend-button` (`feather-arrow-right`) geri açar.
- `minimenu` durumunda: `logo-lg`, `nxl-mtext`, `nxl-arrow` ve sidebar kartı `display:none`;
  `logo-sm` görünür; ikonlar ortalanır; sidebar `100px`.
- **Hover-to-expand:** mini durumdayken sidebar üzerine gelince `navbar-content` `position:absolute`
  olur, geçici olarak `280px`'e açılır ve metin/ok geri görünür — fare çekilince tekrar daralır.
- **Responsive:** `≥1024px` altında `minimenu` zorlanır; `>1600px` üstünde otomatik açık.
- **Mobil (`<1024px`):** sidebar ekran dışına kayar; header'daki hamburger (`.nxl-head-mobile-toggler`)
  ile overlay olarak açılır.

### Aktif & Hover Stilleri
- Aktif/hover menü linki: metin `#283c50`, zemin `#eaebef` (brand-light), geçiş `0.3s ease`.
- Açık alt menünün `nxl-arrow` ikonu `90°` döner (`transform: rotate(90deg)`).

---

## 7. Header Specification (`nxl-header`)

Beyaz zeminli, `80px` yüksekliğinde, sola ve sağa hizalı iki bölümlü üst çubuk.

```html
<header class="nxl-header">
  <div class="header-wrapper">
    <div class="header-left d-flex align-items-center gap-4">
      <a class="nxl-head-mobile-toggler" id="mobile-collapse"> <!-- hamburger --> </a>
      <div class="nxl-navigation-toggle">
        <a id="menu-mini-button"><i class="feather-align-left"></i></a>
        <a id="menu-expend-button" style="display:none"><i class="feather-arrow-right"></i></a>
      </div>
      <!-- Mega Menu tetikleyici -->
    </div>
    <div class="header-right ms-auto">
      <!-- Sağ kontroller -->
    </div>
  </div>
</header>
```

### Sol Bölüm (`header-left`)
- Mobil hamburger toggler.
- Sidebar daralt/aç düğmeleri (`menu-mini-button` / `menu-expend-button`).
- Mega Menu açıcı (`btn btn-light-brand`) — sekmeli geniş dropdown.

### Sağ Bölüm (`header-right`)
Hepsi `dropdown nxl-h-item` deseninde, `nxl-head-link` tetikleyicili:
- **Arama** — `feather-search`; açılır panelde input + son sonuçlar.
- **Dil** — bayrak görseli; dil listesi dropdown'u.
- **Tam ekran** — `feather-maximize` / `feather-minimize` toggle.
- **Tema toggle** — `feather-moon` / `feather-sun`. *(Bu referansta light mod sabittir; bu kontrol
  görsel olarak bırakılabilir ama dark moda geçiş uygulanmaz — veya tamamen kaldırılır.)*
- **Timesheet** — `feather-clock` + yeşil sayaç rozeti (`badge bg-success nxl-h-badge`).
- **Bildirimler** — `feather-bell` + kırmızı sayaç rozeti (`badge bg-danger nxl-h-badge`).
- **Profil** — avatar görseli; dropdown başlığında kullanıcı adı + e-posta + `PRO` rozeti,
  altında durum/abonelik alt menüleri ve çıkış linki.

> Masaüstünde (`>992px`) header dropdown'ları **hover** ile açılır; mobilde tıklama ile.

---

## 8. Component Catalog (Bileşen Kataloğu)

### KPI / Stat Kartı
Dashboard üstündeki metrik kartları. `col-xxl-3 col-md-6` ızgarasında 4'lü dizilir.
```html
<div class="card stretch stretch-full">
  <div class="card-body">
    <div class="d-flex align-items-start justify-content-between mb-4">
      <div class="d-flex gap-4 align-items-center">
        <div class="avatar-text avatar-lg bg-gray-200"><i class="feather-dollar-sign"></i></div>
        <div>
          <div class="fs-4 fw-bold text-dark"><span class="counter">45</span>/<span class="counter">76</span></div>
          <h3 class="fs-13 fw-semibold text-truncate-1-line">Invoices Awaiting Payment</h3>
        </div>
      </div>
      <a href="#"><i class="feather-more-vertical"></i></a>
    </div>
    <div class="pt-4">
      <div class="d-flex align-items-center justify-content-between">
        <a class="fs-12 fw-medium text-muted">Invoices Awaiting</a>
        <div class="text-end"><span class="fs-12 text-dark">$5,569</span> <span class="fs-11 text-muted">(56%)</span></div>
      </div>
      <div class="progress mt-2 ht-3"><div class="progress-bar bg-primary" style="width:56%"></div></div>
    </div>
  </div>
</div>
```

### Standart Kart
```html
<div class="card stretch stretch-full">
  <div class="card-header">
    <h5 class="card-title">Payment Record</h5>
    <div class="card-header-action">
      <div class="card-header-btn">
        <a class="avatar-text avatar-xs bg-danger" data-bs-toggle="remove"></a>
        <a class="avatar-text avatar-xs bg-warning" data-bs-toggle="refresh"></a>
        <a class="avatar-text avatar-xs bg-success" data-bs-toggle="expand"></a>
      </div>
      <div class="dropdown">
        <a class="avatar-text avatar-sm" data-bs-toggle="dropdown"><i class="feather-more-vertical"></i></a>
        <div class="dropdown-menu dropdown-menu-end"> ... </div>
      </div>
    </div>
  </div>
  <div class="card-body custom-card-action"> ... </div>
  <div class="card-footer"> ... </div>
</div>
```
- Kart: beyaz zemin, `10px` radius, `1px` şeffaf border, hafif kart gölgesi, `25px` iç dolgu.
- `card-header` ve `card-footer` alt/üst `1px #e5e7eb` çizgiyle ayrılır, zeminleri şeffaftır.
- `card-title`: `h5`, 16px, ağırlık 700, renk `#283c50`.

### Trend / Sparkline Widget Kartı
Üst bölümde ikon + başlık + alt-başlık (sol) ve büyük rakam (sağ); altında **ince ayraç
çizgisi**; en altta yumuşak sparkline (alan grafiği) + yan tarafta trend metni.
```html
<div class="card stretch stretch-full">
  <!-- Üst bölüm: header -->
  <div class="card-header d-flex align-items-center justify-content-between">
    <div class="d-flex align-items-center gap-3">
      <div class="avatar-text avatar-lg rounded-circle bg-gray-200"><i class="feather-star"></i></div>
      <div>
        <h5 class="card-title mb-0">Tasks Completed</h5>
        <span class="fs-12 fw-normal text-muted">22/35 completed</span>
      </div>
    </div>
    <div class="fs-2 fw-bold text-dark"><span class="counter">22</span>/<span class="counter">35</span></div>
  </div>
  <!-- Alt bölüm: gövde -->
  <div class="card-body d-flex align-items-center justify-content-between">
    <div class="flex-grow-1"><div id="tasks-sparkline-chart"></div></div>
    <div class="text-end ms-3">
      <div class="fs-13 fw-bold text-primary">28% more</div>
      <div class="fs-12 text-muted">from last week</div>
    </div>
  </div>
</div>
```
Kritik detaylar:
- **Ayraç çizgisi:** Üst (header) ile alt (body) bölümü ayıran `1px solid #e5e7eb` çizgi —
  `card-header`'ın `border-bottom`'ı. Bu çizgi yumuşak ve düşük kontrastlıdır; göze batmaz,
  yalnızca iki bilgi katmanını sakince ayırır. Kartın "soft öne çıkışı" bu detaydan gelir.
- **İkon:** Dairesel (`rounded-circle`), nötr `bg-gray-200` zeminli, çizgisel Feather ikon.
- **Hiyerarşi:** başlık `#283c50 / 700`, alt-başlık `#7587a7 / 400`, büyük rakam `fs-2 / 700`.
- **Sparkline:** ApexCharts `area` tipi; çizgi `#3454d1`, dolgu aynı rengin yumuşak gradyanı
  (üstte ~%25 → altta ~%0 opaklık), `stroke` `curve: smooth`, eksen/grid/nokta gizli.
- **Trend metni:** delta değeri aksan renginde (`text-primary`), açıklama muted; sağa hizalı.
- Pozitif/negatif trend için `text-success` / `text-danger` kullanılabilir.

### Butonlar
- **Solid:** `btn btn-primary` (mavi dolgu + beyaz metin). Aynı şekilde `btn-secondary/success/danger/warning/info`.
- **Light-brand:** `btn btn-light-brand` — açık zeminli, nötr; ikincil/araç-çubuğu aksiyonları.
- **Light-soft:** `btn-light-primary` vb. — durum renginin çok açık zemini + renkli metin.
- **Icon button:** `btn btn-icon` — sadece ikon; `11px` dolgu, `1px #dcdee4` border.
- **Boyutlar:** `btn-sm`, varsayılan, `btn-lg`. Tam genişlik `w-100` (mobilde önerilir).
- Metin `uppercase`, ağırlık `700`, `0.5px` letter-spacing, radius `3px`.
- Aktif (tıklama) durumunda hafif fiziksel basılma hissi (`translateY` / `scale`).

### Tablolar
```html
<div class="table-responsive">
  <table class="table table-hover">
    <thead><tr><th>Customer</th><th>Email</th><th>Status</th><th class="text-end">Actions</th></tr></thead>
    <tbody>
      <tr class="single-item">
        <td><a class="hstack gap-3"><div class="avatar-image avatar-md"><img src="..."/></div><span>Ada Lovelace</span></a></td>
        <td><a href="#">ada@example.com</a></td>
        <td><span class="badge bg-soft-success text-success">Active</span></td>
        <td><div class="hstack gap-2 justify-content-end">
          <a class="avatar-text avatar-md"><i class="feather feather-eye"></i></a>
          <div class="dropdown"><a class="avatar-text avatar-md" data-bs-toggle="dropdown"><i class="feather-more-horizontal"></i></a> ... </div>
        </div></td>
      </tr>
    </tbody>
  </table>
</div>
```
- `table table-hover`, satır ayraçları `1px #e5e7eb`, başlık satırı muted/uppercase küçük metin.
- Aksiyonlar satır sonunda `avatar-text` icon-buton + dropdown kombinasyonu.

### Badge / Rozet
- **Solid:** `badge bg-primary` (success/danger/warning/info/teal/indigo).
- **Soft:** `badge bg-soft-success text-success` — açık zemin + renkli metin (tercih edilen biçim).
- Font `fs-11`, ağırlık `600`, dolgu `5px 6px`, radius `3px`. İçinde ok ikonu (`feather-arrow-up`) ile trend gösterilebilir.

### Dropdown
- `dropdown-menu`: beyaz, `10px` radius, `1px #e5e7eb` border, `0 10px 24px rgba(62,57,107,0.18)` gölge, `15px 0` dolgu.
- `dropdown-item`: `13px / 600`, dolgu `10px 15px`, margin `3px 10px`, radius `5px`;
  hover/aktif zemini `#eaebef`. Ayraç: `dropdown-divider`.

### Avatar
- **Görsel:** `avatar-image avatar-{sm|md|lg|xl}` + `<img class="img-fluid">`.
- **Metin/İkon:** `avatar-text avatar-{boyut} bg-{renk}` — ikon veya baş harf taşır.
- Boyutlar: `xs 12 · sm 20 · md 30 · lg 50 · xl 65 · xxl 80` px. Yuvarlak için `rounded` / `%50`.

### Progress Bar
- `progress ht-3` (3px yükseklik) + `progress-bar bg-{renk}`; genişlik inline `style="width:%"`.

### Form Kontrolleri
- **Label** her zaman input'un üstünde: `12px`, ağırlık `600`, renk `#283c50`. Floating label kullanılmaz.
- **Input/Select:** `form-control` — dolgu `12px 15px`, border `1px #e5e7eb`, radius `5px`,
  metin `#283c50`, placeholder `#91a1b6`.
- **Focus:** kenarlık aksan rengine (`#3454d1`) döner.
- **Checkbox:** `18×18px`, işaretli zemin `#3454d1`. Gelişmiş seçim için Select2 (etiketli çoklu seçim).

### Grafik Konteyneri
- Grafikler **ApexCharts** ile, kart içinde `<div id="...-chart"></div>` konteynerine render edilir.
- Grafik renkleri yukarıdaki paletten (primary, success, warning vb.) seçilir.

---

## 9. Motion & Interaction

Hareket işlevseldir; her animasyon bir durum değişikliğini iletir. Sinematik efekt yoktur.

- **Standart geçiş:** `all 0.3s ease` — sidebar collapse, menü hover, içerik yeniden yerleşimi, dropdown.
- **Alt menü aç/kapa:** dikey slide, ~`200ms` (jQuery `slideDown/slideUp "fast"` eşdeğeri).
- **Ok dönüşü:** açık `nxl-hasmenu` içindeki `nxl-arrow` `90°` döner, `0.2s ease-in-out`.
- **Sidebar hover-expand:** mini durumda hover ile `280px`'e açılma, `0.3s ease`.
- **Header dropdown:** masaüstünde (`>992px`) hover ile anında açılır/kapanır.
- **Sayfa loader:** ilk yüklemede `loader-bg` ~`400ms` sonra `fadeOut` ile kaybolur.
- **Kart yenileme:** `data-bs-toggle="refresh"` kartın üzerine bir spinner bindirir.
- **Scroll:** sidebar ve uzun paneller `perfect-scrollbar` ile ince, özelleştirilmiş kaydırma çubuğu kullanır (yalnızca dikey).
- **Performans:** animasyonlar yalnızca `transform` ve `opacity` üzerinden; `width/height/top/left` animasyonundan kaçınılır.

---

## 10. Navigation Rule — SPA / Yenilemesiz Gezinme (KRİTİK)

Orijinal Duralux teması, sidebar alt menü linklerini düz `<a href="analytics.html">` olarak kurar;
her menü tıklaması **tam sayfa yenilemesi** tetikler. **Bu davranış bu referansta YASAKTIR.**

### Zorunlu Davranış
- Menü tıklaması sayfayı yeniden yüklememeli; yalnızca **içerik alanı** (`.nxl-content` içindeki
  `.page-header` + `.main-content`) yerinde güncellenmelidir (SPA-tarzı).
- Linkler `href="*.html"` yerine bir route tanımlayıcı taşır (örn. `data-route="analytics"`),
  JS ile yakalanır; `event.preventDefault()` uygulanır.
- Sidebar açık/kapalı durumu, scroll konumu ve genel layout tıklama sonrası **korunur** —
  yeniden render edilmez, "yanıp sönme" olmaz.
- Aktif menü vurgusu route eşlemesiyle güncellenir (URL string karşılaştırmasıyla değil).
- Tarayıcı geçmişi `history.pushState` ile yönetilir; ileri/geri tuşları `popstate` ile çalışır.
- Sayfa başlığı (`page-header > h5`) ve breadcrumb route'a göre güncellenir.
- İçerik geçişinde kısa, ince bir `opacity` fade (`0.3s ease`) uygulanabilir; sert "pat" geçiş olmaz.

### Refresh'in Kök Nedeni — TAŞINMAYACAK Altyapı
Orijinal Duralux'ta tam sayfa yenilemesi tek bir bug değil, üç parçalı bir mimari altyapının
sonucudur. Bu üç parça bu projeye **kopyalanmaz / taşınmaz**:

1. **Çok dosyalı sayfa mimarisi:** Duralux her ekranı ayrı bir `.html` dosyası olarak kurar
   (`index.html`, `analytics.html`, `customers.html` ...). Bu dosyalar olduğu gibi
   **kullanılmaz**. Proje tek bir kabuk (shell) HTML üzerine kurulur; ekranlar içerik
   parçası/şablon olarak yüklenir.
2. **`nxlNavigation.js`'in URL-eşleştirme mantığı:** Bu dosya aktif menüyü
   `window.location.href` string karşılaştırmasıyla bulur — bu yaklaşım çok dosyalı yapıyı
   varsayar. Bu JS **olduğu gibi dahil edilmez**; navigasyon ve aktif-menü mantığı sıfırdan,
   route tabanlı olarak yazılır.
3. **`href="*.html"` menü linkleri:** Sidebar/menü linkleri dosya yoluna işaret etmez;
   `data-route` taşır (bkz. yukarıdaki Zorunlu Davranış).

Sidebar'ın görsel/animasyon stilleri (collapse, hover-expand, `nxl-*` sınıfları) referans
alınır; ancak **gezinme altyapısı** Duralux'tan miras alınmaz, baştan SPA olarak kurulur.

### Yasak
- Duralux'un hazır `.html` sayfa dosyalarını veya `nxlNavigation.js`'i olduğu gibi kopyalamak.
- `<a href="sayfa.html">` ile menü navigasyonu ve tam sayfa reload.
- `location.reload()` veya `window.location = ...` ile menü gezinmesi.
- Aktif menüyü `window.location.href` string karşılaştırmasıyla tespit etmek.

---

## 11. Layout, Responsive & Mobil Davranış

### 11.1 Genel Yerleşim İlkeleri
- **Grid-first:** Tüm yapısal iskelet Bootstrap 5 grid (`row` / `col-*`) ile kurulur.
- **Kademeli sütunlar:** `col-xxl-*` (≥1400), `col-xl-*`, `col-lg-*`, `col-md-*`, `col-sm-*`.
- **Yatay taşma yasaktır:** Hiçbir kırılımda yatay scroll (horizontal overflow) oluşmaz — kritik hatadır.

### 11.2 Kırılım Noktaları (Breakpoints)
Tema şu eşikleri kullanır: **320 · 375 · 576 · 768 · 992 · 1024 · 1200 · 1400 · 1600 px**.
Düzeni belirleyen üç ana eşik:

| Aralık | Sidebar | Header | İçerik |
|--------|---------|--------|--------|
| **>1600px** | Tam açık (280px) | Tam | `margin-left: 280px` |
| **1024–1600px** | Otomatik mini (100px, sadece ikon) | Tam | `margin-left: 100px` |
| **<1024px (MOBİL)** | Ekran dışı off-canvas drawer | Hamburger modu | `margin-left: 0`, tam genişlik |

`1024px` bu temanın **mobil eşiğidir**: altında masaüstü mantığı (mini-mod, içerik margin'i) tamamen
devre dışı kalır, off-canvas davranışı devreye girer. Ekran yeniden boyutlandığında `1024px`
geçişlerinde mini-mod otomatik eklenir/kaldırılır.

### 11.3 Mobil Sidebar — Off-Canvas Drawer (`<1024px`)
Mobilde sidebar normal akıştan çıkar ve soldan kayan bir çekmeceye dönüşür.

- **Varsayılan konum:** `left: -280px` (ekranın solunda, görünmez).
- **Tetikleyici:** Header'daki hamburger — `#mobile-collapse` / `.nxl-head-mobile-toggler`
  (`<1024px` görünür, `≥1024px` `display:none`). Hamburger ikonu tıklayınca `is-active` sınıfıyla
  ok şekline morf olur (`hamburger--arrowturn`).
- **Açılma:** `.nxl-navigation`'a `mob-navigation-active` sınıfı eklenir → `left: 0`.
- **Animasyon:** `transition: all 0.15s ease-in-out` — masaüstü `0.3s`'ten daha hızlı, dokunma
  geri bildirimi için.
- **Overlay (scrim):** Sidebar açılınca JS bir `<div class="nxl-menu-overlay">` ekler — tüm
  ekranı kaplayan `rgba(0,0,0,0.2)` koyu yarı saydam katman (`position:fixed`, full viewport).
  Sidebar/wrapper bu katmanın üstündedir (`z-index: 5` vs overlay `z-index: 1`).
- **Kapanma:** Overlay'e dokununca sidebar kapanır, overlay kaldırılır, hamburger `is-active` sıfırlanır.
- Mobilde mini-mod, hover-to-expand ve sidebar alt kartı **kullanılmaz**; sidebar tam genişlikte (`280px`) açılır.

### 11.4 Mobil Header (`<1024px`)
- `nxl-navigation-toggle` (mini/aç düğmeleri) gizlenir; yerini hamburger alır.
- `#mobile-collapse` `position:absolute; left:15px` ile sol üste sabitlenir.
- Header `left: 0`, geçiş `0.15s ease-in-out`.
- **`<576px`:** `header-wrapper` iç dolgusu `0 20px`'e iner. Header dropdown'ları
  (`nxl-h-dropdown` — arama, dil, bildirim) `nxl-h-item` `position:static` olur ve dropdown
  **kenardan kenara tam genişlik** açılır (`left:0; right:0; width:fill-available`) — dar ekranda
  taşmayı önler.

### 11.5 Mobil Page Header — Aksiyon Çekmecesi (`<768px`)
Sayfa başlığındaki sağ aksiyon grubu (`page-header-right-items`) ayrı bir **sağdan kayan
çekmeceye** dönüşür:
- Sabit panel: `position:fixed; top:79px; right:0; width:280px; height:100%`, beyaz zemin,
  sol+üst `1px` border.
- Varsayılan: `transform: translateX(100%)`, `opacity:0`, `visibility:hidden`.
- `page-header-right-open` sınıfı → `translateX(0)`, görünür, `box-shadow: lg`, geçiş `0.3s ease`.
- İçinde tüm buton/dropdown'lar `width:100%` ve dikey (`flex-direction:column`) dizilir.
- Üstte `page-header-right-close-toggle` kapatma çubuğu (`65px` yükseklik, alt border).
- `page-header` iç dolgusu `<576px`'te `0 20px`'e iner.

### 11.6 Uygulama Sayfaları Mobil (`apps-*`, `<1200px`)
Takvim/Chat/E-posta gibi `without-header` uygulama sayfalarında sol `content-sidebar`:
- `position:absolute; transform:translateX(-100%); opacity:0; visibility:hidden; z-index:1025`.
- `app-sidebar-open` sınıfı → `translateX(0)`, görünür, `box-shadow: lg`, geçiş `0.3s ease`.
- Tetikleyiciler: `app-sidebar-open-trigger` (`feather-align-left`) ve `app-sidebar-close-trigger`
  (`feather-x`) — `≥1200px`'te `display:none`.
- Mega menü (`nxl-lavel-mega-menu`) `<992px`'te soldan kayan `300px` çekmece olur
  (`html.nxl-lavel-mega-menu-open` ile açılır).

### 11.7 Mobil İçerik & Bileşen Davranışı
- **`<576px`:** `main-content` dolgusu `20px 20px 5px`; `content-area` header/body dolgusu `20px`;
  kart alt boşluğu `24px → 20px`.
- **`<768px`:** Sabit genişlik utility'leri (`wd-300`–`wd-600`) `width:100%`'e döner.
- Çok sütunlu KPI/kart/bento ızgaraları tek sütuna iner (`col-12`).
- **Dokunma hedefleri:** tıklanabilir tüm alanlar min `44px`; birincil butonlar `w-100`.
- Masaüstüne özgü hover etkileşimleri (header hover-dropdown, sidebar hover-expand) mobilde
  devre dışıdır; her şey dokunma/tıklama ile çalışır.

### 11.8 Mobil Animasyon Özeti
| Eleman | Hareket | Süre / Easing |
|--------|---------|----------------|
| Mobil sidebar drawer | `translateX` soldan kayma | `0.15s ease-in-out` |
| Mobil header kayması | `left` / konum | `0.15s ease-in-out` |
| Hamburger ikon | ok şekline morf (`is-active`) | CSS keyframe |
| Page-header aksiyon çekmecesi | `translateX` sağdan kayma | `0.3s ease` |
| App content-sidebar | `translateX` soldan kayma | `0.3s ease` |
| Mega menü çekmecesi | `translateX` soldan kayma | `0.3s ease` |
| Overlay (scrim) | `opacity` fade | `0.3s ease` |

> Tüm kayan paneller `transform: translateX()` + `opacity` üzerinden çalışır (donanım hızlandırmalı).
> `left/right/width` animasyonu kullanılmaz. Her açılan panelin koyu bir scrim'i olur ve scrim'e
> dokunmak paneli kapatır.

---

## 12. Calendar / Takvim Modülü

Takvim, Duralux'un en güçlü uygulama ekranlarından biridir ve Outlook benzeri bir
gün/hafta/ay deneyimi sunar. Bu modül ayrı bir sayfa kalıbı (`apps-calendar`) olarak kurulur.

### Kütüphane
- **TUI Calendar (Toast UI Calendar)** — takvim motoru. Gün/hafta/ay görünümlerini, zaman
  ızgarasını, sürükle-bırak ve popup'ları native sağlar.
- CSS: `tui-calendar.min.css` + `tui-theme.min.css`. Ek: `tui-time-picker`, `tui-date-picker`
  (etkinlik tarih/saat seçimi için).
- Tema (`tui-theme`) Duralux paletine göre özelleştirilir: birincil aksiyon `#3454d1`,
  silme `#d13b4c`, kenarlık `#e5e7eb`.
- *Not:* TUI Calendar v1 mevcut değilse modern muadili **FullCalendar**'dır; aynı
  gün/hafta/ay yapısı ve zaman ızgarası onunla da kurulabilir. Görsel spesifikasyon aynıdır.

### Sayfa Kalıbı (`apps-calendar`)
Standart `page-header` KULLANILMAZ. Tam yükseklik, başlıksız bir yerleşim kurulur:
```html
<main class="nxl-container apps-container apps-calendar">
  <div class="nxl-content without-header nxl-full-content">
    <div class="main-content d-flex">
      <div class="content-sidebar content-sidebar-xl"> ... </div>   <!-- sol: filtre + etkinlik listesi -->
      <div class="content-area">                                    <!-- sağ: takvim -->
        <div class="content-area-header sticky-top"> ... </div>      <!-- araç çubuğu -->
        <div class="content-area-body p-0"><div id="tui-calendar-init"></div></div>
      </div>
    </div>
  </div>
</main>
```
- **Sol içerik-sidebar** (`content-sidebar content-sidebar-xl`): "New Event" butonu, takvim
  kategori filtreleri ve yaklaşan etkinlik kartları. Mobilde gizlenir, `app-sidebar-open-trigger`
  ile overlay olarak açılır; `app-sidebar-close-trigger` (feather-x) ile kapanır.
- **Sağ içerik-alanı** (`content-area`): yapışkan (`sticky-top`) araç çubuğu + takvim gövdesi.

### Görünümler (Views)
Görünüm, araç çubuğundaki açılır menüden (`calendar-dropdown-btn`) seçilir. Varsayılan: **Monthly**.

| Görünüm | TUI view | Tetikleyici (`data-action`) | İkon | Açıklama |
|---------|----------|------------------------------|------|----------|
| **Daily** | `day` | `toggle-daily` | `feather-list` | Tek gün; saat ızgarası — Outlook tarzı yukarıdan aşağıya zaman bloğu. |
| **Weekly** | `week` | `toggle-weekly` | `feather-umbrella` | 7 günlük zaman ızgarası; dikey saat eksenli. |
| **Weeks (2)** | `month` (`visibleWeeksCount:2`) | `toggle-weeks2` | `feather-sliders` | 2 haftalık kompakt ay görünümü. |
| **Weeks (3)** | `month` (`visibleWeeksCount:3`) | `toggle-weeks3` | `feather-framer` | 3 haftalık görünüm. |
| **Monthly** | `month` (`visibleWeeksCount:0`) | `toggle-monthly` | `feather-grid` | Tam aylık ızgara. |

#### Daily & Weekly (Zaman Izgarası — imza deneyim)
- Sol kenarda dikey **saat ekseni** (00:00 → 23:00); gün/günler yatayda sütunlar.
- **Zamanlı etkinlikler** (`category: "time"`) ızgara üzerinde, başlangıç–bitiş saatine göre
  yüksekliği olan **dikey renkli bloklar** olarak çizilir — etkinlik ne kadar uzunsa blok o kadar uzun.
- Blok içinde saat (`<strong>HH:mm</strong>`) + başlık; durum ikonları (kilit/tekrar/katılımcı/konum)
  template ile eklenir.
- **Tüm gün etkinlikleri** (`category: "allday"`) ızgaranın üstündeki ayrı `allday` satırında
  yatay şerit olarak gösterilir.
- Bloklar sürükle-bırak ile taşınabilir/yeniden boyutlandırılabilir.

#### Monthly
- Klasik 7 sütunlu ay ızgarası; her gün hücresinde etkinlikler renkli satır olarak listelenir.
- Hücreye sığmayan etkinlikler "+N more" ile toplanır (`clickMore` olayı).
- Hafta günü başlığı: `12px / 600`, Maven Pro fontu.

### Araç Çubuğu (`content-area-header`)
- **Görünüm seçici dropdown** — `calendar-dropdown-btn`; aktif görünümün adı (`calendarTypeName`)
  ve ikonu (`calendarTypeIcon`) dinamik güncellenir.
- **Today** — `move-today` butonu (`feather-clock`); bugüne döner.
- **Prev / Next** — `avatar-text avatar-md` icon-butonları (`feather-chevron-left/right`),
  `move-prev` / `move-next`; aktif görünüme göre gün/hafta/ay atlar.
- **Render Range** — `#renderRange`; o an görünen tarih veya tarih aralığını yazar
  (örn. `17.12.24` veya `11/12/2024 ~ 17/12/2024`).
- **Görünüm seçenekleri** (dropdown içi onay kutuları): `Show Weekends` (workweek),
  `Start Week on Monday` (haftanın başlangıç günü), `Narrower than weekdays` (hafta sonu dar sütun).

### Kategoriler / Takvimler (Calendar List)
8 renkli kategori; her biri bir `calendarId` taşır. Renk eşlemesi (`data-calendar-id`):

| ID | Ad | Renk |
|----|-----|------|
| 1 | Office | `#5485e4` (primary) |
| 2 | Family | `#25b865` (success) |
| 3 | Friend | `#d13b4c` (danger) |
| 4 | Travel | `#17a2b8` (teal) |
| 5 | Privete | `#e49e3d` (warning) |
| 6 | Holidays | `#5856d6` (indigo) |
| 7 | Company | `#3dc7be` (cyan) |
| 8 | Birthdays | `#475e77` (dark) |

- Etkinlik bloğu zemini: kategori renginin **%15 opaklıklı** hâli; metin tam renk.
- Kategori noktası (`calendar-dot`): `7×7px`, yuvarlatılmış.
- Sol sidebar'da her kategori bir **yuvarlak onay kutusuyla** (`tui-full-calendar-checkbox-round`)
  açılıp kapatılır; "View All Schedules" tüm kategorileri tek seferde toggle eder.

### Etkinlik Listesi Kartı (sol sidebar)
Yaklaşan etkinlikler `schedule-item` kartı olarak listelenir:
```html
<div class="p-4 border-top c-pointer single-item schedule-item">
  <div class="d-flex align-items-start">
    <div class="wd-50 ht-50 bg-soft-success text-success d-flex flex-column
                align-items-center justify-content-center rounded-2 schedule-date">
      <span class="fs-18 fw-bold d-block">17</span>
      <span class="fs-10 text-uppercase d-block">Dec</span>
    </div>
    <div class="ms-3 schedule-body">
      <h6 class="fw-bold text-truncate-1-line">Company Standup Meeting</h6>
      <span class="fs-11 text-muted">8:00am - 9:00am, Engineering Room</span>
      <p class="fs-12 text-muted my-3 text-truncate-2-line">Açıklama...</p>
      <div class="img-group"> <!-- katılımcı avatarları --> </div>
    </div>
  </div>
</div>
```
- Sol tarafta **tarih rozeti**: `50×50px`, kategori renginin soft zemini (`bg-soft-*`),
  üstte gün numarası (`fs-18/700`), altta ay kısaltması (`fs-10` uppercase).
- Sağda başlık (1 satır truncate), saat+konum (muted), açıklama (2 satır truncate), katılımcı avatar grubu.

### Popup'lar ve Etkinlik Oluşturma
- **Oluşturma popup'ı** (`useCreationPopup`) — boş zaman dilimine tıkla/sürükle ile etkinlik kurar.
- **Detay popup'ı** (`useDetailPopup`) — etkinliğe tıklayınca başlık, zaman, konum, kategori gösterir;
  `Edit` (mavi) ve `Delete` (kırmızı) butonları içerir.
- Popup buton stili: `12px 16px` dolgu, `fs-10`, uppercase, `0.5px` letter-spacing, `3px` radius,
  `0.3s ease` geçiş.
- **New Event** butonu (`btn-new-schedule`, `btn btn-primary w-100`) ayrı bir modal açar
  (başlık, konum, "tüm gün" onayı, tarih/saat seçici).
- Etkinlik durumları: `Busy` / `Free`. Etkinlik kategorileri: `allday` ve `time`.

### Responsive
- `<992px`: sol takvim sidebar'ı gizlenir; araç çubuğundaki `app-sidebar-open-trigger`
  (`feather-align-left`) ile overlay açılır.
- Mobilde gün/hafta görünümleri yatay kaydırma yerine tek/az sütuna indirgenir; `renderRange`
  ve `Today` etiketi `d-none d-sm-flex` ile küçük ekranlarda gizlenir.

---

## 13. Anti-Patterns (Yasaklı Uygulamalar)

- **Dark mod yok:** Bu referans yalnızca light moddur. Dark paleti (`#0f172a` vb.) uygulanmaz;
  header'daki tema-toggle ya işlevsiz bırakılır ya kaldırılır.
- **Full page reload yok:** Bkz. Bölüm 10 — menü gezinmesi SPA-tarzı olmak zorundadır.
- **İkinci aksan yok:** Mavi `#3454d1` dışında ikinci bir marka aksanı eklenmez (durum renkleri hariç).
- **Saf siyah yok:** Metin/zemin için `#000000` kullanılmaz.
- **Emoji yok:** Arayüzde, ikon yerine veya metinde emoji kullanılmaz; tüm ikonlar Feather Icons.
- **Sahte yuvarlak sayı yok:** `%99.99`, `1.000.000+` gibi uydurma değerler yerine organik
  veriler (`%46.59`, `14.820`) kullanılır.
- **Jenerik isim yok:** "John Doe", "Acme Corp" gibi yer tutucular yerine özgün isimler.
- **Jenerik framework varsayılanı yok:** Bootstrap'ın ham `border-radius`/renk varsayılanları
  doğrudan kullanılmaz; bu dosyadaki token'lara (kart `10px`, buton `3px`, aksan `#3454d1`) göre özelleştirilir.
- **Spagetti kod yok:** Layout `nxl-*` sınıf sözleşmesine sadık kalır; ölçüsüz inline stil yığını yapılmaz.
```
