# Nexsas Motion Effects Reuse Guide

Bu dokuman, Nexsas template'inde sayfa acilisinda hissedilen blur, fade, move, text reveal ve katman etkilerini baska projelerde bagimsiz olarak yeniden kurmak icin hazirlandi.

Not: Bu rehber herhangi bir `./vendor` veya `./assets/main.js` yoluna bagli degildir. En altta tek dosyalik vanilla JavaScript uygulamasi bulunur. Istersen bu kodu `nexsas-motion.js` gibi bir dosyaya koyup her projede kullanabilirsin.

Kaynak incelemesinde ana davranis su dosyalardan cikarildi:

- `development/src/js/animation/reveal-animation.js`
- `development/src/js/animation/text-reveal.js`
- `development/src/js/animation/elements-move-up-on-scroll.js`
- `development/src/js/animation/avatar.js`
- `development/src/js/utils/progressive-blur-effect.js`
- `development/src/styles/common.css`
- `development/src/components/home/hero.htm`
- `development/src/components/about/hero.htm`

## Genel Hissiyat

Efekt tek bir animasyondan degil, bes katmanin birlikte calismasindan geliyor:

1. Elemanlar once CSS ile gorunmez basliyor.
2. Ana bloklar GSAP ile opacity `0 -> 1`, blur `16px -> 0`, pozisyon offset'i `60px/100px -> 0` olacak sekilde geliyor.
3. Baslik ve paragraflar satir satir maskelenip asagidan yukari dogru aciliyor.
4. Hero icindeki kartlar farkli gecikmelerle geldigi icin sahne katman katman kuruluyor.
5. Bazi gorseller ve kartlar sayfa scroll edildikce hafif kayiyor; bu da acilis sonrasinda sahneyi canli tutuyor.

## Bagimsiz Kurulum Mantigi

Orijinal template GSAP, ScrollTrigger, SplitText, Lenis ve Springer kullaniyor. Fakat baska projelerde birebir hissi tasimak icin yerel `vendor` dosyalarina ihtiyacin yok. Iki yol var:

1. **Tek dosya vanilla yol:** Asagidaki `nexsas-motion.js` kodunu kullan. Harici kutuphane istemez. Blur, fade, move, satir reveal, progressive blur ve scroll drift etkilerini kurar.
2. **GSAP yol:** Projende zaten GSAP varsa, ilerideki GSAP orneklerini kullanabilirsin. Bu durumda paketleri npm/pnpm ile projeye eklersin; `./vendor/...` gibi template'e ozel yollar kullanmazsin.

Tek dosya kullanim:

```html
<script defer src="/nexsas-motion.js"></script>
```

Veya Next.js / bundler icinde:

```ts
import './nexsas-motion';
```

GSAP ile kuracaksan paket mantigi:

```bash
pnpm add gsap lenis
```

SplitText lisansli GSAP eklentisidir. Bagimsiz projelerde ayni hissi lisanssiz almak icin vanilla satir bolme kodu veya `split-type` benzeri bir paket kullanabilirsin. Bu rehberin tek dosya versiyonu paket gerektirmez.

## Baslangic CSS'i

Flash of unstyled content olmasin diye animasyonlanacak elemanlar once saklaniyor.

```css
[data-opai-animate] {
  opacity: 0;
}

[data-text-reveal] {
  opacity: 0;
}

[data-text-reveal] .text-reveal-line {
  opacity: 0;
}
```

Bu uc kural yoksa sayfa acilirken elemanlar once normal hallerinde gorunur, sonra animasyon baslar. Efektin premium hissi bozulur.

## Ana Reveal Efekti

Kaynak selector:

```html
data-opai-animate
```

Varsayilan davranis:

- Opacity: `0 -> 1`
- Blur: `16px -> 0`
- Duration: `0.6s`
- Ease: `power2.out`
- Offset: `60px`
- Direction: `down`
- Trigger: scroll, `top 90%`

Ornek:

```html
<div data-opai-animate data-delay="0.3" data-instant>
  ...
</div>
```

### Attribute Tablosu

