# Design System Reference

## Fixed Dimensions — NEVER CHANGE THESE

Every carousel slide is exactly **420x525px** at design width. Export uses Playwright's `device_scale_factor` to scale to **1080x1350px** output. This means the HTML layout stays at 420px — fonts, spacing, and elements remain pixel-perfect at any export size.

```
Design width:  420px
Design height: 525px
Export width:  1080px
Export height: 1350px
Scale factor:  1080 / 420 = 2.5714
Aspect ratio:  4:5
```

**CRITICAL**: Never set viewport or slide width to 1080px. Never use responsive/percentage-based widths on slides. Every slide is a fixed 420x525px box. This is what prevents export/resize issues.

---

## Font

**Poppins** — all styles, all slides, no exceptions.

```html
<link href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,400;0,500;0,600;0,700;1,400;1,700&display=swap" rel="stylesheet">
```

### Type Scale

| Role | Size | Weight | Line-height | Letter-spacing | Notes |
|------|------|--------|-------------|----------------|-------|
| Hero heading | 44-56px | 700 | 1.05 | -1px | Style-dependent size |
| Content heading | 36-48px | 700 | 1.1 | -0.5px | Centered or left per style |
| Body text | 16-20px | 400 | 1.5-1.6 | 0 | Regular or italic per style |
| Labels/tags | 10-11px | 600 | 1.2 | 1.5-2px | Uppercase |
| Small text | 8-9px | 500 | 1.2 | 1px | Watermark, subtitle |
| Numbers | 20px | 700 | 1 | 0 | Accent color, slide numbering |

---

## Global Color Tokens

These tokens are available to ALL styles. Each style overrides or extends them.

```css
:root {
  /* Brand */
  --brand-primary: #6EE421;
  --brand-light: #85F040;
  --brand-dark: #5BC01C;

  /* Dark palette */
  --dark-bg: #0c1729;
  --dark-elevated: #101d32;
  --dark-surface: #162540;
  --dark-border: #1e3050;

  /* Light palette */
  --light-bg: #ffffff;
  --light-elevated: #f8f8f6;
  --light-surface: #f0f0ec;

  /* Text on dark */
  --text-primary: #ffffff;
  --text-secondary: #8899aa;
  --text-muted: #5a6b7a;

  /* Text on light */
  --text-dark: #0c1729;
  --text-dark-secondary: #4a5568;
  --text-dark-muted: #94a3b8;

  /* Accents */
  --accent-coral: #e8553d;
  --accent-coral-light: #ff7b5f;
}
```

---

## Slide Architecture

### Base Slide CSS

Every slide shares this foundation:

```css
.slide {
  width: 420px;
  height: 525px;
  position: relative;
  overflow: hidden;
  font-family: 'Poppins', sans-serif;
  box-sizing: border-box;
}

.slide > * {
  position: relative;
  z-index: 2;
}
```

### Content Padding

| Slide type | Padding |
|-----------|---------|
| Standard content | `80px 36px 52px 36px` |
| Hero / CTA (centered) | `80px 36px 52px 36px` with `justify-content: center` |
| Bottom-heavy content | `80px 36px 52px 36px` with `justify-content: flex-end` |

The `52px` bottom padding clears the progress bar. The `80px` top padding clears the profile header.

---

## Carousel Container

The carousel lives inside an Instagram-style preview frame for in-chat viewing:

```css
.ig-frame {
  width: 420px;
  max-width: 420px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.12);
  overflow: hidden;
  font-family: 'Poppins', sans-serif;
}

.carousel-viewport {
  width: 420px;
  height: 525px;
  overflow: hidden;
  position: relative;
  cursor: grab;
}

.carousel-track {
  display: flex;
  height: 100%;
  transition: transform 0.3s ease;
}
```

**The .ig-frame must be exactly 420px wide. Do not change this.**

---

## Navigation

The carousel supports three input methods:

1. **Touch**: `touchstart` / `touchmove` / `touchend` with 50px swipe threshold
2. **Mouse**: `mousedown` / `mousemove` / `mouseup` drag
3. **Keyboard**: Left/Right arrow keys

Navigation code moves the `.carousel-track` via `transform: translateX(-${currentSlide * 100}%)`.

---

## Background Rhythm

All styles alternate between two background tones for visual variety:

| Style | Slide 1 (Hero) | Slide 2 | Slide 3 | ... | Last (CTA) |
|-------|----------------|---------|---------|-----|------------|
| Bold Impact | dark-bg | dark-elevated | dark-bg | alternates | White textured |
| Tweet Post | dark-bg / sage | dark-bg / sage | same | same | White textured |
| Clean Editorial | light-bg | light-elevated | light-bg | alternates | White textured |
| Product Showcase | warm gradient | warm gradient | warm gradient | same | White textured |

The CTA slide always uses the same white textured design across all styles.
