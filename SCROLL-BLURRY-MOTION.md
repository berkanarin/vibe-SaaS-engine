# Single HTML Motion Reveal System — Codex Kullanım MD

Bu doküman, **tek dosyalık HTML/CSS/JavaScript projelerinde** DOM elemanlarını scroll sırasında **blurry reveal**, **soft motion**, **stagger**, **hafif bounce** ve **premium giriş animasyonları** ile görünür hale getirmek için hazırlanmıştır.

Bu rehber özellikle şu tip dosyalar için uygundur:

- Tek `index.html` içinde çalışan projeler
- `<style>` içinde tüm CSS’i bulunan arayüzler
- `<script>` içinde vanilla JavaScript kullanan sistemler
- Supabase, Firebase veya başka CDN scriptleri ile çalışan HTML uygulamaları
- React / Next.js kullanılmayan projeler
- Codex ile mevcut HTML dosyalarına animasyon sistemi ekleme işleri

---

## 1. Bu rehber hangi yapı için hazırlanmıştır?

Örnek proje yapısı:

```html
<!DOCTYPE html>
<html lang="tr">
<head>
  <style>
    /* Tüm CSS burada */
  </style>
</head>
<body>
  <div class="container" id="app">
    <div class="header">...</div>
    <div class="grid">
      <div class="card">...</div>
      <div class="card">...</div>
    </div>
  </div>

  <script>
    // Tüm JavaScript burada
  </script>
</body>
</html>
```

Bu yapı React değildir. Bu yüzden `motion/react`, `whileInView`, `variants`, `staggerChildren` gibi React component mantığı doğrudan kullanılmaz.

Bu dosya yapısı için en doğru yaklaşım:

```txt
CSS transition + IntersectionObserver + class toggle
```

Yani:

- Elemanlara başlangıçta `.reveal` class’ı verilir.
- JavaScript viewport’a giren elemanı yakalar.
- Eleman görünür olduğunda `.is-visible` class’ı eklenir.
- CSS opacity, transform, blur ve scale değerlerini transition ile değiştirir.
- Stagger için `data-reveal-delay` kullanılır.
- Dinamik üretilen DOM’lar için helper function çalıştırılır.

---

## 2. Framer Motion burada kullanılmalı mı?

Bu tip tek HTML dosyalarında **Framer Motion / Motion for React kullanılmamalı**.

Sebep:

- Framer Motion React componentleriyle çalışır.
- Dosya yapısı React değildir.
- Mevcut CSS ve JS sistemini bozabilir.
- Tek HTML dosyasına React eklemek projeyi gereksiz büyütür.
- Codex’in mevcut sistemi koruması zorlaşır.

Bu tip projelerde Motion hissini CSS + JS ile üretmek daha doğru olur.

---

## 3. Tailwind burada kullanılmalı mı?

Bu tarz mevcut HTML dosyalarında Tailwind de doğrudan eklenmemeli.

Sebep:

- Dosyada zaten özel CSS token sistemi vardır.
- `:root` içinde renkler, gölgeler, radius ve transition değişkenleri bulunur.
- Tailwind CDN eklemek mevcut class yapısını değiştirmeye zorlayabilir.
- Tasarım zaten custom CSS üzerinden kontrol edilmektedir.

Öneri:

```txt
Tailwind ekleme.
Mevcut CSS değişkenlerini kullan.
Yeni animasyon sistemini mevcut <style> bloğuna ekle.
```

---

## 4. Codex için temel görev

Codex’e verilecek ana hedef:

```md
Bu tek HTML dosyasına React, Next.js veya Tailwind ekleme.

Mevcut tasarımı, layout’u, Supabase bağlantısını ve işlevleri koru.

Sadece CSS + vanilla JavaScript ile scroll-triggered blurry reveal animasyon sistemi ekle.

Elemanlar viewport’a girdikçe:
- opacity 0’dan 1’e gelsin
- y ekseninde aşağıdan yukarı otursun
- scale 0.96’dan 1’e gelsin
- blur 14px’ten 0px’e insin
- hafif bounce hissi olsun
- grid/list elemanları teker teker gelsin

Mevcut .card, .header, .month, .room-btn, .reservation-toolbar, .table-scroll, .modal-content gibi yapıları bozmadan uygula.
```

