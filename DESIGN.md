---
$schema: https://design.md
colors:
  primary: "#6750A4"
  onPrimary: "#FFFFFF"
  primaryContainer: "#EADDFF"
  onPrimaryContainer: "#21005D"
  secondary: "#625B71"
  onSecondary: "#FFFFFF"
  secondaryContainer: "#E8DEF8"
  onSecondaryContainer: "#1D192B"
  surface: "#FEF7FF"
  surfaceContainer: "#F3EDF7"
  surfaceContainerHigh: "#ECE6F0"
  onSurface: "#1D1B20"
  onSurfaceVariant: "#49454F"
  outline: "#79747E"
  outlineVariant: "#CAC4D0"
  error: "#B3261E"
  onError: "#FFFFFF"
  success: "#2E7D32"
  onSuccess: "#FFFFFF"
typography:
  displayLarge:
    fontSize: "57px"
    fontWeight: "400"
    lineHeight: "64px"
  headlineMedium:
    fontSize: "28px"
    fontWeight: "600"
    lineHeight: "36px"
  titleLarge:
    fontSize: "22px"
    fontWeight: "500"
    lineHeight: "28px"
  bodyLarge:
    fontSize: "16px"
    fontWeight: "400"
    lineHeight: "24px"
  bodyMedium:
    fontSize: "14px"
    fontWeight: "400"
    lineHeight: "20px"
  labelLarge:
    fontSize: "14px"
    fontWeight: "500"
    lineHeight: "20px"
spacing:
  base: "8px"
  xs: "4px"
  small: "8px"
  medium: "16px"
  large: "24px"
  xl: "32px"
  xxl: "48px"
rounded:
  none: "0px"
  small: "8px"
  medium: "12px"
  large: "28px"
  full: "9999px"
---

## 1. VISION & SYSTEM PRINCIPLES
* **Deterministic Layouts**: Never generate floating pixel values. Every dimension must be a strict multiple of `{spacing.base}`.
* **Surface Hiearchy**: Visual depth is achieved through surface tones (`{colors.surfaceContainer}` -> `{colors.surfaceContainerHigh}`), not heavy dropshadows.
* **Interaction Clarity**: Every interactive item must explicitly declare standard, hover, active, and disabled visual branches using the defined token map.

---

## 2. DETAILED COMPONENT SPECIFICATIONS

### 2.1 Buttons & Interactive Elements
* **Filled Button (Primary Action)**:
  * Structure: Height: 40px, Padding: Horizontal `{spacing.large}`, Vertical `{spacing.small}`.
  * Styles: Background: `{colors.primary}`, Text: `{colors.onPrimary}`, Shape: `{rounded.full}`.
  * Font: `{typography.labelLarge}`.
  * States: Hover: Darken background by 8%. Active: Scale down slightly (0.98). Disabled: Background opacity 12% of `{colors.onSurface}`, Text opacity 38%.
* **Outlined Button (Secondary Action)**:
  * Structure: Border: 1px solid `{colors.outline}`, Shape: `{rounded.full}`.
  * Styles: Text: `{colors.primary}`, Background: Transparent.
* **Elevated Button / FAB (Floating Action)**:
  * Structure: Height: 56px, Width: Auto/56px, Shape: `{rounded.large}`.
  * Styles: Background: `{colors.primaryContainer}`, Text: `{colors.onPrimaryContainer}`. Shadow: `0px 4px 8px rgba(0,0,0,0.05)`.

### 2.2 Cards & Surface Containers
* **Standard Content Card**:
  * Structure: Padding: `{spacing.medium}`, Corner: `{rounded.medium}`.
  * Styles: Background: `{colors.surfaceContainer}`, Border: 1px solid `{colors.outlineVariant}`.
* **Interactive/Clickable Card**:
  * Transition: `background-color 0.2s ease, transform 0.2s ease`.
  * Hover State: Background shifts to `{colors.surfaceContainerHigh}`, Transform: `translateY(-2px)`.

### 2.3 Form Fields & Input Architecture
* **Text Inputs & Textareas**:
  * Structure: Min-height: 56px, Padding: `{spacing.medium}`, Shape: `{rounded.small}` top-corners, flat bottom.
  * Styles: Background: `{colors.surfaceContainerHigh}`, Border-bottom: 1px solid `{colors.onSurfaceVariant}`.
  * Typography: Input text uses `{typography.bodyLarge}`, placeholder text uses `{colors.onSurfaceVariant}` with 60% opacity.
  * States: Focus state switches border-bottom to 2px solid `{colors.primary}` and animates label upwards. Error state switches border to 2px solid `{colors.error}`.

### 2.4 Navigation & App Bars
* **Top Navigation Bar**:
  * Structure: Height: 64px, Padding: Horizontal `{spacing.medium}`.
  * Styles: Background: `{colors.surface}`, Border-bottom: 1px solid `{colors.outlineVariant}`.
* **Sidebar Navigation**:
  * Structure: Width: 256px, Padding: `{spacing.medium}` gap between items.
  * Active Item: Background: `{colors.secondaryContainer}`, Text: `{colors.onSecondaryContainer}`, Shape: `{rounded.full}`.

---

## 3. INTERACTIVE GUIDE MAKER - SPECIFIC PATTERNS
* **Step Container**: Every interactive step guide card must use `{colors.surfaceContainer}`, a left-border of 4px solid `{colors.primary}`, and padding of `{spacing.large}`.
* **Code Blocks / Output Previews**:
  * Background: `#1E1E1E` (Dark mode contrast), Text: `#E3E3E3`, Font: Monospace, Padding: `{spacing.medium}`, Shape: `{rounded.small}`.
* **Status Badges (Pills)**:
  * Success: Background `{colors.success}` (12% opacity), Text `{colors.success}`, Shape `{rounded.full}`.
  * Warning/Error: Background `{colors.error}` (12% opacity), Text `{colors.error}`.

---

## 4. STRICT AI GUARDRAILS (DO'S & DON'TS)
* **DO**: Group layout sections using distinct vertical grids (`{spacing.xl}` or `{spacing.xxl}`).
* **DO**: Maintain readable text accessibility by pairing `{colors.primary}` only with `{colors.onPrimary}`.
* **DON'T**: Invent inline styles, magical margins (e.g., 15px, 22px), or modern neon gradients.
* **DON'T**: Fall back to generic gray colors; use `{colors.onSurfaceVariant}` or `{colors.outline}` for subtle text/borders.