| Attribute | Varsayilan | Etki |
|---|---:|---|
| `data-delay` | `0` | Animasyon gecikmesi, saniye cinsinden. Hero'da genelde `0.3`, `0.4`, `0.5` gibi kademeli. |
| `data-duration` | `0.6` | Animasyon suresi. |
| `data-offset` | `60` | X veya Y eksenindeki baslangic mesafesi. |
| `data-direction` | `down` | Baslangic yonu. `left`, `right`, `down`, `up`. |
| `data-instant` | yok | Varsa scroll beklemeden sayfa yuklenince baslar. |
| `data-start` | `top 90%` | ScrollTrigger baslangici. |
| `data-end` | `top 50%` | ScrollTrigger bitis referansi. |
| `data-spring` | yok | Varsa `Springer(0.2, 0.8)` easing kullanir. |
| `data-rotation` | `0` | GSAP transform'a rotation ekler. |
| `data-animation-type` | `from` | `from` normal giris, `to` hedef animasyonu. |

Direction mapping:

```js
left  => x: -offset
right => x:  offset
down  => y:  offset
up    => y: -offset
```

Hero acilisinda kullanilan desen:

```html
<header data-opai-animate data-instant data-direction="up" data-offset="100"></header>

<h1 data-text-reveal>...</h1>

<p data-text-reveal data-reveal-delay="0.2">...</p>

<div data-opai-animate data-delay="0.3" data-instant>CTA</div>

<figure data-opai-animate data-delay="0.4" data-instant>Hero image</figure>

<div data-opai-animate data-delay="0.5" data-instant>Left card</div>
<div data-opai-animate data-delay="0.6" data-instant data-direction="right" data-offset="100">
  Right card
</div>
```

### Reusable JS

```js
function initRevealAnimation() {
  const elements = document.querySelectorAll('[data-opai-animate]');
  const SpringerFactory = window.Springer?.default;

  elements.forEach((elem) => {
    const duration = Number.parseFloat(elem.getAttribute('data-duration') || '0.6');
    const delay = Number.parseFloat(elem.getAttribute('data-delay') || '0');
    const offset = Number.parseFloat(elem.getAttribute('data-offset') || '60');
    const instant = elem.hasAttribute('data-instant') && elem.getAttribute('data-instant') !== 'false';
    const start = elem.getAttribute('data-start') || 'top 90%';
    const end = elem.getAttribute('data-end') || 'top 50%';
    const direction = elem.getAttribute('data-direction') || 'down';
    const useSpring = elem.hasAttribute('data-spring') && SpringerFactory;

    const vars = {
      opacity: 0,
      filter: 'blur(16px)',
      duration,
      delay,
      ease: useSpring ? SpringerFactory(0.2, 0.8) : 'power2.out',
    };

    if (direction === 'left') vars.x = -offset;
    if (direction === 'right') vars.x = offset;
    if (direction === 'down') vars.y = offset;
    if (direction === 'up') vars.y = -offset;

    if (!instant) {
      vars.scrollTrigger = { trigger: elem, start, end, scrub: false };
    }

    gsap.from(elem, vars);
  });
}

document.addEventListener('DOMContentLoaded', initRevealAnimation);
```

## Text Reveal Efekti

Kaynak selector:

```html
data-text-reveal
```

Davranis:

- Fontlar hazir olana kadar bekler.
- Metni satirlara boler.
- Her satira mask uygular.
- Satirlar `yPercent: 110` ve `opacity: 0` durumundan gelir.
- Hedef: `yPercent: 0`, `opacity: 1`.
- Duration: `0.8s`
- Stagger: `0.08s`
- Ease: `power3.out`
- Default delay: `0.1s`

Dogru kullanim:

```html
<h1 data-text-reveal>
  Clear product headline over two lines
</h1>

<p data-text-reveal data-reveal-delay="0.2">
  Supporting text arrives just after the title.
</p>
```

Not: Kaynak HTML'de bazi yerlerde `data-delay` kullanilmis, fakat `text-reveal.js` delay okumak icin `data-reveal-delay` bekliyor. Bu efekti baska projeye tasirken text reveal icin `data-reveal-delay` kullan.

### Reusable JS