---

## 5. Eklenecek CSS bloğu

Bu CSS bloğu mevcut `<style>` içinde, animasyon tanımlarından sonra veya `.card` bölümünden önce eklenebilir.

```css
/* ── SCROLL REVEAL MOTION SYSTEM ───────────────────────────────────────────── */

:root {
  --reveal-y: 28px;
  --reveal-blur: 14px;
  --reveal-scale: .965;
  --reveal-duration: .72s;
  --reveal-ease: cubic-bezier(.22, 1.18, .34, 1);
}

/* Temel reveal */
.reveal {
  opacity: 0;
  transform: translate3d(0, var(--reveal-y), 0) scale(var(--reveal-scale));
  filter: blur(var(--reveal-blur));
  transition:
    opacity var(--reveal-duration) var(--reveal-ease),
    transform var(--reveal-duration) var(--reveal-ease),
    filter var(--reveal-duration) var(--reveal-ease);
  transition-delay: var(--reveal-delay, 0ms);
  will-change: opacity, transform, filter;
}

/* Görünür durum */
.reveal.is-visible {
  opacity: 1;
  transform: translate3d(0, 0, 0) scale(1);
  filter: blur(0);
}

/* Daha yumuşak section girişi */
.reveal-soft {
  --reveal-y: 22px;
  --reveal-blur: 10px;
  --reveal-scale: .98;
  --reveal-duration: .64s;
}

/* Daha belirgin premium giriş */
.reveal-premium {
  --reveal-y: 36px;
  --reveal-blur: 18px;
  --reveal-scale: .95;
  --reveal-duration: .82s;
}

/* Daha bounce hissi veren giriş */
.reveal-bounce {
  --reveal-y: 34px;
  --reveal-blur: 12px;
  --reveal-scale: .94;
  --reveal-duration: .78s;
  --reveal-ease: cubic-bezier(.2, 1.45, .28, 1);
}

/* Sadece fade isteyen küçük elemanlar */
.reveal-fade {
  opacity: 0;
  transform: translate3d(0, 10px, 0);
  filter: blur(6px);
  transition:
    opacity .48s var(--ease),
    transform .48s var(--ease),
    filter .48s var(--ease);
  transition-delay: var(--reveal-delay, 0ms);
}

.reveal-fade.is-visible {
  opacity: 1;
  transform: translate3d(0, 0, 0);
  filter: blur(0);
}

/* Dinamik eklenen itemlar için hızlı giriş */
.reveal-live {
  opacity: 0;
  transform: translate3d(0, 14px, 0) scale(.98);
  filter: blur(8px);
  transition:
    opacity .42s var(--ease),
    transform .42s var(--ease),
    filter .42s var(--ease);
}

.reveal-live.is-visible {
  opacity: 1;
  transform: translate3d(0, 0, 0) scale(1);
  filter: blur(0);
}

/* Mobilde hareketi sadeleştir */
@media (max-width: 768px) {
  .reveal,
  .reveal-soft,
  .reveal-premium,
  .reveal-bounce {
    --reveal-y: 16px;
    --reveal-blur: 8px;
    --reveal-scale: .985;
    --reveal-duration: .48s;
  }
}

/* Hareket azaltma tercihini destekle */
@media (prefers-reduced-motion: reduce) {
  .reveal,
  .reveal-soft,
  .reveal-premium,
  .reveal-bounce,
  .reveal-fade,
  .reveal-live {
    opacity: 1 !important;
    transform: none !important;
    filter: none !important;
    transition: none !important;
  }
}
```

---

## 6. Mevcut `.card` animasyonuyla çakışmayı önle

