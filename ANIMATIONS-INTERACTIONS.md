# ULTRA-PREMIUM SaaS INTERACTION & MOTION SPECIFICATION
# Target: AI Agent UI Generation - Linear, Vercel & Supabase Aesthetics
# Strict Rule: Enforce high-fidelity micro-interactions, spring mechanics, and tactical dark-mode glow.

---

## 1. HIGH-FIDELITY SURFACES & LUXURY EFFECTS (SaaS Yüzey Efektleri)

### 1.1 Dynamic Reactive Border Glow (Farenin Konumuna Göre Parlayan Kenarlıklar)
* **Concept**: Cards must have a structural dark outline that turns into a sharp, localized gradient light tracking the exact position of the user's cursor.
* **Tailwind & JS Recipe**: Bind `onmousemove` to capture percentage values (`--mouse-x`, `--mouse-y`). Use a pseudo-element (`before:absolute inset-0`) with a 1px mask and `radial-gradient(circle 150px at var(--mouse-x) var(--mouse-y), rgba(103,80,164,0.3), transparent 80%)`. 

### 1.2 Animated Conic Border (Dönen Neon Kontur)
* **Application**: Reserved exclusively for active states, premium loading pipelines, or the primary interactive element (e.g., Active Step Card).
* **CSS Rule**: Continuous rotation via CSS hardware-accelerated animations using `conic-gradient` wrapping from `{colors.primary}` to transparent. Mask the inner body with `background-clip: padding-box` so only a 1px neon line spins smoothly.

### 1.3 Layered Glassmorphism & Bento Depth (Derinlik ve Katman Algısı)
* **Bento Grid Panels**: Group content blocks using clean, asymmetrical layouts. Use high-contrast surface transitions: Base canvas is dark `#000000` or neutral `#0B0B0C`. Panels must use `bg-[#121214]/60 backdrop-blur-xl border border-white/[0.06]`.
* **Subtle Inner Shadows**: Apply a crisp inner top-border shadow `inset 0 1px 0 0 rgba(255,255,255,0.05)` to give buttons and panels a hardware-molded, 3D premium hardware finish.

---

## 2. COMPONENT-SPECIFIC SPRING INTERACTIONS (Bileşen Animasyonları)

### 2.1 Magnetic & Shimmer Action CTA (SaaS Buton Mekaniği)
* **Magnetic Hover (Fizik Tabanlı Esneme)**: Primary SaaS buttons must feel physical. On hover, translate the element coordinates slightly (`max 6px`) toward the cursor using CSS transitions mimicking a spring physics model: `transition: transform 0.3s cubic-bezier(0.25, 1, 0.5, 1)`.
* **Animated Shimmer Strip**: Infuse an uninterrupted, angled light beam sliding across the primary CTA button surface every 3 seconds: `linear-gradient(90deg, transparent, rgba(255,255,255,0.15), transparent) animate-[shimmer_3s_infinite]`.

### 2.2 Fluid List Reordering & Deletion (Sürükle-Bırak Mikrosaniyeleri)
* **Active Drag Ghosting**: When a guide step card is initiated for drag-and-drop, instantly append a high-blur canvas shadow: `box-shadow: 0 30px 60px -15px rgba(103,80,164,0.4)`. Tilt the container by exactly `1.5deg` to communicate weight.
* **Dynamic Destruction (Trash Gravitation)**: As an item hovers closer to the bottom-left destruction zone, apply an inverse scaling effect (`scale-100` down to `scale-75`) while executing a CSS `filter: blur()` sweep to simulate the item melting/gravitating into the trash pocket.

### 2.3 Command Palette & Keyboard Focus (Komut Paleti ve Odaklanma)
* **Instant Keyboard Overlay (`Ctrl + K`)**: The tool must feature a global Command Menu overlay (Linear-style). When invoked, open a centralized glassmorphism list with a sharp `scale-98` to `scale-100` transition utilizing `cubic-bezier(0.16, 1, 0.3, 1)`.
* **Focused Line Draw (Giriş Alanları)**: Form input fields on focus must draw a sharp, non-blurry accent line underneath using absolute relative widths changing from `0%` to `100%` inside 200ms.

---

## 3. GPU RENDERING METRICS & OPTIMIZATIONS (Performans Koruma)
* **Composite Layer Enforcement**: To prevent UI frame drops (lag) during complex layout shifts, use only composite properties: `transform`, `opacity`, and CSS `filters`. Never animate spatial footprints like `height` or `padding`.
* **GPU Activation Trigger**: Force the GPU thread to pre-render animated components by injecting `will-change: transform, opacity, filter` and `transform: translateZ(0)`.

---

## 4. PRODUCTION MASTER PROMPT (Yapay Zekaya Verilecek Komut Şablonu)
"Refactor the selected UI code base into a high-end SaaS product. Read the design foundations from @DESIGN.md and merge the luxury kinetic logic from @ANIMATIONS-INTERACTIONS.md. Enforce a deep bento-grid layout with a mouse-reactive border gradient glow, transform standard buttons into magnetic shimmer surfaces, and use high-speed spring-like cubic-bezier parameters for all layout transitions."