```js
const LINE_CLASS = 'text-reveal-line';

function initTextReveal() {
  if (!window.gsap || !window.SplitText) return;

  gsap.registerPlugin(SplitText);
  if (window.ScrollTrigger) gsap.registerPlugin(ScrollTrigger);

  document.fonts.ready.then(() => {
    document.querySelectorAll('[data-text-reveal]').forEach((el) => {
      const delay = Number.parseFloat(el.dataset.revealDelay || '0.1');
      const instant = el.dataset.instant !== undefined && el.dataset.instant !== 'false';
      const start = el.dataset.start || 'top 90%';
      const end = el.dataset.end || 'top 50%';

      SplitText.create(el, {
        type: 'lines',
        mask: 'lines',
        linesClass: LINE_CLASS,
      });

      const lines = el.querySelectorAll(`.${LINE_CLASS}`);
      gsap.set(el, { opacity: 1 });

      const vars = {
        yPercent: 0,
        opacity: 1,
        duration: 0.8,
        stagger: 0.08,
        ease: 'power3.out',
        delay,
      };

      if (!instant && window.ScrollTrigger) {
        vars.scrollTrigger = { trigger: el, start, end, scrub: false };
      }

      gsap.fromTo(lines, { yPercent: 110, opacity: 0 }, vars);
    });
  });
}

document.addEventListener('DOMContentLoaded', initTextReveal);
```

## Progressive Blur Perdesi

Bu entrance animation degil; gorselin kenarina blur perdesi ekler. About hero gorselinin altinda kullaniliyor.

Kaynak selector:

```html
data-progressive-blur-effect
```

Ornek:

```html
<figure class="relative overflow-hidden">
  <img src="./image.png" alt="" />
  <div
    data-progressive-blur-effect
    data-intensity="250"
    data-position="bottom"
    class="h-[150px]"
  ></div>
</figure>
```

Davranis:

- Container absolute konumlanir.
- Uc farkli `backdrop-filter` layer uretilir.
- Layer'lar mask gradient ile farkli bolgelerde gorunur.
- Blur degerleri intensity ile carpilir.

Intensity formulu:

```txt
intensityFactor = intensity / 50
layer 1 blur = 1px * intensityFactor
layer 2 blur = 3px * intensityFactor
layer 3 blur = 6px * intensityFactor
```

`data-intensity="250"` icin:

- Layer 1: `5px`
- Layer 2: `15px`
- Layer 3: `30px`

Mask bolgeleri:

```txt
Layer 1: 0%  -> 25%
Layer 2: 25% -> 75%
Layer 3: 75% -> 100%
```

### Reusable JS

```js
function initProgressiveBlurEffect() {
  document.querySelectorAll('[data-progressive-blur-effect]').forEach((element) => {
    const intensity = Number.parseFloat(element.dataset.intensity || '50');
    const position = element.dataset.position || 'top';
    const intensityFactor = intensity / 50;

    const layers = [
      { blur: 1 * intensityFactor, maskStart: 0, maskEnd: 25, zIndex: 1 },
      { blur: 3 * intensityFactor, maskStart: 25, maskEnd: 75, zIndex: 2 },
      { blur: 6 * intensityFactor, maskStart: 75, maskEnd: 100, zIndex: 3 },
    ];

    const positionStyles = {
      bottom: { bottom: '0', left: '0', right: '0', top: 'auto' },
      top: { top: '0', left: '0', right: '0', bottom: 'auto' },
      left: { left: '0', top: '0', bottom: '0', right: 'auto' },
      right: { right: '0', top: '0', bottom: '0', left: 'auto' },
    };

    const gradientDirection = {
      bottom: 'to bottom',
      top: 'to top',
      left: 'to left',
      right: 'to right',
    };

    Object.assign(element.style, {
      position: 'absolute',
      zIndex: '10',
      pointerEvents: 'none',
      ...positionStyles[position],
    });

    layers.forEach((layer) => {
      const layerElement = document.createElement('div');
      const maskImage = `linear-gradient(${gradientDirection[position]}, transparent ${layer.maskStart}%, rgb(24 24 27) ${layer.maskEnd}%)`;

      Object.assign(layerElement.style, {
        position: 'absolute',
        inset: '0',
        pointerEvents: 'none',
        zIndex: String(layer.zIndex),
        backdropFilter: `blur(${layer.blur}px)`,
        WebkitBackdropFilter: `blur(${layer.blur}px)`,
        maskImage,
        WebkitMaskImage: maskImage,
      });

      element.appendChild(layerElement);
    });
  });
}

document.addEventListener('DOMContentLoaded', initProgressiveBlurEffect);
```

