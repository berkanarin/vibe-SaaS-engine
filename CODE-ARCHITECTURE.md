# SENIOR CODE ARCHITECTURE & SYSTEM QUALITY SPECIFICATION
# Target: AI Code Agent - Strict Enterprise Maintainability & Performance
# Project Context: Single-File / Browser-Based Application Ecosystem

---

## 1. ARCHITECTURAL PATTERN & STATE MANAGEMENT (Uygulama Mimarisi)
* **Single Source of Truth (SSOT)**: The application core MUST use a unified global State Object (e.g., `const AppState = { steps: [], activeStepId: null, settings: {} }`). 
* **State-Driven DOM Mutation**: AI must never read raw values directly from the UI text fields to compute operational logic. All mutations must occur inside `AppState` first, followed by a deterministic UI synchronization layer.
* **Separation of Concerns (SoC)**: Code must be decoupled into three strict logical layers:
  1. **Data layer**: Pure functions that compute state modifications (e.g., reordering arrays, appending raw metadata).
  2. **Event layer**: Handlers catching user inputs, debouncing triggers, or intercepting keydowns.
  3. **View layer**: Isolated rendering logic responsible for injecting sanitized markup into the DOM wrappers.

---

## 2. MODULAR CODE REUSABILITY & "DRY" COMPLIANCE (Temiz Kod Kuralları)
* **The 50-Line Method Threshold**: No single JavaScript function or routine should exceed 50 lines of execution logic. If a function expands past this boundary, the AI MUST abstract sub-tasks into standalone, testable utility helpers.
* **Pure Utility Extraction**: Core business workflows—such as generation of dynamic HTML templates, exporting asset JSON files, or counting multi-step totals—must be isolated into an immutable namespace object (e.g., `const GuideUtils = { ... }`).
* **UI Component Templates**: Repetitive visual constructs (e.g., step timeline cells, toggle input rows, toast notification containers) must use deterministic, parameter-driven ES6 Template Literals or decoupled component render methods.

---

## 3. PERFORMANT DOM MANIPULATION & INTERACTIVITY SAFEGUARDS
* **Universal Event Delegation**: Never attach recursive inline listeners (`onclick=""`) or multi-instance loops (`querySelectorAll().forEach().addEventListener`) to dynamic item grids. Attach a singular structural event listener to parent anchors (e.g., `#sidebar-wrapper`, `#workspace-main`) and filter operations via `event.target.closest()`.
* **Fragment-Based DOM Batching**: When refreshing rich step lists or timeline arrays, never perform iterative `innerHTML +=` or sequentially invoke `.appendChild()` inside loops. Accumulate the structural updates inside an offline Document Fragment (`document.createDocumentFragment()`) or batch string interpolations to execute exactly ONE paint mutation.
* **State Debouncing**: Heavy operations—including micro-compiling live previews, firing heavy `localStorage` writes, or calculating cross-step collision coordinates—must be wrapped in a 250ms asynchronous Debounce Function to protect main-thread rendering performance.

---

## 4. STRICT VARIABLE SCOPING, TYPING & CONSTANTS
* **Immutability First**: Default to `const` for all variable declarations. Use `let` only for tracking structural index loops or operational mutation values. The use of `var` is strictly forbidden.
* **Global Configuration Map**: Hardcoded settings, default parameters, system timeouts, and third-party fallback CDNs must be compiled at the apex of the file inside a frozen configuration freeze block:
  ```javascript
  const SYSTEM_CONFIG = Object.freeze({
    DEFAULT_THEME: 'light',
    AUTOSAVE_INTERVAL_MS: 1000,
    MAX_STEP_LIMIT: 200,
    DOM_IDS: { SIDEBAR: 'app-sidebar', PREVIEW: 'live-output-frame' }
  });
  ```
* **Explicit Defending / Type Casts**: Before performing mechanical array adjustments or arithmetic mutations, apply explicit type validations. Wrap data conversions via `Number()`, `String()`, or logical boolean check protocols (`Array.isArray()`) to deflect runtime crashes.

---

## 5. REFACTORING PROMPT SPECIFICATION (Yapay Zekaya Verilecek Komut Şablonu)
When directing the AI to modify or introduce functionality to the workspace codebase, prepend this directive block:
"Examine the designated script files using the strict parameters set in @CODE-ARCHITECTURE.md. Refactor the implementation to extract bloated routines into under-50-line pure functions, enforce event delegation on the root container anchors, isolate any inline data states into the central state object, and deploy batch document fragments for any dynamic markup rendering tasks."