Mevcut dosyada `.card` üzerinde şöyle bir yapı olabilir:

```css
.card {
  animation: fadeInUp .5s var(--ease) both;
}
```

Scroll reveal sistemi kullanılacaksa bu satır çakışma yaratabilir.

Codex’e şu değişikliği yaptır:

```css
.card {
  /* animation: fadeInUp .5s var(--ease) both; */
}
```

veya daha güvenli şekilde:

```css
.card:not(.reveal) {
  animation: fadeInUp .5s var(--ease) both;
}
```

Aynı şekilde şu satırlar varsa, reveal sistemiyle beraber yeniden değerlendirilir:

```css
.card:nth-child(1){animation-delay:.04s}
.card:nth-child(2){animation-delay:.09s}
.card:nth-child(3){animation-delay:.14s}
.card:nth-child(4){animation-delay:.19s}
```

Önerilen yeni yaklaşım:

```css
.card:nth-child(1){ --reveal-delay: 40ms; }
.card:nth-child(2){ --reveal-delay: 90ms; }
.card:nth-child(3){ --reveal-delay: 140ms; }
.card:nth-child(4){ --reveal-delay: 190ms; }
```

---

## 7. Eklenecek JavaScript bloğu

Bu JS bloğu mevcut `<script>` içinde en alta, `DOMContentLoaded` veya `init()` çağrılarından sonra eklenebilir.

```js
// ═══════════════════════════════════════════════════════════════════════════════
// SCROLL REVEAL MOTION SYSTEM
// ═══════════════════════════════════════════════════════════════════════════════

const revealState = {
  observer: null,
  initialized: false,
};

function setupScrollReveal() {
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  if (reduceMotion) {
    document
      .querySelectorAll('.reveal, .reveal-soft, .reveal-premium, .reveal-bounce, .reveal-fade, .reveal-live')
      .forEach((el) => el.classList.add('is-visible'));
    return;
  }

  if (revealState.observer) {
    revealState.observer.disconnect();
  }

  revealState.observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return;

      const el = entry.target;
      const delay = el.getAttribute('data-reveal-delay');

      if (delay) {
        el.style.setProperty('--reveal-delay', `${delay}ms`);
      }

      el.classList.add('is-visible');
      revealState.observer.unobserve(el);
    });
  }, {
    threshold: 0.16,
    rootMargin: '0px 0px -70px 0px',
  });

  document
    .querySelectorAll('.reveal, .reveal-soft, .reveal-premium, .reveal-bounce, .reveal-fade, .reveal-live')
    .forEach((el) => {
      if (!el.classList.contains('is-visible')) {
        revealState.observer.observe(el);
      }
    });

  revealState.initialized = true;
}

function revealNow(selectorOrElement) {
  const el =
    typeof selectorOrElement === 'string'
      ? document.querySelector(selectorOrElement)
      : selectorOrElement;

  if (!el) return;

  requestAnimationFrame(() => {
    el.classList.add('is-visible');
  });
}

function revealChildren(containerSelector, childSelector, baseDelay = 60, step = 70) {
  const container = document.querySelector(containerSelector);
  if (!container) return;

  const children = [...container.querySelectorAll(childSelector)];

  children.forEach((child, index) => {
    child.classList.add('reveal-live');
    child.style.setProperty('--reveal-delay', `${baseDelay + index * step}ms`);
  });

  requestAnimationFrame(() => {
    children.forEach((child) => child.classList.add('is-visible'));
  });
}

document.addEventListener('DOMContentLoaded', () => {
  setupScrollReveal();
});
```

---

## 8. HTML içine class ekleme stratejisi

Mevcut HTML’i bozmadan şu class’lar eklenir.

Header:

```html
<div class="header reveal-premium" data-reveal-delay="0">
```

Ana kartlar:

```html
<div class="card reveal" data-reveal-delay="80">
```

Rezervasyon kartı gibi daha önemli alanlar:

