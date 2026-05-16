# 🚀 Vibe Coding SaaS Development Engine & Prompt Framework

Bu depo; yapay zeka kodlama ajanlarının (**Cursor, Claude Engineer, v0, GitHub Copilot**) sıradan ve ortalama kodlar üretmesini engelleyerek, onları **Stripe, Linear, Vercel ve Supabase** standartlarında çalışan kıdemli birer yazılım mimarı ve UI/UX mühendisine dönüştürmek için tasarlanmış **7 Muhafızlı Bir Prompt Mühendisliği Ekosistemidir**.

Özellikle üretkenlik, yüksek performans ve lüks mikro etkileşimler gerektiren tarayıcı tabanlı **SaaS (Software as a Service)** uygulamaları geliştirmek veya mevcut projeleri (Legacy Code) bu standartlara göre ameliyat etmek için kurgulanmıştır.

---

## 📂 Dosya Yapısı ve Sorumluluk Alanları

Depoda yer alan her bir `.md` dosyası, yapay zeka asistanının zihninde farklı bir uzmanlık katmanını (Bilişsel Filtre) tetikler:

*   **`GUIDE.md`**: Tüm sistemin master kullanım kılavuzudur. Projelere sıfırdan başlarken veya revizyon yaparken izlenecek operasyon reçetelerini içerir.
*   **`DESIGN.md`**: Projenin görsel anayasasıdır. Google Material Design 3 ve modern SaaS trendlerine uygun renk tokenlarını, tipografi ölçeklerini ve 8px katı grid hiyerarşisini dayatır. AI'ın kafasına göre piksel ve hex kodu uydurmasını engeller.
*   **`ANIMATIONS-INTERACTIONS.md`**: Arayüze lüks ve premium hissi veren harekettir. AI'a yay fiziğine dayalı (`cubic-bezier`) yumuşak geçişler, farenin konumuna duyarlı parlayan kenarlıklar (mouse-tracking glow) ve modern bento-grid yerleşimleri yazdırır.
*   **`UX-CRITIQUE.md`**: Bilişsel yükü azaltma kılavuzudur. Ekrandaki buton enflasyonunu yok eder. Statik düğmeler yerine sürükle-bırak, çift tıklama ile yerinde düzenleme (inline editing) ve kademeli gösterim (progressive disclosure) mantığını koda işler.
*   **`CODE-ARCHITECTURE.md`**: Yazılım kalitesi ve performans güvencesidir. AI'ın spagetti kod yazmasını yasaklar. Maksimum 50 satırlık saf (pure) fonksiyonlar, tekil doğruluk kaynağı (SSOT) state yönetimi ve event delegation mekanizmaları dayatır.
*   **`EDGE-CASES.md`**: Uygulamanın yıkılmazlık kalkanıdır. Tarayıcı depolama kotası sınırları (`QuotaExceededError`), dizi sınır aşım kontrolleri, asenkron işlem yarışları (race conditions) ve XSS (`<iframe>` sandbox) güvenliklerini zorunlu kılar.
*   **`MOBILE-RESPONSIVE.md`**: Mobil parmak ergonomisi ve dokunma mühendisliğidir. Mobilde hover (üzerine gelme) bağımlılıklarını sıfırlar. Masaüstü panellerini yay animasyonlu alt çekmecelere (Bottom Sheets) ve 3'lü Tab navigasyon yapısına dönüştürür. Minimum 48px dokunma hedefleri sağlar.
*   **`Master Prompt`**: Sıfırdan yepyeni bir SaaS projesine başlarken AI asistanına verilecek ana brifing komutudur.
*   **`Revize Master Prompt`**: Mevcut/eski bir projeyi kırmadan, parça parça ve modüler şekilde bu standartlara yükseltmek için kullanılan refactor komutudur.
*   **`Analiz Prompt`**: AI'ı kod yazmadan önce "Kullanıcı Davranış Simülatörü" moduna sokan tetikleyicidir. AI'ın koda insan gözüyle bakıp; fare yolculuğu (Fitts' Law), göz takibi (eye-tracking) ve mobil başparmak erişim alanı analizleri yapmasını sağlar.

---

## 🛠️ Nasıl Kullanılır? (Workflow)

Bu depoyu bilgisayarınızdaki ana geliştirme dizinine (Örn: `C:\py\`) klonlayın.

### Senaryo A: Sıfırdan Yeni Proje Üretimi
1. Projenizin üst dizini olan ana klasörü Cursor/VS Code ile açın (böylece AI tüm `.md` kılavuzlarını tarayabilir).
2. `Master Prompt` içeriğini kopyalayın, en alttaki proje amacınızı yazıp AI'a gönderin.
3. AI'ın kurduğu modüler iskelet üzerinden kodları parça parça üretmesini isteyin.

### Senaryo B: Mevcut Projenin SaaS Seviyesine Yükseltilmesi
1. `Analiz Prompt` içeriğini kopyalayın, en altına mevcut kodlarınızı veya sunucu linkinizi ekleyip AI'a gönderin.
2. AI'ın kod yazmasını engelleyin; size sunacağı **Bilişsel Simülasyon Raporunu** ve **Refactor Yol Haritasını** inceleyin.
3. Yol haritasını onayladıktan sonra sırasıyla tek cümlelik emirlerle cerrahi müdahaleyi başlatın:
   > *"Raporu onaylıyorum. Şimdi Milestone 1 (Veri ve Güvenlik) kodlarını üretmeye başla."*

---

## ⚖️ Lisans

Bu proje açık kaynaklıdır ve her geliştiricinin AI ile premium SaaS ürünleri üretebilmesi için özgürce kullanılabilir, genişletilebilir ve modifiye edilebilir.
