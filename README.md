# vibe-SaaS-engine

AI coding araclarini ortalama sonuc vermekten cikarip daha tutarli, daha premium ve daha savunmali SaaS arayuzleri uretmeye zorlayan Markdown tabanli bir sistem kutuphanesi.

Bu repo bir kod starter'i degil. Tasarim dili, UX karar sistemi, mimari sinirlar, analiz promptlari ve motion rehberleri iceren bir referans motorudur. Yeni proje uretirken de, legacy bir projeyi toparlarken de ayni kalite standardini korumak icin tasarlandi.

## Neden Bu Repo?

- AI'in jenerik UI kaliplarina dusmesini engeller
- Tasarim, motion, UX ve kod mimarisini tek yerde toplar
- Kod yazmadan once analiz yaptirmayi zorlar
- Refactor islerinde parca parca ve kontrollu ilerleme saglar
- Tekil efektleri baska projelere tasimak icin yeniden kullanilabilir rehberler sunar

## Son Eklenenler

Son eklenen dosyalar sadece isim olarak degil, belirli ihtiyaclari cozmek icin eklendi:

- `SCROLL-BLURRY-MOTION.md`: Tek dosyali HTML projelerde React veya Tailwind eklemeden, sadece CSS + vanilla JavaScript ile reveal, blur, stagger ve premium scroll girisleri kurdurur. Ozellikle mevcut `index.html` tabanli projelere sonradan animasyon eklemek icin var.
- `NEXSAS-MOTION-EFFECTS.md`: Nexsas template'indeki blur, fade, move-up, text reveal ve katmanli hero acilis hissini baska projelere tasimak icin parcali bir reuse rehberidir. Sadece "animasyon ekle" demez; hangi etki hangi katmanda kuruluyor onu ayristirir.
- `TRANSITION-DEV.md`: transitions.dev reposunu analiz ederek reusable transition mantigini, state toggle yapisini, semantic CSS variable yaklasimini ve hangi transition'da JS gerekip gerekmedigini ozetler. Kopyalanabilir effect secimi icin katalog gibi kullanilir.
- `DURALUX-DESIGN.md`: Genel premium vibe-coding esteginden farkli olarak, light-mode ve veri-yogun admin dashboard dili icin detayli bir referans sunar. Landing page degil, kurumsal panel isteyen durumlar icin eklendi.
- `DURALUX-PROMPT.md`: `DURALUX-DESIGN.md` referansini dogrudan uretim promptuna cevirir. Ozellikle SPA davranisi, sidebar iskeleti, admin layout ve mobil drawer kurallarini AI'a net ve sert sekilde dayatmak icin kullanilir.

## Hemen Basla

### Yeni proje uretmek icin

1. Cekirdek dosyalari sec: `DESIGN.md`, `ANIMATIONS-INTERACTIONS.md`, `UX-CRITIQUE.md`, `CODE-ARCHITECTURE.md`, `EDGE-CASES.md`, `MOBILE-RESPONSIVE.md`
2. `MASTER-PROMPT.md` dosyasini proje amacina gore doldur
3. Prompt icinde ilgili dosyalari `@DOSYA-ADI` ile referans ver
4. AI'in tum sistemi tek seferde degil, moduler sekilde kurmasini iste

### Mevcut projeyi analiz edip revize etmek icin

1. `ANALIZ-PROMPT.md` ile once rapor cikar
2. Kodu hemen yazdirmadan, rapordaki yol haritasini kontrol et
3. Onaydan sonra `REVIZE-MASTER-PROMPT.md` ile asamali refactor baslat

### Sadece belirli bir motion veya UI hissi eklemek icin

1. Genel sistem icin gerekiyorsa `ANIMATIONS-INTERACTIONS.md` dosyasini referans al
2. Sonra ihtiyaca gore tekil rehberi ekle: `SCROLL-BLURRY-MOTION.md`, `NEXSAS-MOTION-EFFECTS.md`, `TRANSITION-DEV.md`, `DURALUX-DESIGN.md`, `DURALUX-PROMPT.md`

## Repo Yapisi

### Cekirdek sistem

| Dosya | Ne yapar? | Ne zaman kullanilir? |
|---|---|---|
| `GUIDE.md` | Tum sistemin genel mantigini ve birlikte kullanim seklini anlatir | Repodaki yapinin buyuk resmini hizlica anlamak istediginde |
| `DESIGN.md` | Premium vibe-coding tasarim dili, tipografi, renk ve layout kurallari verir | Yeni bir SaaS arayuzu uretirken veya tasarim kalitesini yukari cekmek isterken |
| `ANIMATIONS-INTERACTIONS.md` | Motion dili, micro-interaction, glow ve premium hareket kurallari tanimlar | UI'a daha canli ve daha premium hareket davranisi eklemek istediginde |
| `UX-CRITIQUE.md` | Buton enflasyonunu azaltan ve gesture-first yaklasimi zorlayan UX kurallari verir | Fazla butonlu, karmasik veya yorucu akislari sadelestirmek istediginde |
| `CODE-ARCHITECTURE.md` | SSOT, modulerlik, event delegation ve temiz kod sinirlari tanimlar | Kodun spagettiye donmesini engellemek veya mevcut yapini toparlamak istediginde |
| `EDGE-CASES.md` | Runtime guvenligi, storage korumasi ve XSS gibi savunmalari toplar | Hata, veri kaybi, limit asimi ve savunma katmanlarini dusunmek gerektiginde |
| `MOBILE-RESPONSIVE.md` | Mobil ergonomi, bottom-sheet, touch hedefleri ve jest odakli responsive kurallar verir | Masaustu iyi ama mobil zayif kalan projeleri duzeltirken veya mobile-first kurgularken |

