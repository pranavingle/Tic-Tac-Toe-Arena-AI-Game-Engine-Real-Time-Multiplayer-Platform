# Visual Design System Specification — Tic-Tac-Toe Arena

> **Document Status**: Active / Living Document  
> **Last Updated**: 2026-09-23  
> **Current Project Phase**: Phase 1 — Project Foundation  

---

## 1. Design Direction & Visual Identity

Tic-Tac-Toe Arena follows a **Sleek Cyberpunk / Glassmorphism Dark Aesthetic**. The visual identity balances high-contrast gaming energy with modern clean UI interfaces.

### Core Visual Principles
- **Deep Dark Canvas**: High contrast slate dark backgrounds to maximize visual impact of neon accents.
- **Glassmorphic Cards**: Translucent dark surfaces with subtle background blur (`backdrop-filter: blur(12px)`) and thin glowing borders.
- **Neon Accent Indicators**: Distinct neon colors for Player X (Vibrant Cyan), Player O (Neon Violet), and Special Alerts / Wins (Gold & Rose).
- **Tactile Micro-Interactions**: Hover glows, subtle button press compressions, and fluid board placement animations.

---

## 2. Color System Palette

```
┌───────────────────┬───────────────────────┬──────────────────────────┐
│ Token Name        │ Value (Hex / HSL)     │ Intended Usage           │
├───────────────────┼───────────────────────┼──────────────────────────┤
│ Background Canvas │ `#0b0f19` (222 37% 7%)│ Main App Background      │
│ Surface Glass     │ `#131b2e` (222 41% 13%)│ Cards, Modals, Containers│
│ Border Subtle     │ `#1e293b` (217 33% 17%)│ Card & Grid Borders      │
│ Primary Cyan (X)  │ `#00f0ff` (184 100% 50%)│ Player X Symbol & Glow   │
│ Accent Violet (O) │ `#a855f7` (271 91% 65%)│ Player O Symbol & Glow   │
│ Highlight Gold    │ `#fbbf24` (43 96% 56%) │ Win Lines & Ratings      │
│ Error Rose        │ `#f43f5e` (343 89% 60%)│ Validation / Errors      │
│ Text Primary      │ `#f8fafc` (210 40% 98%)│ Headings & Main Text     │
│ Text Muted        │ `#94a3b8` (215 16% 65%)│ Subtitles & Labels       │
└───────────────────┴───────────────────────┴──────────────────────────┘
```

---

## 3. Typography & Font Strategy

- **Primary Sans-Serif Font**: `Inter` or `Outfit` (Clean, modern geometric sans-serif for UI titles, body text, buttons).
- **Monospace Telemetry Font**: `JetBrains Mono` or `Fira Code` (For AI search node counts, decision time ms, board state coordinate logs).
- **Font Scale**:
  - `Display H1`: 2.5rem (40px) / Bold 700 / Tracking tight
  - `Header H2`: 1.75rem (28px) / SemiBold 600
  - `Subheader H3`: 1.25rem (20px) / Medium 500
  - `Body Regular`: 1rem (16px) / Normal 400
  - `Caption / Micro`: 0.875rem (14px) / Normal 400

---

## 4. Layout, Grid & Spacing System

- **Base Spacing Unit**: `4px` grid (8px, 12px, 16px, 24px, 32px, 48px).
- **Border Radius**:
  - Small Elements (Inputs, Badges): `8px`
  - Medium Elements (Buttons, Modals): `12px`
  - Large Elements (Board Containers, Glass Cards): `16px`

---

## 5. Component Design Specifications

### 5.1 Game Board Grid `[PLANNED]`
- 3x3 square grid centered on screen.
- Cell dimensions: Minimum `100px x 100px` (desktop: `140px x 140px`).
- Cell border: Thin 1px translucent glass border (`#1e293b`).
- Cell hover: Subtle cyan/violet tint fill on active turn cursor.
- **X Marker**: Cyan vector symbol with subtle drop-shadow glow (`0 0 15px rgba(0,240,255,0.5)`).
- **O Marker**: Neon violet vector symbol with drop-shadow glow (`0 0 15px rgba(168,85,247,0.5)`).
- **Winning Line Overlay**: Animated glowing gold line connecting winning 3-in-a-row cells.

### 5.2 Buttons & Controls `[PLANNED]`
- **Primary Action Button**: Solid cyan-to-blue gradient fill, dark bold text, hover elevation with glow.
- **Secondary Glass Button**: Translucent background, border outline, cyan hover text.
- **Danger Button**: Rose border/fill for forfeit or game reset.

### 5.3 Telemetry Modal / Panel `[PLANNED]`
- Glassmorphic slide-over or collapsible modal displaying real-time AI metrics:
  - Total states evaluated (formatted integer).
  - Branches pruned count (alpha-beta cuts).
  - Search depth level.
  - Decision elapsed time (ms).

---

## 6. Unfinalized Design Decisions

The following design options are explicitly marked as **`TO BE DECIDED`**:
- `[TO BE DECIDED]`: Choice of custom sound effects (sfx) for move placement, win notification, and draw alert.
- `[TO BE DECIDED]`: Light Mode theme implementation (Currently focusing exclusively on Dark Cyberpunk aesthetic).
- `[TO BE DECIDED]`: Animated 3D board tilt effect using Three.js / React Three Fiber vs 2D Framer Motion animations.