## Scroll Ile Hafif Katman Kaymasi

Kaynak selector:

```html
data-move-up-on-scroll-element
```

Template'te hero kartlari icin kullaniliyor. Orijinal kaynak computed `top` degerini alip ScrollTrigger ile yukari tasiyor. Bu gorsel olarak parallax hissi veriyor.

Stitch uyumlu projelerde `top` yerine `transform` kullan:

```js
function initLayerDrift() {
  document.querySelectorAll('[data-move-up-on-scroll-element]').forEach((element) => {
    const value = Number.parseFloat(element.dataset.moveUpValue || '8');

    gsap.fromTo(
      element,
      { yPercent: 0 },
      {
        yPercent: -value,
        ease: 'none',
        scrollTrigger: {
          trigger: element,
          start: 'top 90%',
          end: 'bottom 20%',
          scrub: 1,
        },
      }
    );
  });
}

document.addEventListener('DOMContentLoaded', initLayerDrift);
```

Ornek:

```html
<div
  data-move-up-on-scroll-element
  data-move-up-value="15"
  data-opai-animate
  data-delay="0.6"
  data-instant
  data-direction="right"
  data-offset="100"
>
  Floating card
</div>
```

## Hero Gorselinin Surekli Nefes Hareketi

Kaynak class:

```html
hero-img-animate
```

Davranis:

- Duration: `4s`
- Infinite
- Sadece `transform`
- `translateY(-2%)` ile `translateY(2%)` arasinda gider gelir.

```css
.hero-img-animate {
  animation: hero-img-animate 4s infinite;
}

@keyframes hero-img-animate {
  0%,
  100% {
    transform: translateY(-2%);
  }

  50% {
    transform: translateY(2%);
  }
}
```

## Avatar Pop Efekti

Kaynak selector:

```html
data-opai-avatar
```

Davranis:

- Duration: `1.5s`
- Opacity: `0 -> 1`
- Scale: `0 -> 1`
- Blur: `5px -> 0`
- Ease: `elastic.out(1, 0.7)`
- ScrollTrigger: `top 90%` / `bottom 20%`

Ornek:

```html
<img
  data-opai-avatar
  data-avatar-delay="0.2"
  data-avatar-direction="left"
  data-avatar-offset="16"
  data-avatar-scale="0"
  src="./avatar.png"
  alt=""
/>
```

## Katman Sirasini Kurma Recesi

Hero benzeri bir bolumde kademeli acilis icin su delay ritmi iyi calisiyor:

```txt
Header/nav:     0.0s, direction up, offset 100
Headline:       text reveal, default 0.1s
Body copy:      text reveal, 0.2s
Primary action: 0.3s
Main visual:    0.4s
Left card:      0.5s
Right card:     0.6s, direction right, offset 100
Inner items:    0.7s, 0.8s
```

Temel fikir: once metin okunabilir hale gelir, sonra aksiyon ve ana gorsel gelir, en son dekoratif/yardimci kartlar oturur. Boylece sahne tek anda patlamaz; goz soldan saga ve merkezden cevreye dogru yonlenir.

## Experience Cards Pattern

`Experience AI in action` bolumunde hos duran katmanli etki de ayni sistemin daha yogun kullanimi. Kaynak davranis su sekilde:

- Bolum basligi `data-text-reveal` ile satir reveal alir.
- Aciklama metni `data-text-reveal` ve `data-delay="0.2"` ile basliktan sonra gelir.
- Tum kart grid'i `data-opai-animate data-delay="0.3"` ile once tek bir blok olarak fade/blur/move girisi yapar.
- Grid icindeki bazi kartlar kendi icinde tekrar `data-opai-animate` kullanir.
- Ic katmanlarda delay `0.4 -> 0.7` araliginda ilerler.
- Bazi ic katmanlarda `data-instant`, `data-animation-type="to"`, `data-rotation` ve `data-offset="0"` kullanilarak kartlar yerinde belirip hafif acili kompozisyona oturur.