```html
<div class="card reveal-premium" data-reveal-delay="120">
```

Arama / filtre paneli:

```html
<div class="card reveal-soft" id="mineCard">
```

Toolbar:

```html
<div class="reservation-toolbar reveal-fade" data-reveal-delay="220">
```

Tablo alanları:

```html
<div class="table-scroll reveal-soft" data-reveal-delay="160">
```

Modal içerikleri için scroll reveal değil, mevcut modal animasyonu korunur:

```html
<div class="modal-content">
```

Modal için ekstra reveal class’ı ekleme. Çünkü modal zaten `.show`, `scaleIn`, `slideUp` gibi kendi animasyonlarıyla çalışır.

---

## 9. Dinamik üretilen DOM’lar için yaklaşım

Bu tip HTML uygulamalarında bazı DOM’lar JavaScript ile sonradan üretilir.

Örnekler:

- Oda butonları
- Takvim ayları
- Gün kutuları
- Tablo satırları
- Rezervasyon blokları
- Gün özeti satırları

Bu elemanlara statik HTML’de class eklemek yeterli olmaz. Çünkü sayfa açıldıktan sonra JavaScript tarafından üretilirler.

Bu yüzden ilgili render fonksiyonlarına reveal class eklenmelidir.

---

## 10. Oda butonları için örnek

Mevcut üretim mantığı şuna benzer olabilir:

```js
roomsEl.innerHTML = state.rooms.map(room => `
  <button class="room-btn">
    ${room}
  </button>
`).join('');
```

Bunu şöyle değiştir:

```js
roomsEl.innerHTML = state.rooms.map((room, index) => `
  <button
    class="room-btn reveal-live"
    style="--reveal-delay:${index * 70}ms"
  >
    ${room}
  </button>
`).join('');

requestAnimationFrame(() => {
  document
    .querySelectorAll('#rooms .room-btn')
    .forEach((btn) => btn.classList.add('is-visible'));
});
```

---

## 11. Takvim ayları için örnek

Ay container’larına reveal ekle:

```html
<div class="month reveal-soft" style="--reveal-delay:120ms">
```

JavaScript ile üretiliyorsa:

```js
const monthDelay = monthIndex * 80;

html += `
  <div class="month reveal-soft" style="--reveal-delay:${monthDelay}ms">
    ...
  </div>
`;
```

Render sonrası:

```js
requestAnimationFrame(() => {
  document
    .querySelectorAll('.month.reveal-soft')
    .forEach((month) => month.classList.add('is-visible'));
});
```

---

## 12. Gün kutuları için daha hafif animasyon

Takvimde çok fazla `.day` olduğu için tüm günlere ağır blur vermek performansı düşürebilir.

Gün kutuları için ağır `.reveal` yerine hafif `.reveal-fade` veya özel class kullan.

```css
.day.day-enter {
  opacity: 0;
  transform: scale(.92);
  transition:
    opacity .28s var(--ease),
    transform .28s var(--ease);
  transition-delay: var(--day-delay, 0ms);
}

.day.day-enter.is-visible {
  opacity: 1;
  transform: scale(1);
}
```

Render sırasında:

```js
html += `
  <button
    class="day day-enter"
    style="--day-delay:${Math.min(dayIndex * 8, 180)}ms"
  >
    ${day}
  </button>
`;
```

Render sonrası:

```js
requestAnimationFrame(() => {
  document
    .querySelectorAll('.day.day-enter')
    .forEach((day) => day.classList.add('is-visible'));
});
```

Not: Gün kutularında blur kullanma. Sadece opacity + scale yeterlidir.

---

## 13. Tablo satırları için örnek

Tablo satırları dinamik üretiliyorsa:

```js
tbody.innerHTML = rows.map((row, index) => `
  <tr class="reveal-live" style="--reveal-delay:${Math.min(index * 35, 300)}ms">
    <td>${row.room}</td>
    <td>${row.date}</td>
    <td>${row.owner}</td>
  </tr>
