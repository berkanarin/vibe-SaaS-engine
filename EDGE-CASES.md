# BULLETPROOF APPLICATION RESILIENCY & EDGE-CASE SPECIFICATION
# Target: AI Code Agent - Zero-Crash Runtime, Security & Boundary Safeguards
# Project Context: Client-Side Interactive Guide Maker

---

## 1. BOUNDARY CONDITIONS & LIMIT ENFORCEMENT (Sınır Değerleri)
* **Maximum/Minimum String Inputs**: Guide titles, step names, and descriptions must have strict character limits. If a user inputs 5000+ characters, do NOT allow layout breakage. Enforce ellipsis (`text-overflow: ellipsis`) or multi-line wraps in the view layer.
* **Array Index Overflow/Underflow**: When deleting the currently active step, moving the first item "up", or moving the last item "down", the system must explicitly check array boundaries. Implement conditional guards (`if (index <= 0) return;`) to prevent out-of-bounds runtime crashes.
* **Large Data Scaling Thresholds**: If a user creates 100+ steps or 50+ chapters, blocking the browser's single thread is forbidden. The application must switch sidebar item rendering to lightweight virtualization or batched background processing.

---

## 2. CLIENT-SIDE PERSISTENCE & DATA LOSS PREVENTION (Veri Koruma)
* **Storage Quota Overflows**: `localStorage` and `sessionStorage` have a hard limit of ~5MB. If a guide contains massive embedded Base64 images or long scripts, saving will throw a `QuotaExceededError`. The AI must wrap all storage operations in a strict `try-catch` block:
  ```javascript
  try {
    localStorage.setItem('guidemaker_data', JSON.stringify(state));
  } catch (error) {
    if (error.name === 'QuotaExceededError') {
      NotificationUtils.toast('Storage is full! Please download your project as a JSON file.', 'error');
    }
  }
  ```
* **Accidental Navigation & Page Refresh**: If the user closes the tab, reloads, or clicks an external link while there are unsaved state changes, the application must intercept the action using the `window.onbeforeunload` API to present a recovery warning dialogue.
* **Corrupted Cache/State Recovery**: If the application boots and detects corrupted or partial JSON schema inside `localStorage`, it must not crash with a white screen. It must automatically isolate the broken string, dump it safely to an emergency recovery download, and re-initialize a clean default layout.

---

## 3. ASYNCHRONOUS SECURITY & INTERACTION SAFEGUARDS (Güvenlik)
* **Cross-Site Scripting (XSS) via HTML Injection**: Because the Guide Maker handles dynamic HTML previews, custom CSS injector blocks, and script payloads, the AI must strictly isolate the live output preview. All generated guides must render inside a heavily sandboxed `<iframe>` wrapper:
  ```html
  <iframe sandbox="allow-scripts allow-modals" referrerpolicy="no-referrer"></iframe>
  ```
* **Double-Action Race Conditions**: Fast clicking or automated script inputs on asynchronous buttons (e.g., "Export Project", "Compile Guide HTML") will create race conditions. Every operational button must immediately change its inner state to `disabled=true` and display a loading indicator until the current call stack clears.
* **Dynamic DOM Reference Validation**: Before querying or manipulating a dynamic element (e.g., `document.getElementById(activeStepId).focus()`), always verify that the target element currently exists in the live DOM node map using short-circuit operators.

---

## 4. ENVIRONMENT AND DEVICE AGNOSTIC RESILIENCY (Ortam Uyumu)
* **Offline Network Continuity**: The application is an "Offline Maker." If a third-party script, font asset, or icon pack fails to load via CDN due to poor or zero network connection, the system must degrade gracefully using local Web-Safe fonts and fallback inline SVGs.
* **Clipboard API Failures**: Reading or writing data directly to the clipboard (`navigator.clipboard.writeText`) can throw immediate permission blocks on certain browsers or non-HTTPS local setups. Always build an immediate fallback that renders the output code inside a hidden textarea with `.select()` and `document.execCommand('copy')`.

---

## 5. AUDITING PROMPT SPECIFICATION (Yapay Zekaya Verilecek Komut Şablonu)
When asking the AI to review a block of code before generation, always prepend this command:
"Audit the following logic using @EDGE-CASES.md. Inject boundary safeguards against empty arrays, implement a try-catch harness around browser local storage pipelines, enforce strict sanitization/sandboxing on dynamic preview components, and block all click-race condition possibilities."