Bu bolumun ritmi:

```txt
Section title:        text reveal, 0.1s
Section description:  text reveal, 0.2s
Whole card grid:      reveal, 0.3s
Card 2 main image:    reveal, 0.4s
Card 2 back panel 1:  reveal, 0.5s
Card 2 back panel 2:  reveal, 0.6s
Card 2 back panel 3:  reveal, 0.7s
Card 4 image group:   reveal, 0.2s
Card 4 front image:   reveal, 0.3s
Card 4 back image:    reveal, 0.4s
Card 5 image group:   reveal, 0.4s
Card 5 angled image:  reveal, 0.5s, rotation 7.7deg, offset 0
Card 5 front image:   reveal, 0.6s
Card 5 back panel:    reveal, 0.7s, rotation -7.7deg, offset 0
CTA:                  reveal, 0.8s
```

### Neden Bu Kadar Iyi Hissettiriyor?

Kart grid'i once tek bir yuzey gibi geliyor, sonra secili kartlarin icindeki parcalar kendi siralarinda aciliyor. Yani animasyon iki seviyeli:

1. **Macro reveal:** kartlarin bulundugu grid sahneye giriyor.
2. **Micro reveal:** grid icindeki gorsel katmanlar sirayla beliriyor.

Bu iki seviye birlesince kullanici tek tek kart animasyonu degil, kurulmakta olan bir arayuz hissi goruyor.

### HTML Iskeleti

```html
<section class="experience-section">
  <div class="section-copy">
    <h2 data-text-reveal>Experience AI in action</h2>
    <p data-text-reveal data-reveal-delay="0.2">
      See how we delivered measurable success to our clients.
    </p>
  </div>

  <div data-opai-animate data-delay="0.3" class="experience-grid">
    <article class="experience-card wide">
      <h3>Neural network builders</h3>
      <img src="/images/builders.png" alt="" />
    </article>

    <article class="experience-card stack-card">
      <div class="stack-visual">
        <figure data-opai-animate data-delay="0.4" class="stack-layer stack-front">
          <img src="/images/nlp.png" alt="" />
        </figure>
        <div data-opai-animate data-delay="0.5" class="stack-layer stack-back back-1"></div>
        <div data-opai-animate data-delay="0.6" class="stack-layer stack-back back-2"></div>
        <div data-opai-animate data-delay="0.7" class="stack-layer stack-back back-3"></div>
      </div>
      <h3>Multilingual NLP models</h3>
    </article>

    <article class="experience-card collage-card">
      <div data-opai-animate data-delay="0.4" class="collage-visual">
        <figure
          data-opai-animate
          data-delay="0.5"
          data-instant
          data-rotation="7.7"
          data-animation-type="to"
          data-offset="0"
          class="collage-layer collage-top"
        >
          <img src="/images/recommendation-a.png" alt="" />
        </figure>

        <figure
          data-opai-animate
          data-delay="0.6"
          data-instant
          class="collage-layer collage-front"
        >
          <img src="/images/recommendation-b.png" alt="" />
        </figure>

        <div
          data-opai-animate
          data-delay="0.7"
          data-instant
          data-rotation="-7.7"
          data-animation-type="to"
          data-offset="0"
          class="collage-layer collage-back"
        ></div>
      </div>
      <h3>Recommendation systems</h3>
    </article>
  </div>

  <div data-opai-animate data-delay="0.8">
    <a href="/demo">Try a live demo</a>
  </div>
</section>
```

### CSS Iskeleti