`).join('');

requestAnimationFrame(() => {
  tbody
    .querySelectorAll('tr.reveal-live')
    .forEach((row) => row.classList.add('is-visible'));
});
```

Tablolarda gecikmeyi çok uzatma:

```js
Math.min(index * 35, 300)
```

---

## 14. Açılıp kapanan panel animasyonları

Mevcut dosyada şu tip yapı olabilir:

```css
#mineCard {
  overflow: hidden;
  max-height: 0;
  opacity: 0;
  transform: translateY(-6px);
  pointer-events: none;
}

#mineCard.open {
  max-height: 2400px;
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}
```

Bunu bozmadan daha yumuşak hale getirmek için:

```css
#mineCard {
  transition:
    max-height .55s cubic-bezier(.22, 1, .36, 1),
    opacity .34s var(--ease),
    transform .42s cubic-bezier(.22, 1.18, .34, 1),
    padding .35s var(--ease),
    border-width .35s var(--ease);
}

#mineCard.open {
  filter: blur(0);
}
```

İlk kapalı durumda blur da istenirse:

```css
#mineCard {
  filter: blur(8px);
}

#mineCard.open {
  filter: blur(0);
}
```

Ancak bu alanda ağır blur kullanma. Çünkü panel içinde tablo ve inputlar var.

---

## 15. Modal için öneri

Modal sistemine scroll reveal ekleme.

Modal için mevcut animasyonları iyileştir:

```css
.modal-content {
  animation: modalPop .28s cubic-bezier(.2, 1.35, .32, 1) both;
}

@keyframes modalPop {
  from {
    opacity: 0;
    transform: translateY(18px) scale(.96);
    filter: blur(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
    filter: blur(0);
  }
}
```

Bottom sheet modal için:

```css
.daily-summary-modal .modal-content,
.mine-action-sheet-modal .modal-content {
  animation: sheetReveal .28s cubic-bezier(.22, 1, .36, 1) both;
}

@keyframes sheetReveal {
  from {
    opacity: 0;
    transform: translateY(34px) scale(.98);
    filter: blur(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
    filter: blur(0);
  }
}
```

---

## 16. Hover animasyonlarıyla çakışma riski

Dikkat edilmesi gereken nokta:

`.card:hover` veya `.room-btn:hover` gibi class’larda zaten `transform` kullanılıyorsa, reveal class’ı da `transform` kullandığı için aynı anda çakışabilir.

Bu genelde sorun olmaz çünkü reveal tamamlandıktan sonra `.is-visible` transform’u `scale(1)` olur. Ancak daha güvenli yapı için hover transformlarını koru.

Şu yapı kabul edilebilir:

```css
.card.reveal.is-visible:hover {
  transform: translateY(-6px) scale(1);
}
```

Room button için:

```css
.room-btn.reveal-live.is-visible:hover {
  transform: translateY(-5px) scale(1.02);
}
```

---

## 17. Codex için ana uygulama promptu

Aşağıdaki prompt, örnek dosya gibi tek HTML uygulamalarında kullanılmak üzere hazırlanmıştır.