### Prompt katmani

| Dosya | Ne yapar? | Ne zaman kullanilir? |
|---|---|---|
| `MASTER-PROMPT.md` | Sifirdan proje uretmek icin ana prompt iskeletini verir | Yeni uygulama, landing page veya SaaS paneli baslatirken |
| `REVIZE-MASTER-PROMPT.md` | Mevcut projeyi ameliyat planiyla yeniden kurdurur | Legacy, daginik veya hizla buyumus bir projeyi toparlarken |
| `ANALIZ-PROMPT.md` | Kod yazmadan once masaustu ve mobil bilissel simulasyon raporu ister | Once sorunlari gormek, sonra kod uretmek istediginde |

### Motion ve efekt rehberleri

| Dosya | Ne yapar? | Ne zaman kullanilir? |
|---|---|---|
| `SCROLL-BLURRY-MOTION.md` | Tek dosyalik HTML projelerine IntersectionObserver tabanli blurry reveal, stagger ve soft motion sistemi kurdurur | React olmayan, tek `index.html` ile calisan projelerde mevcut yapiyi bozmadan reveal efekti eklerken |
| `NEXSAS-MOTION-EFFECTS.md` | Nexsas tarzi blur, fade, move, text reveal, progressive blur ve scroll drift hissini katmanlara ayirir | Belirli bir premium acilis hissini veya hero motion yapisini baska projeye tasimak istediginde |
| `TRANSITION-DEV.md` | transitions.dev yapisini analiz eder; selector namespace, semantic token ve JS orchestration mantigini ozetler | Katalog mantigiyla component transition secmek, state toggle tasarlamak veya reusable gecis sistemi kurmak istediginde |

### Ozel referanslar

| Dosya | Ne yapar? | Ne zaman kullanilir? |
|---|---|---|
| `DURALUX-DESIGN.md` | Light-mode, veri yogun admin dashboard dili icin layout, renk, tipografi, sidebar, header ve bilesen referansi sunar | Premium landing yerine kurumsal admin panel tasarlamak istediginde |
| `DURALUX-PROMPT.md` | Duralux referansina gore dashboard uretmek icin hazir prompt verir; SPA navigasyon ve mobil drawer davranisini da zorlar | Duralux tarzini dogrudan uygulamaya donusturmek ve AI'a net admin panel siniri koymak istediginde |

### Workspace notu

| Dosya | Ne yapar? | Ne zaman kullanilir? |
|---|---|---|
| `AGENTS.md` | Yerel calisma tercihlerini tutar | Bu workspace'teki varsayilan yol ve calisma baglamini anlamak istediginde |

## Onerilen Is Akislari

### Akis 1: Sifirdan yeni bir SaaS

1. `DESIGN.md` ile temel gorunumu sabitle
2. `ANIMATIONS-INTERACTIONS.md` ve `UX-CRITIQUE.md` ile davranis kalitesini belirle
3. `CODE-ARCHITECTURE.md` ve `EDGE-CASES.md` ile teknik siniri kur
4. `MOBILE-RESPONSIVE.md` ile mobil davranisi erkenden tanimla
5. `MASTER-PROMPT.md` ile uretimi baslat

### Akis 2: Once analiz, sonra refactor

1. `ANALIZ-PROMPT.md` ile rapor al
2. Hatalari ve yol haritasini incele
3. `REVIZE-MASTER-PROMPT.md` ile milestone bazli duzeltmeye gec

### Akis 3: Sadece bir etki veya referans sistemi ekleme

1. Genel hareket dili gerekiyorsa `ANIMATIONS-INTERACTIONS.md` ekle
2. Sonra dar ihtiyaca gore rehber sec: `SCROLL-BLURRY-MOTION.md` veya `NEXSAS-MOTION-EFFECTS.md` veya `TRANSITION-DEV.md`
3. Admin panel gerekiyorsa `DURALUX-DESIGN.md` ve `DURALUX-PROMPT.md` kullan

## Hangi Dosyayla Baslamaliyim?

- Yeni bir SaaS urunu icin: `MASTER-PROMPT.md`
- Once denetim istiyorsan: `ANALIZ-PROMPT.md`
- Legacy refactor icin: `REVIZE-MASTER-PROMPT.md`
- Tek HTML reveal animasyonu icin: `SCROLL-BLURRY-MOTION.md`
- Admin dashboard icin: `DURALUX-DESIGN.md`
- Tum sistemi anlamak icin: `GUIDE.md`

## Notlar

- Bu repo birden fazla estetik mod tasir. `DESIGN.md` daha yaratci ve premium vibe-coding tarafini temsil ederken `DURALUX-DESIGN.md` daha kurumsal ve veri-yogun admin panel senaryosuna odaklanir.
- `SCROLL-BLURRY-MOTION.md`, tek dosyali HTML projelerde hizli sonuc veren en pratik motion rehberlerinden biridir.
- Dosya adlari tek tip buyuk harf + kebab-case standardina cekildigi icin prompt referanslarini takip etmek daha kolaydir.

## Ozet

Bu kutuphane, AI ile arayuz gelistirirken ortaya cikan ortalama ve daginik sonuc kalibini kirmak icin hazirlandi. Tasarim dili, motion kalitesi, UX ergonomisi, kod mimarisi ve savunma katmanlari ayni yerde toplandigi icin hem yeni proje kurulumunda hem de legacy refactor islerinde guclu bir operasyon paketi gibi calisir.