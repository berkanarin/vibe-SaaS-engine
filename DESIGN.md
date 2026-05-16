# Design System: Premium Vibe-Coding Standard
**Skill Framework:** stitch-design-taste

---

## Configuration — Set Your Style
Aşağıdaki kadranlar, Cursor ve Claude gibi yapay zeka modellerinin üreteceği arayüzlerin sanatsal cesaretini, yoğunluğunu ve yerleşim asimetrisini kontrol etmektedir. 

| Dial | Level | Description |
|------|-------|-------------|
| **Creativity** | `8` | `1` = Ultra-minimal, İsviçre tarzı sessiz arayüz. `10` = Cesur tipografi deneyimleri, asimetrik yerleşimler, kelime arası inline görseller. |
| **Density** | `4` | `1` = Sanat galerisi ferahlığı, devasa boşluklar. `10` = Veri yoğun, kokpit düzeni. |
| **Variance** | `8` | `1` = Tahmin edilebilir simetrik gridler. `10` = Sanatsal kaos, birbirine benzemeyen asimetrik bölümler. |
| **Motion Intent**| `6` | `1` = Statik arayüz. `10` = Kod aşamasında her bileşende sinematik akışkan mikrosaniyeler. |

---

## 1. Visual Theme & Atmosphere
Tasarım sistemi, lüks bir mimarlık stüdyosunun temiz, ferah ve akıllı atmosferini referans almaktadır. Görsel derinlik, ucuz gölge efektleriyle değil, yüzey katmanlarının soft ton geçişleriyle kurulur. Simetrik sıkıcılıktan kaçınmak adına yüksek varyans (Level 8) ve ferah bir whitespace (Level 4) dengesi gözetilir. Her eleman ekranda bir fonksiyona hizmet etmek için bulunur; süsleme amaçlı hiçbir jenerik katman barındırılmaz.