```css
.experience-grid {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  gap: 1rem;
}

.experience-card {
  min-height: 18.75rem;
  border-radius: 2.5rem;
  background: #ffffff;
  padding: 2rem;
  overflow: hidden;
}

.stack-card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.stack-visual {
  position: relative;
  width: 14.375rem;
  height: 9.4375rem;
  margin-inline: auto;
}

.stack-layer {
  position: absolute;
  left: 50%;
  border-radius: 1rem;
  transform: translateX(-50%);
}

.stack-front {
  top: 0;
  z-index: 4;
  width: 14.375rem;
}

.stack-back {
  top: 0.25rem;
  background: #f1f4f6;
}

.back-1 {
  z-index: 3;
  width: 13.625rem;
  height: 8.0625rem;
}

.back-2 {
  z-index: 2;
  width: 12.875rem;
  height: 8.625rem;
}

.back-3 {
  z-index: 1;
  width: 12rem;
  height: 9.1875rem;
}

.collage-visual {
  position: relative;
  width: 12.875rem;
  height: 13.75rem;
  margin-inline: auto;
}

.collage-layer {
  position: absolute;
  border-radius: 1.25rem;
  overflow: hidden;
}

.collage-top {
  top: 0;
  right: 0.5rem;
  z-index: 10;
  width: 7.75rem;
}

.collage-front {
  bottom: 0;
  left: 50%;
  z-index: 20;
  width: 10.25rem;
  transform: translateX(-50%);
}

.collage-back {
  bottom: 2rem;
  left: 0.75rem;
  z-index: 10;
  width: 7.75rem;
  height: 9.1875rem;
  background: #e4eaee;
}
```

### Uygulama Kurali

Bu pattern'i baska projeye tasirken her karti ayri ayri geciktirmek yerine once grid wrapper'a `data-opai-animate data-delay="0.3"` ver. Sonra sadece gorsel olarak katmanli olan kartlarin icindeki parcalara `0.4`, `0.5`, `0.6`, `0.7` delay ver. Etkinin sirri bu: butun bolum birlikte gelir, sadece secili detaylar sonradan derinlik kazanir.

## React / Next.js Tasima Notu

Next.js veya React projesinde init fonksiyonlarini client component icinde `useEffect` ile calistir:

```tsx
'use client';

import { useEffect } from 'react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { SplitText } from 'gsap/SplitText';

export function MotionBoot() {
  useEffect(() => {
    gsap.registerPlugin(ScrollTrigger, SplitText);
    initRevealAnimation();
    initTextReveal();
    initProgressiveBlurEffect();
    initLayerDrift();

    return () => {
      ScrollTrigger.getAll().forEach((trigger) => trigger.kill());
    };
  }, []);

  return null;
}
```

Sayfada bir kez render et:

```tsx
export default function Page() {
  return (
    <>
      <MotionBoot />
      <Hero />
    </>
  );
}
```

## Tek Dosyalik Bagimsiz Uygulama

Bu bolum, herhangi bir vendor klasoru veya GSAP kurulumu olmadan calisan bagimsiz versiyondur. Bir dosya olustur:

```txt
public/nexsas-motion.js
```

Icine sunu koy:

```js
(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  const number = (value, fallback) => {
    const parsed = Number.parseFloat(value);
    return Number.isNaN(parsed) ? fallback : parsed;
  };

  const directionTransform = (direction, offset) => {
    if (direction === 'left') return `translate3d(${-offset}px, 0, 0)`;
    if (direction === 'right') return `translate3d(${offset}px, 0, 0)`;
    if (direction === 'up') return `translate3d(0, ${-offset}px, 0)`;
    return `translate3d(0, ${offset}px, 0)`;
  };

  const animate = (element, keyframes, options) => {
    if (prefersReducedMotion) {
      element.style.opacity = '1';
      element.style.transform = 'none';
      element.style.filter = 'none';
      return null;
    }

    return element.animate(keyframes, {
      fill: 'both',
      easing: 'cubic-bezier(0.22, 1, 0.36, 1)',
      ...options,
    });
  };

  const observeOnce = (element, callback, start = 0.1) => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (!entry.isIntersecting) return;
          observer.unobserve(entry.target);
          callback();
        });
      },
      { threshold: start, rootMargin: '0px 0px -10% 0px' }
    );

    observer.observe(element);
  };

  const initReveal = () => {
    document.querySelectorAll('[data-opai-animate]').forEach((element) => {
      const delay = number(element.dataset.delay, 0) * 1000;
      const duration = number(element.dataset.duration, 0.6) * 1000;
      const offset = number(element.dataset.offset, 60);
      const direction = element.dataset.direction || 'down';
      const instant = element.dataset.instant !== undefined && element.dataset.instant !== 'false';
      const rotation = number(element.dataset.rotation, 0);
      const scale = number(element.dataset.scale, 1);

      const baseTransform = getComputedStyle(element).transform;
      const hasBaseTransform = baseTransform && baseTransform !== 'none';
      const rotatePart = rotation ? ` rotate(${rotation}deg)` : '';
      const scalePart = scale !== 1 ? ` scale(${scale})` : '';
      const fromTransform = `${directionTransform(direction, offset)}${rotatePart}${scalePart}`;

      element.style.opacity = '1';

      const run = () => {
        animate(
          element,
          [
            {
              opacity: 0,
              filter: 'blur(16px)',
              transform: hasBaseTransform ? `${baseTransform} ${fromTransform}` : fromTransform,
            },
            {
              opacity: 1,
              filter: 'blur(0px)',
              transform: hasBaseTransform ? baseTransform : 'translate3d(0, 0, 0)',
            },
          ],
          { duration, delay }
        );
      };

      if (instant) run();
      else observeOnce(element, run);
    });
  };

  const splitIntoLines = (element) => {
    const text = element.textContent.trim().replace(/\s+/g, ' ');
    const words = text.split(' ');
    element.textContent = '';

    const measurer = document.createElement('span');
    measurer.style.visibility = 'hidden';
    measurer.style.position = 'absolute';
    measurer.style.whiteSpace = 'nowrap';
    measurer.style.font = getComputedStyle(element).font;
    document.body.appendChild(measurer);

    const maxWidth = element.clientWidth;
    const lines = [];
    let current = '';

    words.forEach((word) => {
      const test = current ? `${current} ${word}` : word;
      measurer.textContent = test;

      if (measurer.offsetWidth > maxWidth && current) {
        lines.push(current);
        current = word;
      } else {
        current = test;
      }
    });

    if (current) lines.push(current);
    measurer.remove();

    lines.forEach((line) => {
      const mask = document.createElement('span');
      const inner = document.createElement('span');

      mask.className = 'text-reveal-mask';
      inner.className = 'text-reveal-line';
      inner.textContent = line;

      mask.appendChild(inner);
      element.appendChild(mask);
    });

    return element.querySelectorAll('.text-reveal-line');
  };

  const initTextReveal = () => {
    document.querySelectorAll('[data-text-reveal]').forEach((element) => {
      const delay = number(element.dataset.revealDelay || element.dataset.delay, 0.1) * 1000;
      const instant = element.dataset.instant !== undefined && element.dataset.instant !== 'false';

      const run = () => {
        const lines = splitIntoLines(element);
        element.style.opacity = '1';

        lines.forEach((line, index) => {
          animate(
            line,
            [
              { opacity: 0, transform: 'translate3d(0, 110%, 0)' },
              { opacity: 1, transform: 'translate3d(0, 0, 0)' },
            ],
            {
              duration: 800,
              delay: delay + index * 80,
              easing: 'cubic-bezier(0.16, 1, 0.3, 1)',
            }
          );
        });
      };

      if (instant) run();
      else observeOnce(element, run);
    });
  };

  const initProgressiveBlur = () => {
    document.querySelectorAll('[data-progressive-blur-effect]').forEach((element) => {
      const intensity = number(element.dataset.intensity, 50);
      const position = element.dataset.position || 'top';
      const factor = intensity / 50;

      const layers = [
        { blur: 1 * factor, start: 0, end: 25, z: 1 },
        { blur: 3 * factor, start: 25, end: 75, z: 2 },
        { blur: 6 * factor, start: 75, end: 100, z: 3 },
      ];

      const positions = {
        bottom: { bottom: '0', left: '0', right: '0', top: 'auto' },
        top: { top: '0', left: '0', right: '0', bottom: 'auto' },
        left: { left: '0', top: '0', bottom: '0', right: 'auto' },
        right: { right: '0', top: '0', bottom: '0', left: 'auto' },
      };

      const gradients = {
        bottom: 'to bottom',
        top: 'to top',
        left: 'to left',
        right: 'to right',
      };

      Object.assign(element.style, {
        position: 'absolute',
        zIndex: '10',
        pointerEvents: 'none',
        ...positions[position],
      });

      layers.forEach((layer) => {
        const div = document.createElement('div');
        const mask = `linear-gradient(${gradients[position]}, transparent ${layer.start}%, rgb(24 24 27) ${layer.end}%)`;

        Object.assign(div.style, {
          position: 'absolute',
          inset: '0',
          pointerEvents: 'none',
          zIndex: String(layer.z),
          backdropFilter: `blur(${layer.blur}px)`,
          WebkitBackdropFilter: `blur(${layer.blur}px)`,
          maskImage: mask,
          WebkitMaskImage: mask,
        });

        element.appendChild(div);
      });
    });
  };

  const initLayerDrift = () => {
    const elements = [...document.querySelectorAll('[data-move-up-on-scroll-element]')];

    if (!elements.length || prefersReducedMotion) return;

    const update = () => {
      elements.forEach((element) => {
        const value = number(element.dataset.moveUpValue, 8);
        const rect = element.getBoundingClientRect();
        const progress = 1 - Math.min(Math.max(rect.top / window.innerHeight, 0), 1);
        element.style.transform = `translate3d(0, ${-value * progress}%, 0)`;
      });
    };

    update();
    window.addEventListener('scroll', update, { passive: true });
    window.addEventListener('resize', update);
  };

  const init = () => {
    initReveal();
    initTextReveal();
    initProgressiveBlur();
    initLayerDrift();
  };

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
```

