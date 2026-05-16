# VIBE CODING ULTRA-PREMIUM SaaS DEVELOPMENT ENGINE
# Core System Guide & Framework Execution Manual

---

## 1. ECOSYSTEM ARCHITECTURE & CORE ARTIFACTS
Çalışma alanınızın kök dizininde (`C:\py\`) bulunması gereken 7 adet muhafız `.md` dosyası ve bunların yapay zeka (AI) üzerindeki psikolojik/teknik sorumlulukları:


| Dosya Adı | Sorumluluk Alanı | AI Üzerindeki Etkisi |
| :--- | :--- | :--- |
| `DESIGN.md` | Görsel Temeller | Rastgele renk ve piksel üretmesini engeller. Google M3 & SaaS standartlarını dayatır. |
| `ANIMATIONS-INTERACTIONS.md` | SaaS Lüks Hareketi | Lineer/Vercel tarzı yay fiziği (`cubic-bezier`), parlayan kenarlıklar ve bento grid üretir. |
| `UX-CRITIQUE.md` | Bilişsel Yük Kontrolü | Buton enflasyonunu yok eder. Sürükle-bırak, çift tıklama gibi jest tabanlı ergonomi dayatır. |
| `CODE-ARCHITECTURE.md` | Mühendislik Kalitesi | Spagetti kodu engeller. Maksimum 50 satırlık modüler fonksiyonlar ve net State (SSOT) zorlar. |
| `EDGE-CASES.md` | Yıkılmaz Altyapı | `try-catch` blokları, depolama kotası sınırları ve XSS (`<iframe>` sandbox) güvenlikleri ekletir. |
| `MOBILE-RESPONSIVE.md` | Mobil Parmak Ergonomisi | Hover bağımlılığını siler. 3-Tab navigasyon, Bottom-Sheet çekmeceleri ve `48px` dokunma hedefleri kurar. |
| `ANALIZ-PROMPT.md` | Bilişsel Denetim | AI'a kod yazmadan önce insan gözü, faresi ve parmağı gibi çalışan bir "Simülatör" zekası yükler. |

---

## 2. STEP-BY-STEP OPERATION WORKFLOWS (OPERASYON REÇETELERİ)

### 🟢 REÇETE A: Sıfırdan Yeni Bir Projeye Başlarken
1. **Workspace Hazırlığı:** `C:\py\` dizinini VS Code veya Cursor ile ana çalışma alanı (Workspace) olarak açın.
2. **Kılavuzları Kilitleyin:** Yukarıdaki ilk 6 `.md` dosyasının dizinde fiziksel olarak var olduğundan emin olun.
3. **Ateşleme (Brifing):** `GÜNCEL MASTER PROMPT` içeriğini kopyalayın, en alttaki `[PROJE DETAYLARI VE AMACI]` alanını kendi kelimelerinizle doldurup AI asistanınıza gönderin.
4. **Modüler İnşa:** AI'ın onayladığınız mimari iskelet üzerinden kodları adım adım, parça parça üretmesini sağlayın. Asla tek seferde tüm projeyi yazdırmayın.

### 🔵 REÇETE B: Mevcut/Eski Bir Projeyi Revize Ederken
1. **Doğrulama (Linting):** Terminalde `node node_modules/@google/design.md/dist/index.js lint DESIGN.md` komutunu çalıştırarak tasarım sistemi şemanızı test edin.
2. **Röntgen (Bilişsel Simülasyon):** `ANALIZ-PROMPT.md` içeriğinin tamamını kopyalayın. En alttaki ilgili alanlara projenizin canlı server linkini veya arayüzü oluşturan HTML/JS kod bloklarını yapıştırıp AI'a gönderin.
3. **Frenleme (Rapor Aşaması):** AI bu aşamada kesinlikle kod yazmamalıdır. Size sunacağı **"Bilişsel Simülasyon Raporu"** ve 3 aşamalı **"Refactor Yol Haritasını"** insan gözüyle inceleyin.
4. **Ameliyat (Zincirleme Yürütme):** Yol haritasını onayladıktan sonra, AI'a sırasıyla şu tetikleyici emirleri vererek projenizi aşama aşama güncelletin:
   * *"Raporu onaylıyorum. Şimdi Milestone 1 (Veri Katmanı) kodlarını üret."*
   * *"Aşama 1 harika çalışıyor. Şimdi Milestone 2 (Sol Panel ve Sürükle-Sil Jestleri) aşamasına geç."*

---

## 3. CORE COMMANDS & PROMPT TRIGGERS (SIHİRLİ KOMUTLAR)

* **AI'ı Sınırlandırma (Spagetti Engeli):** 
  > "Bu özelliği yazarken `@CODE-ARCHITECTURE.md` dosyasındaki 50 satır altı fonksiyon kuralına ve Event Delegation kurallarına sıkı sıkıya bağlı kal."
* **Modalları Yok Etme (SaaS Ergonomisi):** 
  > "Mevcut popupları kaldır. `@UX-CRITIQUE.md` ve `@ANIMATIONS-INTERACTIONS.md` kurallarına uygun, sağdan kayarak açılan cam efektli (`backdrop-blur`) bento-grid bir Inspector Panel inşa et."
* **Güvenlik ve Çökme Kontrolü:** 
  > "Yazdığın bu fonksiyonu `@EDGE-CASES.md` süzgecinden geçir. Tarayıcı depolama kotası dolduğunda veya dizi sınırları aşıldığında oluşabilecek hataları try-catch ile izole et."
