# ULTRA-PREMIUM MOBILE RESPONSIVITY & TOUCH ERGONOMICS SPECIFICATION
# Target: AI Code Agent - Responsive Layout, Touch Gestures & Mobile Adaptive Flow
# Strict Rule: Eradicate hover dependencies, optimize for thumb zones, and implement dynamic sheet layout logic on mobile screens.

---

## 1. MOBILE LAYOUT & ADAPTIVE ARCHITECTURE (Mobil Düzen ve Alan Yönetimi)
* **The Bottom-Sheet Pattern (Alt Panel Modeli)**: On mobile viewports (`< 768px`), do NOT render the right "Inspector Panel" or the left "Sidebar" as desktop side-drawers. Convert them into native-feeling **Bottom Sheets** (Sliding drawers emerging from the bottom edge) wrapped in `fixed bottom-0 left-0 right-0 z-[9999] rounded-t-large`.
* **The 3-View Tab System (Ekran Bölümleme)**: Instead of the desktop 3-column split screen, merge the authoring workspace into a persistent bottom navigation bar on mobile. Divide the app into 3 global tabs: `[1. Steps/List]` `[2. Canvas/Preview]` `[3. Inspector/Settings]`. Only ONE tab must be visible at a time.
* **Safe Areas & Padding Boundaries**: Every fixed mobile element (like headers or bottom sheets) must strictly enforce safe area padding rules (`padding-bottom: env(safe-area-inset-bottom)`) to guarantee zero interference with OS navigation bars (iOS Home Bar / Android Navigation).

---

## 2. TOUCH ERGONOMICS & INTERACTIVE HIGHLIGHTS (Parmak Ergonomisi)
* **The Thumb Zone Rule (Başparmak Alanı)**: Core actionable buttons (Save, Export, Add Step) must be clustered strictly within the bottom 1/3 of the screen—the natural reach zone of the user's thumb. Avoid putting primary interactive triggers at the top edge.
* **Minimum Touch Target Size (Fitts' Law)**: Every clickable or touchable interactive element (step pills, hotspots, picker cards) MUST feature a minimum physical target zone of `48px x 48px` to eliminate double-tap frustrations. Expand hidden target areas via transparent padding if the visual node is small.
* **No-Hover Fallbacks (Hover Bağımlılığını Yok Etme)**: Desktop elements hidden behind cursor hovers (e.g., Delete/Duplicate triggers) must automatically reveal themselves persistently on mobile viewports. Alternatively, bind them to a single-tap activation model.

---

## 3. ADVANCED MOBILE JESTS & ANIMATIONS (Dokunma Jestleri ve Yay Fiziği)
* **Swipe-to-Dismiss / Pull-Down Sheet**: Bottom sheets must support a fluid pull-down gesture to collapse. Capture `touchstart`, `touchmove`, and `touchend` events to translate the sheet vertically (`transform: translateY()`). If pulled down beyond `100px`, trigger programmatic collapse with a swift spring cubic-bezier `cubic-bezier(0.32, 0.94, 0.6, 1)`.
* **Swipe-to-Delete for Step Cards**: Inside the step navigation tab, let users swipe a `.step-card` horizontally to the left. As the card translates `-80px`, slide out a red background container housing a clean trash icon underneath. A full swipe triggers deletion directly.
* **Double-Tap Inline Action**: To mirror desktop's double-click inline title editing, mobile triggers must accept rapid double-taps on headers to instantiate a system native `<input type="text">` with autofocus and automatic soft-keyboard lifting.

---

## 4. IMAGE INTERACTION & CANVAS ERGONOMICS (Görsel ve Hotspot Yönetimi)
* **Pinch-to-Zoom Canvas**: The main authoring screenshot canvas must support multi-touch gestures. Implement dual-pointer event listeners (`PointerEvent`) to measure distance changes, translating into a seamless canvas scaling factor (`transform: scale()`) so users can draw precision hotspots on tiny mobile screens.
* **Draggable Hotspot Adjusters**: Hotspot resizing anchors (corners) must expand their active touch interaction box from desktop's 6px up to 24px on mobile viewports, ensuring smooth movement without losing pointer connection.

---

## 5. MOBILE RENDERING METRICS & OPTIMIZATIONS (Mobil Performans)
* **Touch-Action Override**: Explicitly declare `touch-action: none` on the hotspot drawing canvas and the Sortable list handles to prevent the browser's default window scrolling/bouncing from interrupting active user interactions.
* **Virtual Keyboard Protection**: Inputs inside bottom sheets must use reactive viewport height styling (`height: 100dvh` instead of `100vh`) to prevent the mobile virtual keyboard from pushing critical action buttons out of the visible screen boundaries.

---

## 6. PRODUCTION MASTER MOBILE PROMPT (Yapay Zekaya Verilecek Komut)
"Inject mobile-first touch ergonomics using the principles defined in @MOBILE-RESPONSIVE.md. Refactor the layout underneath the mobile breakpoint (< 768px) to collapse desktop's 3-column architecture into a clean 3-Tab persistent navigation bar. Convert modal frameworks or sidebar views into spring-animated Bottom Sheets, switch hover events to explicit touch targets of minimum 48px, and bind mobile-friendly swipe gestures for sorting and deletion workflows."