CSS'e de sunu ekle:

```css
[data-opai-animate],
[data-text-reveal],
[data-text-reveal] .text-reveal-line {
  opacity: 0;
}

.text-reveal-mask {
  display: block;
  overflow: hidden;
}

.text-reveal-line {
  display: block;
  will-change: transform, opacity;
}

.hero-img-animate {
  animation: hero-img-animate 4s infinite;
}

@keyframes hero-img-animate {
  0%,
  100% {
    transform: translateY(-2%);
  }

  50% {
    transform: translateY(2%);
  }
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

HTML'de kullanim:

```html
<script defer src="/nexsas-motion.js"></script>

<h1 data-text-reveal data-instant>AI systems built for focused teams</h1>

<p data-text-reveal data-instant data-reveal-delay="0.2">
  Design the first viewport as layered content, then let each piece settle into place.
</p>

<div data-opai-animate data-instant data-delay="0.3">Primary action</div>

<figure data-opai-animate data-instant data-delay="0.4">
  <img class="hero-img-animate" src="/hero.png" alt="" />
</figure>

<div
  data-opai-animate
  data-instant
  data-delay="0.6"
  data-direction="right"
  data-offset="100"
  data-move-up-on-scroll-element
  data-move-up-value="15"
>
  Floating layer
</div>
```

## Kalite Kontrol Checklist

- `data-opai-animate` elemanlari CSS'te once `opacity: 0` olmali.
- Hero ustundeki kritik elemanlarda `data-instant` kullanilmali.
- Scroll altindaki bolumlerde `data-instant` kullanilmaz; ScrollTrigger devreye girer.
- Text reveal icin `data-reveal-delay` kullanilmali.
- Blur girisi icin ana reveal'de `filter: blur(16px)` korunmali.
- Kademeli sahne icin delay degerleri `0.1s` araliklarla ilerlemeli.
- Scroll parallax hissi icin `top` animasyonu yerine `transform` tabanli `yPercent` kullanilmali.
- Mobilde agir hareketleri azaltmak icin `prefers-reduced-motion` kontrolu eklenmeli.

## Reduced Motion Eki

```js
const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (reduceMotion) {
  document.querySelectorAll('[data-opai-animate], [data-text-reveal]').forEach((el) => {
    el.style.opacity = '1';
    el.style.transform = 'none';
    el.style.filter = 'none';
  });
} else {
  initRevealAnimation();
  initTextReveal();
  initProgressiveBlurEffect();
  initLayerDrift();
}
```

## En Kisa Uygulama

Asgari haliyle bu hissi almak icin:

1. CSS baslangic saklama kurallarini ekle.
2. GSAP, ScrollTrigger ve SplitText yukle.
3. `initRevealAnimation()` ve `initTextReveal()` calistir.
4. Hero elemanlarina `data-instant` ve kademeli `data-delay` ver.
5. Kart/gorsel katmanlarinda `data-direction` ve `data-offset="100"` kullan.
6. Kenar blur'u gerekiyorsa `data-progressive-blur-effect` ekle.
