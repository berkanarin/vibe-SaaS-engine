# SENIOR UI/UX ARCHITECTURE & COGNITIVE REDUCTION SPECIFICATION
# Target: AI Agent UI Refactoring, Gesture Enforcement & Ergonomics
# Strict Rule: Eliminate button clutter, enforce direct manipulation, and minimize user friction.

---

## 1. COGNITIVE LOAD & ACTION CONSOLIDATION (Button & Clutter Control)
* **The 1-Primary-Action Rule**: Every unique viewport, modal, or major container card MUST have exactly ONE primary visual action button. All other actions must be downgraded to secondary (outlined) or tertiary (borderless text links).
* **Contextual Dropdowns (More Actions)**: If an element (e.g., a guide step, list item, or table row) requires more than 2 administrative actions (e.g., Edit, Clone, Delete, Export, Move Up, Move Down), do NOT spread them across the screen. Collapse them into a single, clean vertical triple-dot icon (`⋮`) dropdown menu.
* **Hover-Triggered Utilities (Contextual Disclosure)**: Destructive or secondary adjustments (like a "Delete Step" or "Duplicate Section" button) must be hidden by default (`opacity-0`) and seamlessly transition to visible (`opacity-100`) ONLY when the user hovers over that specific container block.
* **Proximity of Destructive Actions**: Place destructive actions (e.g., "Delete", "Wipe All Data") physically separated from progressive actions. Ensure destructive actions require a clear double-click confirmation or an intermediate popover check.

---

## 2. DIRECT MANIPULATION & GESTURE-BASED UX (Button Elimination Rules)
* **Re-ordering & Sorting via Drag and Drop**: Never generate static "Move Up" / "Move Down" buttons or a separate "Sorting Mode" toggle for lists or step sequences. Instead, place a subtle drag handle icon (`⠿`) on the left edge. Enforce drag-and-drop mechanics using native HTML5 Drag and Drop API or lightweight libraries (e.g., SortableJS) so users sort elements intuitively.
* **Destructive Gestures (Drag-to-Delete)**: Eliminate repetitive "Delete" buttons inside individual rows or lists. Implement an elegant, dynamic trash system:
  1. **Trash Can Dropzone**: The moment a user starts dragging a list item, reveal a distinct, visually isolated "Trash Zone" container (e.g., sliding out from the bottom-left corner of the screen with a smooth backdrop blur and red accent tint). Dropping the item into this zone executes the deletion.
  2. **Swipe to Delete**: On touch-enabled layouts or mobile breakpoints, support swiping rows horizontally to reveal a contextual red deletion panel behind the item.
* **Inline Dynamic Editing (Double-Click to Edit)**: Do not place an edit "pencil" icon next to every single piece of text or title. Implement inline editing: when a user double-clicks a title, description, or step text, instantly convert that text element into a focused input/textarea field. Save changes and revert to standard text automatically on `Blur` (clicking away) or when pressing `Enter`.
* **Keyboard-First Shortcuts**: Do not force users to seek buttons for repetitive core workflows. Map global event listeners for these universal shortcuts:
  * **Create New Item / Step**: `Ctrl + N` (or `Enter` when inside a production input field).
  * **Delete Selected Item**: `Delete` or `Backspace`.
  * **Undo / Redo Last Action**: `Ctrl + Z` / `Ctrl + Y`.

---

## 3. PROGRESSIVE DISCLOSURE & LAYOUT HIERARCHY
* **The 80/20 Rule for Forms**: Expose only the top 20% of fields that 80% of users need on initial load. Hide advanced metadata configurations, technical flags, and edge-case settings behind an explicit "Advanced Settings" collapsible accordion (`<details>` panel or equivalent animated drawer).
* **Dashboard Split / Screen Real Estate**: For tools involving heavy production (like a Guide Maker), separate the "Authoring Zone" (Inputs) from the "Preview Zone" (Output). Use a clean, persistent Split Screen layout (50/50 or 60/40 ratio) on desktop, or an un-cluttered Tab System (`Edit View` / `Live Output`) with persistent hotkeys.
* **Empty State Guidance**: Never render a blank slate or an empty table when no data exists. Replace empty surfaces with an instructional panel: a descriptive headline, a 1-sentence outcome explanation, and a single centered primary CTA button (e.g., "Create Your First Guide Step").

---

## 4. FORM INPUT ARCHITECTURE & HEURISTICS
* **Single-Column Scanning**: Stack inputs vertically in a single column to maintain a linear down-scanning eye movement. Multiple columns are allowed ONLY for tightly coupled, interdependent short values (e.g., `City + Zip Code` or `Duration Amount + Time Unit`).
* **Persistent Visual Context**: Labels must never disappear. Use Material-style floating inline labels or explicitly fixed labels positioned strictly above the input fields. Never rely solely on placeholder text for form fields.
* **Real-Time Contextual Validation**: Do not block user flow with massive validation modal errors at submission time. Highlight field boundaries inline using red outlines and present precise, micro-copy remediation text immediately below the affected field as the user types.

---

## 5. DESIGN FOR INTERACTIVITY (State Handling & Animation Safeguards)
* **Loading and Pending States**: Every single asynchronous action (e.g., "Generate HTML", "Save Draft") must visually disable the originating button and swap text for a localized spinner animation to prevent double-submission bugs.
* **Feedback Loops (Toast Notifications)**: Success operations (e.g., "Guide exported to clipboard") must trigger an auto-dismissing, non-intrusive notification (Toast) in the screen corner, rather than disrupting the workspace layout with alert boxes.
* **Sticky Focus Areas**: Long form processes must leverage a sticky header or a bottom utility ribbon for global controls (e.g., `[Cancel] [Save Changes]`), preventing users from scrolling thousands of pixels to find actions.

---

## 6. REFACTORING PROMPT TEMPLATE (How to instruct the AI)
When asking the AI to audit code, prepend your request with this strict command block:
"Analyze the provided code through the lens of @UX-CRITIQUE.md and apply strict cognitive reduction. Eradicate redundant buttons by enforcing direct manipulation rules: implement drag-and-drop sorting, a dynamic drag-to-delete dropzone layout, double-click inline text editing, and ensure all input structures align with single-column progressive disclosure safeguards."