```md
Ekteki index.html dosyasını incele.

Bu proje React veya Next.js değil. Tailwind de kullanmıyor. Bu nedenle React, Framer Motion, motion/react, Tailwind veya build sistemi ekleme.

Mevcut dosya tek HTML içinde:
- <style> bloğunda custom CSS
- <script> bloğunda vanilla JavaScript
- Supabase CDN
- custom CSS variables
- .card, .header, .rooms, .calendar-grid, .month, .day, .modal, .modal-content gibi mevcut sınıflar
kullanıyor.

Görev:
Mevcut işlevleri bozmadan CSS + vanilla JavaScript ile scroll-triggered motion reveal sistemi ekle.

İstenen efekt:
- DOM elemanları ilk durumda opacity 0 olsun.
- Hafif aşağıdan gelsin.
- scale 0.95–0.98 arası başlasın.
- blur 8px–18px arası başlasın.
- Görünür olunca opacity 1, y 0, scale 1, blur 0 olsun.
- Hafif bounce hissi olsun ama abartılı olmasın.
- Kartlar, sectionlar ve dinamik liste elemanları teker teker gelsin.
- Mobilde animasyonlar daha sade çalışsın.
- prefers-reduced-motion desteği olsun.

Yapılacaklar:
1. Mevcut <style> içine SCROLL REVEAL MOTION SYSTEM başlıklı CSS bloğu ekle.
2. .reveal, .reveal-soft, .reveal-premium, .reveal-bounce, .reveal-fade, .reveal-live classlarını oluştur.
3. IntersectionObserver tabanlı setupScrollReveal() fonksiyonu ekle.
4. DOMContentLoaded içinde setupScrollReveal() çağır.
5. Header’a reveal-premium ekle.
6. Ana .card elemanlarına reveal veya reveal-premium ekle.
7. .reservation-toolbar ve .table-scroll gibi alt alanlara reveal-fade/reveal-soft ekle.
8. Mevcut .card animation: fadeInUp yapısı reveal ile çakışmayacak şekilde düzenle.
9. JavaScript ile sonradan üretilen DOM elemanlarında:
   - room-btn için reveal-live
   - month için reveal-soft
   - tablo satırları için reveal-live
   - gün kutuları için ağır blur değil, sadece day-enter classı kullan.
10. Render fonksiyonları bittikten sonra requestAnimationFrame ile yeni elemanlara is-visible ekle.
11. Modal sistemine scroll reveal ekleme; sadece modal giriş animasyonlarını yumuşat.
12. Supabase, state, kayıt, silme, filtreleme, rezervasyon ve tarih seçme fonksiyonlarını bozma.
13. Mevcut onclick, id ve class bağımlılıklarını değiştirme.
14. Tasarımı yeniden yazma; sadece motion layer ekle.
15. Dosyanın sonunda kısa yorum olarak hangi classların eklendiğini belirt.

Önemli:
Transform kullanan hover animasyonları ile reveal transformları çakışmasın.
.card:hover ve .room-btn:hover davranışlarını koru.
Mobilde sabit altta duran oda seçimi kartına ağır reveal animasyonu uygulama.
```

---

## 18. Daha kısa Codex promptu

```md
Bu tek HTML dosyasına React, Framer Motion veya Tailwind ekleme.

Mevcut CSS + vanilla JS yapısını koruyarak scroll reveal animasyon sistemi ekle.

CSS ile .reveal, .reveal-soft, .reveal-premium, .reveal-bounce, .reveal-fade, .reveal-live classları oluştur.
JavaScript tarafında IntersectionObserver ile viewport’a giren elemanlara .is-visible ekle.

Efekt:
opacity 0 -> 1
translateY(28px) -> 0
scale(.96) -> 1
blur(14px) -> 0
hafif bounce easing
stagger için data-reveal-delay veya --reveal-delay kullan.

.card, .header, .reservation-toolbar, .table-scroll gibi statik alanlara reveal classları ekle.
JS ile üretilen room-btn, month ve tablo satırlarına render sonrası reveal-live/is-visible uygula.
Takvim günlerinde blur kullanma, sadece hafif opacity + scale kullan.
Mevcut Supabase ve rezervasyon fonksiyonlarını bozma.
```

---

## 19. Direkt kopyalanabilir minimal patch

### CSS