## 2. Color Palette & Roles
- **Canvas White** (#F9FAFB) — Ana arka plan yüzeyi. Sıcak-nötr bir tondur, asla klinik mavi-beyaz içermez.
- **Pure Surface** (#FFFFFF) — Kart ve konteyner iç dolguları. Elevation (yükseklik) hissi için whisper shadow ile eşlenir.
- **Charcoal Ink** (#18181B) — Birincil güçlü metin ve başlık rengidir. (Zinc-950 derinliği - asla saf siyah #000000 kullanılmaz).
- **Steel Secondary** (#71717A) — Gövde metinleri, açıklamalar ve ikincil detaylar. (Zinc-500 sıcaklığı).
- **Muted Slate** (#94A3B8) — Üçüncül metinler, zaman damgaları ve pasif durumlar.
- **Whisper Border** (rgba(226,232,240,0.5)) — Kart sınırları ve yapısal 1px çizgiler. Derinlik için yarı saydamdır.
- **Diffused Shadow** (rgba(0,0,0,0.05)) — Kart yükseltileri için 40px blur ve -15px offset değerine sahip geniş yayılan soft gölge.

### Accent Selection (Singular Accent Rule)
- **Electric Blue** (#3B82F6) — SaaS, üretkenlik araçları ve odak noktaları için kullanılacak tek aksan rengidir. Satürasyonu %80'in altındadır.

### Banned Colors (Yasaklı Renkler)
- Mor ve violet neon gradyanlar (Klişe "AI Purple" estetiği kesinlikle yasaktır).
- Saf Siyah (#000000) kullanımı yasaktır.
- Proje genelinde sıcak ve soğuk gri tonlarının aynı anda karıştırılması yasaktır.

## 3. Typography Rules
- **Display/Headlines:** `Geist`, `Satoshi` veya `Outfit` — Harf arası hafifçe sıkıştırılmış (Track-tight: `-0.025em`), kontrollü akışkan ölçek. Hiyerarşi devasa boyutlarla değil, font ağırlığı (700-900) ve Charcoal Ink kontrastıyla sağlanmaktadır. Satır yüksekliği dar tutulur (`1.1`). 
- **Body:** Aynı yazı tipi ailesinin 400 ağırlığı kullanılır. Rahat satır yüksekliği (`1.65`), maksimum 65 karakter (65ch) genişlik sınırı ve Steel Secondary rengi uygulanır. Ölçek: `1rem` veya `1.125rem` arasındadır.
- **Mono:** `Geist Mono` veya `JetBrains Mono` — Kod blokları, meta veriler ve zaman damgaları için kullanılır. Arayüz yoğunluğu (Density) Level 7'yi geçtiğinde tüm sayılar otomatik olarak monospace fonta döner.

### Banned Fonts (Yasaklı Yazı Tipleri)
- `Inter` yazı tipi premium ve yaratıcı bağlamlarda tamamen YASAKTIR.
- `Times New Roman`, `Georgia`, `Garamond` gibi jenerik tarayıcı serif fontları YASAKTIR. (Eğer editoryal bir dokunuş gerekirse sadece `Fraunces` veya `Instrument Serif` gibi modern ve karakteristik serifler kullanılabilir; ancak dashboard veya yazılım arayüzlerinde serif kullanımı tamamen yasaktır).

## 4. Component Stylings
* **Buttons:** Düz yüzeyli, dış ışıması (outer glow) olmayan net yapılar. Primary aksiyonlar Electric Blue dolgu ve beyaz metin içerir. Secondary aksiyonlar ghost veya outline border ile çözülür. Aktif (click) durumunda `-1px translateY` veya `scale(0.98)` ile fiziksel bir basılma hissi verilir.
* **Cards & Containers:** Cömertçe yuvarlatılmış köşeler (`2.5rem`), saf beyaz dolgu, yarı saydam whisper border ve diffused shadow kombinasyonu ile oluşturulur. İç boşluklar `2rem` ile `2.5rem` arasındadır. Kartlar sadece hiyerarşik bir yükselme gerekiyorsa kullanılır; yüksek yoğunluklu alanlarda kart yerine `border-top` ayıraçları veya negatif boşluklar tercih edilir.
* **Inputs/Forms:** Etiket (Label) her zaman girdinin üstünde yer alır; hata mesajları altta Deep Rose tonlarında gösterilir. Odaklanma (Focus) durumunda aksan renginde 2px offset değerine sahip bir halka belirir. Floating label (yüzen etiket) kullanımı kesinlikle yasaktır.
* **Loaders (Yükleme Durumları):** Dairesel dönen jenerik spinner'lar yasaktır. Ekran ve kart boyutlarıyla birebir eşleşen, üzerinde yumuşak ışık dalgalanması olan iskelet yükleme şablonları (skeletal shimmer) kullanılmaktadır.
* **Empty/Error States:** Sadece "Veri bulunamadı" yazısı değil; yönlendirici bir kompozisyon barındırır. Hata durumları satır içi (inline) ve kurtarma aksiyonu içerecek şekilde kurgulanır.

## 5. Hero Section (İlk İzlenim Mimarisi)
* **Inline Image Typography:** Başlıkların ve büyük fontlu vurguların kelimeleri veya harfleri arasına, font yüksekliğiyle eşit, kenarları yuvarlatılmış küçük bağlamsal fotoğraflar/ikonlar inline olarak gömülür (Örn: "Geleceği [görsel] birlikte [görsel] kodluyoruz"). Bu, sistemin imza kreatif tekniğidir.
* **Sıfır Çakışma:** Metinler asla bir görselin veya başka bir katmanın üzerine binmez. Her elemanın kendine ait temiz bir mekansal alanı vardır. İçerik katmanlarında `z-index` istifleme yasaktır.
* **Gereksiz Yönlendirme Yasağı:** "Aşağı kaydır", "Keşfetmek için kaydır" metinleri, zıplayan ok ikonları veya chevron animasyonları tamamen BANNED. İçeriğin kalitesi kullanıcıyı doğal olarak aşağı çekmelidir.
* **Asimetrik Yapı:** Bu varyans seviyesinde ortalanmış (centered) Hero düzenleri yasaktır. Split Screen (50/50), sol blok metin / sağ blok görsel veya geniş boşluklara sahip asimetrik yerleşimler zorunludur.
* **CTA Kısıtlaması:** Maksimum bir adet primary CTA butonu bulunabilir. İkincil "Daha fazla bilgi al" linkleri veya buton altı mikro yazıları kullanılmaz.

## 6. Layout & Responsive Principles
* **Grid-First:** Tüm yapısal iskelet CSS Grid ile kurulur. Flexbox ile yüzdelik matematik hesapları yapmak (`calc(33% - 1rem)`) kesinlikle yasaktır.
* **Bento Mimari:** Özellik listeleri ve feature grid'ler için jenerik "yan yana 3 eşit kart" dizilimi YASAKTIR. Bunun yerine Asymmetric Bento düzenleri (Örn: Satır 1: 3 sütun | Satır 2: %70/%30 bölümlenmiş 2 sütun) tercih edilir.
* **Mobile-First Collapse (< 768px):** Çok sütunlu tüm bento ve asimetrik grid düzenleri mobil ekranlarda istisnasız tek sütuna (`width: 100%`) düşer. Mobil cihazlarda horizontal overflow (sağa doğru taşma ve yatay kaydırma) oluşması kritik bir sistem hatasıdır.
* **Dokunma Alanları:** Mobil cihazlarda tıklanabilir tüm alanlar minimum `44px` boyutundadır ve butonlar mobil modda tam genişlik (`width: 100%`) alır.

## 7. Motion & Interaction (Code-Phase Intent)
*Bu bölüm, Claude/Cursor gibi kodlama ajanlarının tasarımı canlı ürüne dönüştürürken uygulayacağı animasyon niyetlerini tanımlar:*
* **Spring Physics:** Tüm hareket motoru spring tabanlıdır (`stiffness: 100, damping: 20`). Arayüzün hiçbir yerinde doğrusal (linear/ease) yumuşatma kullanılmaz; her hareketin premium ve ağırbaşlı bir kütle hissi olmalıdır.
* **Sürekli Mikro Döngüler (Perpetual Micro-Loops):** Aktif ekran elementleri arka planda sonsuz bir mikro döngü durumundadır: Durum ikonlarında hafif pulse, arama çubuklarında typewriter efekti, yükleme alanlarında shimmer dalgalanması.
* **Kademeli Reveal:** Listeler ve grid elemanları ekrana aynı anda pat diye gelmez; indeks değerine göre hesaplanan gecikmelerle (`animation-delay: calc(var(--index) * 100ms)`) şelale etkisiyle akar.
* **Performans:** Animasyonlar sadece `transform` ve `opacity` özellikleri üzerinden donanım hızlandırmalı (hardware-accelerated) olarak tetiklenir. `top`, `left`, `width`, `height` animasyonları kesinlikle yasaktır.

## 8. Anti-Patterns (Banned / Kesinlikle Yasaklı AI Alışkanlıkları)
* **No Emojis:** Arayüzün hiçbir yerinde, kod yorum satırlarında veya görsel alt metinlerinde emoji kullanılamaz.
* **No AI Copywriting Clichés:** Metin yazarlıklarında "Elevate", "Seamless", "Unleash", "Next-Gen", "Revolutionize", "Empower" gibi yapay zekanın sıkça tekrarladığı jenerik kelimeler kesinlikle yasaktır.
* **No Fake Round Numbers:** Sahte ve uydurma izlenimi veren `%99.99`, `%50`, `1.000.000+` gibi sayılar yerine, arayüze gerçekçi organik veriler (`%47.2`, `14.820`) yerleştirilir.
* **No Generic Names:** Yer tutucu olarak "John Doe", "Acme Corp", "Nexus", "SmartFlow" gibi jenerik isimler kullanılamaz; özgün isim projeksiyonları yapılmalıdır.
* Jenerik `shadcn/ui` varsayılan border-radius ve renk ayarları doğrudan kullanılamaz; sistemin kendi `2.5rem` köşelerine ve şeffaf whisper sınırlarına göre customize edilmelidir.