```css
.reveal {
  opacity: 0;
  transform: translateY(28px) scale(.96);
  filter: blur(14px);
  transition:
    opacity .7s cubic-bezier(.22, 1.18, .34, 1),
    transform .7s cubic-bezier(.22, 1.18, .34, 1),
    filter .7s cubic-bezier(.22, 1.18, .34, 1);
  transition-delay: var(--reveal-delay, 0ms);
  will-change: opacity, transform, filter;
}

.reveal.is-visible {
  opacity: 1;
  transform: translateY(0) scale(1);
  filter: blur(0);
}

.reveal-live {
  opacity: 0;
  transform: translateY(14px) scale(.98);
  filter: blur(8px);
  transition:
    opacity .42s var(--ease),
    transform .42s var(--ease),
    filter .42s var(--ease);
  transition-delay: var(--reveal-delay, 0ms);
}

.reveal-live.is-visible {
  opacity: 1;
  transform: translateY(0) scale(1);
  filter: blur(0);
}

.day-enter {
  opacity: 0;
  transform: scale(.94);
  transition:
    opacity .24s var(--ease),
    transform .24s var(--ease);
  transition-delay: var(--day-delay, 0ms);
}

.day-enter.is-visible {
  opacity: 1;
  transform: scale(1);
}

@media (prefers-reduced-motion: reduce) {
  .reveal,
  .reveal-live,
  .day-enter {
    opacity: 1 !important;
    transform: none !important;
    filter: none !important;
    transition: none !important;
  }
}
```

### JS

```js
function setupScrollReveal() {
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  const revealEls = document.querySelectorAll('.reveal, .reveal-live');

  if (reduceMotion) {
    revealEls.forEach((el) => el.classList.add('is-visible'));
    return;
  }

  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return;
      entry.target.classList.add('is-visible');
      observer.unobserve(entry.target);
    });
  }, {
    threshold: 0.16,
    rootMargin: '0px 0px -70px 0px',
  });

  revealEls.forEach((el) => observer.observe(el));
}

document.addEventListener('DOMContentLoaded', setupScrollReveal);
```

---

## 20. Bu örnek dosya için önerilen class dağılımı

Statik alanlar:

```html
<div class="header reveal">
<div class="card reveal">
<div class="reservation-toolbar reveal">
<div class="table-scroll reveal">
```

Daha önemli alanlar:

```html
<div class="card reveal-premium">
```

Daha hafif alanlar:

```html
<div class="selection-summary reveal-fade">
```

JS ile üretilenler:

```html
<button class="room-btn reveal-live">
<div class="month reveal-live">
<tr class="reveal-live">
<button class="day day-enter">
```

Uygulanmaması gerekenler:

```txt
.modal
.modal-content
.toast
.sticky-delete-bar
.loading-badge
.day-tip
```

Bunlar zaten anlık durum/overlay elemanlarıdır. Scroll reveal eklemek beklenmedik davranış üretebilir.

---

## 21. Test listesi

Codex değişiklik yaptıktan sonra şu kontroller yapılmalı:

```txt
[ ] Sayfa ilk açıldığında header yumuşak geliyor mu?
[ ] Kartlar tek tek geliyor mu?
[ ] Oda butonları dinamik üretildikten sonra görünüyor mu?
[ ] Takvim ayları görünür mü?
[ ] Gün kutularında performans sorunu yok mu?
[ ] Rezervasyon kaydetme çalışıyor mu?
[ ] Rezervasyon silme çalışıyor mu?
[ ] Gün özeti modalı açılıyor mu?
[ ] Mobilde alttaki oda seçim kartı bozulmadı mı?
[ ] Inputlara tıklama, filtreleme, tablo scroll davranışı korunuyor mu?
[ ] Supabase bağlantısı etkilenmedi mi?
[ ] prefers-reduced-motion durumunda animasyon kapanıyor mu?
```

---

## 22. Net karar

Bu tip dosyalar için önerilen çözüm:

```txt
Framer Motion değil.
Tailwind değil.
React değil.
CSS + IntersectionObserver + mevcut class sistemine uyumlu motion layer.
```

Bu yaklaşım:

- Tek HTML yapısını korur.
- Codex’in uygulaması daha kolaydır.
- Mevcut Supabase ve DOM fonksiyonlarını bozma riski düşer.
- Dinamik üretilen elemanlara da uygulanabilir.
- Performans açısından daha güvenlidir.
