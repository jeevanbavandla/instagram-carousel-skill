# Instagram Carousel Design Styles

Complete specifications for 4 carousel design styles. All styles share Poppins font, 420x525px fixed slide dimensions, and the shared components defined in `components.md`.

---

## Shared Foundations

All styles use these base constraints:

- **Slide dimensions:** 420x525px (fixed, no responsive)
- **Font:** Poppins (Google Fonts), all weights 300-800
- **Profile header:** Top-left on every slide (except CTA)
- **Arrow indicator:** Bottom-right on every slide (except CTA)
- **Handle watermark:** Bottom-left on every slide (except CTA)
- **Progress bar:** Bottom edge of every slide (including CTA)
- **CTA slide:** Shared white design across all styles (see end of document)

```css
/* Base slide container — all styles */
.slide {
  width: 420px;
  height: 525px;
  position: relative;
  overflow: hidden;
  font-family: 'Poppins', sans-serif;
  box-sizing: border-box;
}
```

---

## STYLE 1: BOLD IMPACT (Dark Premium)

**Default style.** Use when no style is specified.

### Use Cases

- Numbered insights (5 shifts, 7 mistakes, 10 tips)
- Bold statements and hot takes with supporting detail
- Listicle-style educational content
- Any content with numbered items or sequential points

### Design Principles

- MASSIVE typography filling the slide — text IS the design
- No glass cards, no containers, no boxes — text sits directly on the dark background
- Green accent for key words, numbers, and emphasis
- Film grain texture + gradient orbs for premium depth
- Alternating background shades between slides for visual rhythm

### Color Palette

| Token | Value | Usage |
|-------|-------|-------|
| `--bg-primary` | `#0c1729` | Primary navy background (odd slides) |
| `--bg-elevated` | `#101d32` | Elevated navy background (even slides) |
| `--accent` | `#6EE421` | Bright green — numbers, key words, emphasis |
| `--text-primary` | `#ffffff` | Main heading and body text |
| `--text-secondary` | `#8899aa` | Subtitle text, secondary info |

### Typography

```css
/* Bold Impact — Heading */
.bi-heading {
  font-family: 'Poppins', sans-serif;
  font-size: 44px;       /* Range: 44-56px depending on text length */
  font-weight: 700;
  line-height: 1.05;
  letter-spacing: -1px;
  color: #ffffff;
}

/* Bold Impact — Body */
.bi-body {
  font-family: 'Poppins', sans-serif;
  font-size: 18px;       /* Range: 18-20px */
  font-weight: 400;
  font-style: italic;
  line-height: 1.5;
  color: #ffffff;
}

/* Bold Impact — Accent number */
.bi-number {
  font-family: 'Poppins', sans-serif;
  font-size: 56px;
  font-weight: 800;
  color: #6EE421;
  line-height: 1;
}

/* Bold Impact — Accent inline keyword */
.bi-keyword {
  color: #6EE421;
  font-weight: 700;
  font-style: italic;
}
```

### Hero Slide (Slide 1)

- Profile header top-left
- MASSIVE heading, left-aligned, green words + white words mixed
- Large green number (e.g., "5") + white italic subtitle below it
- Arrow indicator bottom-right

```css
/* Hero slide layout */
.bi-hero {
  background: #0c1729;
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.bi-hero .bi-heading {
  font-size: 52px;       /* Larger on hero */
  margin-bottom: 24px;
  text-align: left;
}

.bi-hero .bi-heading .accent {
  color: #6EE421;
}

.bi-hero .bi-subtitle {
  font-size: 18px;
  font-weight: 400;
  font-style: italic;
  color: #ffffff;
  line-height: 1.5;
}
```

### Content Slides (Slides 2-N)

- Alternating backgrounds: `#0c1729` / `#101d32`
- Centered green number (`#1`, `#2`, etc.) at top
- MASSIVE centered heading (44-50px, white)
- Body text below (18px, white italic)
- Key phrases highlighted inline in GREEN BOLD ITALIC

```css
/* Content slide layout */
.bi-content {
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.bi-content .bi-number {
  font-size: 56px;
  font-weight: 800;
  color: #6EE421;
  margin-bottom: 16px;
  line-height: 1;
}

.bi-content .bi-heading {
  font-size: 44px;       /* 44-50px based on length */
  text-align: center;
  margin-bottom: 20px;
}

.bi-content .bi-body {
  font-size: 18px;
  text-align: center;
  max-width: 340px;
}

.bi-content .bi-body .bi-keyword {
  color: #6EE421;
  font-weight: 700;
  font-style: italic;
}
```

### Visual Effects

#### Film Grain

Applied as a `::before` pseudo-element on every Bold Impact slide. Creates a subtle analog/cinematic texture.

```css
/* Film grain — apply to every .bi-slide */
.bi-slide {
  position: relative;
}

.bi-slide::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  pointer-events: none;
  opacity: 0.035;
  background-size: 180px;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='180' height='180'%3E%3Cfilter id='g'%3E%3CfeTurbulence baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23g)'/%3E%3C/svg%3E");
}

/* All content inside the slide must be above the grain layer */
.bi-slide > *:not(::before) {
  position: relative;
  z-index: 2;
}
```

**Film grain parameters:**
- SVG filter: `feTurbulence`
- `baseFrequency`: `0.65`
- `numOctaves`: `3`
- `stitchTiles`: `stitch`
- Pseudo-element opacity: `0.035`
- `background-size`: `180px`

#### Gradient Orbs

Three decorative orbs that rotate across slides for visual variety. Only one or two orbs per slide — cycle through the three variants.

```css
/* Gradient orb — Variant A: top-right */
.bi-orb-a {
  position: absolute;
  top: -60px;
  right: -60px;
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(110, 228, 33, 0.16) 0%, transparent 70%);
  pointer-events: none;
  z-index: 0;
}

/* Gradient orb — Variant B: bottom-left */
.bi-orb-b {
  position: absolute;
  bottom: -50px;
  left: -50px;
  width: 250px;
  height: 250px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(110, 228, 33, 0.10) 0%, transparent 70%);
  pointer-events: none;
  z-index: 0;
}

/* Gradient orb — Variant C: center */
.bi-orb-c {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 350px;
  height: 350px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(110, 228, 33, 0.08) 0%, transparent 70%);
  pointer-events: none;
  z-index: 0;
}
```

**Orb rotation pattern across slides:**
- Slide 1 (hero): Orb A (top-right)
- Slide 2: Orb B (bottom-left)
- Slide 3: Orb C (center)
- Slide 4: Orb A (top-right) + Orb B (bottom-left)
- Slide 5+: Continue cycling

**Layering order (bottom to top):**
1. Background color (`z-index: 0`)
2. Gradient orbs (`z-index: 0`)
3. Film grain pseudo-element (`z-index: 1`)
4. All content (text, numbers, header) (`z-index: 2`)

---

## STYLE 2: TWEET POST (Minimal Text-Only)

### Use Cases

- Hot takes and opinions
- Quotes and wisdom
- Conversational statements
- Personal reflections and stories
- Anything that reads like a tweet or casual thought

### Design Principles

- Ultra clean — ZERO visual effects (no grain, no orbs, no glass, no shadows)
- Conversational tone reflected in the design
- Minimal decoration — text and spacing do all the work
- Bold/underline emphasis instead of color-based highlighting

### Variants

Two color variants. Choose based on content mood.

#### Dark Variant

| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#0c1729` | Background |
| `--text` | `#ffffff` | Primary text |
| `--accent` | `#6EE421` | Accent (numbers, emphasis) |
| `--divider` | `rgba(255, 255, 255, 0.4)` | Divider line and diamonds |

#### Sage Variant

| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#7d9e92` | Teal/sage background |
| `--text` | `#f5f0eb` | Cream text |
| `--accent` | `#f0b0a0` | Coral accent |
| `--divider` | `rgba(245, 240, 235, 0.4)` | Divider line and diamonds |

### Typography

```css
/* Tweet Post — Heading */
.tp-heading {
  font-family: 'Poppins', sans-serif;
  font-size: 36px;       /* Range: 36-44px depending on text length */
  font-weight: 700;
  line-height: 1.15;
  color: var(--text);
}

/* Tweet Post — Body */
.tp-body {
  font-family: 'Poppins', sans-serif;
  font-size: 18px;       /* Range: 18-20px */
  font-weight: 400;
  line-height: 1.6;
  color: var(--text);
}

/* Tweet Post — Bold emphasis (NOT color-based — uses weight + underline) */
.tp-emphasis {
  font-weight: 700;
  text-decoration: underline;
  text-decoration-thickness: 2px;
  text-underline-offset: 3px;
}
```

### Hero Slide

- Profile header top-left
- Optional large circular photo (centered, ~120px diameter)
- Large heading text
- CTA pill/button at bottom (optional)

```css
/* Tweet Post hero slide */
.tp-hero {
  background: var(--bg);
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.tp-hero .tp-photo {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 24px;
}

.tp-hero .tp-heading {
  font-size: 40px;
  margin-bottom: 16px;
}

/* Optional CTA pill on hero */
.tp-hero .tp-pill {
  display: inline-block;
  padding: 12px 28px;
  border: 2px solid var(--accent);
  border-radius: 100px;
  color: var(--accent);
  font-family: 'Poppins', sans-serif;
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 0.5px;
  margin-top: 20px;
}
```

### Content Slides

- Number + heading at top
- Body text with bold/underlined emphasis
- Clean, no visual effects

```css
/* Tweet Post content slide */
.tp-content {
  background: var(--bg);
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.tp-content .tp-number {
  font-family: 'Poppins', sans-serif;
  font-size: 48px;
  font-weight: 800;
  color: var(--accent);
  line-height: 1;
  margin-bottom: 12px;
}

.tp-content .tp-heading {
  font-size: 36px;
  margin-bottom: 16px;
}

.tp-content .tp-body {
  font-size: 18px;
}

.tp-content .tp-body .tp-emphasis {
  font-weight: 700;
  text-decoration: underline;
  text-decoration-thickness: 2px;
  text-underline-offset: 3px;
}
```

### Diamond Divider

Unique decorative element for Tweet Post style. Thin horizontal line with three diamond shapes centered.

```css
/* Diamond divider — unique to Tweet Post */
.tp-divider {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  margin: 24px 0;
  opacity: 0.4;
}

.tp-divider::before,
.tp-divider::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--divider);
}

.tp-divider .tp-diamonds {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 0 12px;
  font-size: 10px;
  color: var(--divider);
  letter-spacing: 4px;
}

/* Usage in HTML:
   <div class="tp-divider">
     <span class="tp-diamonds">&#9671; &#9671; &#9671;</span>
   </div>
   
   Renders as: ────── ◇ ◇ ◇ ──────
*/
```

**Diamond divider specs:**
- Line: 1px solid, uses `--divider` color
- Diamonds: 3x `◇` (Unicode `&#9671;`) characters
- Spacing: 6px gap between diamonds, 12px padding from lines
- Overall opacity: 0.4
- Placed between heading and body, or at bottom of content area

---

## STYLE 3: CLEAN EDITORIAL (Light/White)

### Use Cases

- Frameworks and methodologies
- Tutorials and step-by-step guides
- Case studies with data
- Listicles and checklists
- Educational infographics with diagrams

### Design Principles

- White/light background — clean, professional, educational feel
- Subtle grid pattern overlay for structure
- Coral accent for labels, pills, and highlights
- Structured content with clear visual hierarchy
- CSS-only diagram components (no images needed)

### Color Palette

| Token | Value | Usage |
|-------|-------|-------|
| `--bg-white` | `#ffffff` | Primary white background (odd slides) |
| `--bg-cream` | `#f8f8f6` | Slight warm white (even slides, alternating) |
| `--accent` | `#e8553d` | Coral — labels, pills, checkmarks, highlights |
| `--heading` | `#0c1729` | Dark navy heading text |
| `--body` | `#4a5568` | Gray body text |
| `--label` | `#e8553d` | Coral label text (same as accent) |
| `--grid` | `rgba(0, 0, 0, 0.03)` | Grid pattern lines |

### Typography

```css
/* Clean Editorial — Heading */
.ce-heading {
  font-family: 'Poppins', sans-serif;
  font-size: 36px;       /* Range: 36-44px */
  font-weight: 700;
  line-height: 1.15;
  color: #0c1729;
}

/* Clean Editorial — Body */
.ce-body {
  font-family: 'Poppins', sans-serif;
  font-size: 16px;       /* Range: 16-18px */
  font-weight: 400;
  line-height: 1.6;
  color: #4a5568;
}

/* Clean Editorial — Label */
.ce-label {
  font-family: 'Poppins', sans-serif;
  font-size: 16px;
  font-weight: 600;
  color: #e8553d;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}
```

### Grid Pattern Overlay

Applied as a `::before` pseudo-element on every Clean Editorial slide. Creates a subtle graph-paper feel.

```css
/* Grid pattern — apply to every .ce-slide */
.ce-slide {
  position: relative;
}

.ce-slide::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  pointer-events: none;
  background-image:
    linear-gradient(rgba(0, 0, 0, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0, 0, 0, 0.03) 1px, transparent 1px);
  background-size: 40px 40px;
}

/* All content must be above grid */
.ce-slide > *:not(::before) {
  position: relative;
  z-index: 2;
}
```

**Grid pattern parameters:**
- Grid cell size: 40x40px
- Line color: `rgba(0, 0, 0, 0.03)` (3% opacity black)
- Line width: 1px
- Applied via two `linear-gradient` layers (horizontal + vertical)

### Slide Types

#### Hero Slide

```css
.ce-hero {
  background: #ffffff;
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.ce-hero .ce-label {
  margin-bottom: 12px;
}

.ce-hero .ce-heading {
  font-size: 42px;
  margin-bottom: 16px;
}

.ce-hero .ce-tagline {
  font-size: 18px;
  color: #4a5568;
  font-weight: 400;
  line-height: 1.5;
}
```

#### Problem / Solution Slide

Coral label at top, description text, and a coral pill summary at bottom.

```css
.ce-problem-solution {
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.ce-problem-solution .ce-label {
  font-size: 14px;
  margin-bottom: 16px;
}

.ce-problem-solution .ce-heading {
  font-size: 36px;
  margin-bottom: 12px;
}

.ce-problem-solution .ce-body {
  margin-bottom: 24px;
}

/* Coral pill summary */
.ce-pill {
  display: inline-block;
  padding: 14px 24px;
  background: #e8553d;
  color: #ffffff;
  font-family: 'Poppins', sans-serif;
  font-size: 15px;
  font-weight: 600;
  border-radius: 100px;
  text-align: center;
}
```

#### Checklist Slide

Items with coral checkmarks.

```css
.ce-checklist {
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.ce-checklist .ce-heading {
  font-size: 36px;
  margin-bottom: 24px;
}

.ce-checklist-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  margin-bottom: 16px;
}

.ce-checklist-item .ce-check {
  flex-shrink: 0;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: #e8553d;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  font-size: 13px;
  font-weight: 700;
  margin-top: 2px;
}

.ce-checklist-item .ce-body {
  font-size: 17px;
  line-height: 1.5;
}
```

### CSS-Only Diagram Components

All diagrams are built with pure CSS -- no images, no SVGs (except optional decorative elements).

#### Venn Diagram

Two overlapping circles showing intersection of concepts.

```css
/* Venn diagram container */
.ce-venn {
  position: relative;
  width: 280px;
  height: 180px;
  margin: 24px auto;
}

.ce-venn-circle {
  position: absolute;
  width: 160px;
  height: 160px;
  border-radius: 50%;
  top: 10px;
}

/* Left circle — coral */
.ce-venn-circle.left {
  left: 20px;
  background: rgba(232, 85, 61, 0.3);
  border: 2px solid rgba(232, 85, 61, 0.5);
}

/* Right circle — green */
.ce-venn-circle.right {
  right: 20px;
  background: rgba(110, 228, 33, 0.3);
  border: 2px solid rgba(110, 228, 33, 0.5);
}

/* Labels */
.ce-venn-label {
  position: absolute;
  font-family: 'Poppins', sans-serif;
  font-size: 12px;
  font-weight: 600;
  color: #0c1729;
  text-align: center;
}

.ce-venn-label.left {
  top: 50%;
  left: 30px;
  transform: translateY(-50%);
  width: 70px;
}

.ce-venn-label.right {
  top: 50%;
  right: 30px;
  transform: translateY(-50%);
  width: 70px;
}

.ce-venn-label.center {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60px;
}

/* Usage:
   <div class="ce-venn">
     <div class="ce-venn-circle left"></div>
     <div class="ce-venn-circle right"></div>
     <span class="ce-venn-label left">Strategy</span>
     <span class="ce-venn-label center">Growth</span>
     <span class="ce-venn-label right">Creative</span>
   </div>
*/
```

#### Bar Chart

Horizontal or vertical bars for data comparison.

```css
/* Bar chart — vertical bars */
.ce-bar-chart {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  gap: 16px;
  height: 160px;
  margin: 24px auto;
  padding: 0 20px;
}

.ce-bar-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.ce-bar {
  width: 48px;
  background: #e8553d;
  border-radius: 6px 6px 0 0;
  /* Height set inline: style="height: 80px" etc. */
}

/* Alternate bar color */
.ce-bar.alt {
  background: rgba(232, 85, 61, 0.4);
}

.ce-bar-label {
  font-family: 'Poppins', sans-serif;
  font-size: 11px;
  font-weight: 600;
  color: #4a5568;
  text-align: center;
}

.ce-bar-value {
  font-family: 'Poppins', sans-serif;
  font-size: 13px;
  font-weight: 700;
  color: #0c1729;
}

/* Usage:
   <div class="ce-bar-chart">
     <div class="ce-bar-group">
       <span class="ce-bar-value">85%</span>
       <div class="ce-bar" style="height: 120px"></div>
       <span class="ce-bar-label">Meta</span>
     </div>
     <div class="ce-bar-group">
       <span class="ce-bar-value">62%</span>
       <div class="ce-bar alt" style="height: 88px"></div>
       <span class="ce-bar-label">Google</span>
     </div>
   </div>
*/
```

#### Timeline

Vertical timeline with left border and circle markers.

```css
/* Timeline */
.ce-timeline {
  position: relative;
  padding-left: 28px;
  margin: 20px 32px;
}

/* Vertical line */
.ce-timeline::before {
  content: '';
  position: absolute;
  left: 8px;
  top: 0;
  bottom: 0;
  width: 2px;
  background: #e8553d;
}

.ce-timeline-item {
  position: relative;
  margin-bottom: 20px;
  padding-left: 16px;
}

/* Circle marker on the line */
.ce-timeline-item::before {
  content: '';
  position: absolute;
  left: -24px;
  top: 6px;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: #e8553d;
  border: 3px solid #ffffff;
}

.ce-timeline-item .ce-label {
  font-size: 12px;
  margin-bottom: 4px;
}

.ce-timeline-item .ce-body {
  font-size: 15px;
  line-height: 1.4;
}

/* Usage:
   <div class="ce-timeline">
     <div class="ce-timeline-item">
       <div class="ce-label">STEP 1</div>
       <div class="ce-body">Audit current campaigns</div>
     </div>
     <div class="ce-timeline-item">
       <div class="ce-label">STEP 2</div>
       <div class="ce-body">Restructure ad groups</div>
     </div>
   </div>
*/
```

---

## STYLE 4: PRODUCT SHOWCASE (Soft Gradient)

### Use Cases

- Tool reviews and recommendations
- Product comparisons (side-by-side)
- Curated category lists (top 5 tools for X)
- Software roundups and stacks

### Design Principles

- Soft warm gradient background — approachable, lifestyle feel
- Centered refined layout with generous whitespace
- App icon cards as primary visual element
- Pagination dots for category navigation
- Subtle dot pattern overlay for texture

### Color Palette

| Token | Value | Usage |
|-------|-------|-------|
| `--bg-gradient-top` | `#f5efe6` | Gradient start (top) |
| `--bg-gradient-bottom` | `#ede4d8` | Gradient end (bottom) |
| `--heading` | `#2d2a26` | Dark brown heading |
| `--body` | `#5a5651` | Medium brown body text |
| `--number` | `#a09890` | Muted gray category number |
| `--dot-standard` | `#c4bdb4` | Inactive pagination dot |
| `--wordmark` | `#a09890` | Muted tan bottom wordmark |

#### Category Accent Colors

Each category/section gets its own accent color. Rotate through these:

| Accent | Value | Usage |
|--------|-------|-------|
| Teal | `#7d9e92` | First category accent |
| Brown | `#8b6f5c` | Second category accent |
| Coral | `#c4665a` | Third category accent |

The active accent is used for: pagination dot active state, app icon card border highlight (if needed), and category label color.

### Typography

```css
/* Product Showcase — Heading */
.ps-heading {
  font-family: 'Poppins', sans-serif;
  font-size: 40px;       /* Range: 40-52px */
  font-weight: 700;
  line-height: 1.1;
  color: #2d2a26;
  text-align: center;
}

/* Product Showcase — Body */
.ps-body {
  font-family: 'Poppins', sans-serif;
  font-size: 16px;       /* Range: 16-18px */
  font-weight: 400;
  line-height: 1.5;
  color: #5a5651;
  text-align: center;
}

/* Product Showcase — Category number */
.ps-category-number {
  font-family: 'Poppins', sans-serif;
  font-size: 20px;
  font-weight: 600;
  color: #a09890;
}
```

### Dot Pattern Overlay

Applied as a `::before` pseudo-element on every Product Showcase slide.

```css
/* Dot pattern — apply to every .ps-slide */
.ps-slide {
  position: relative;
  background: linear-gradient(180deg, #f5efe6 0%, #ede4d8 100%);
}

.ps-slide::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  pointer-events: none;
  background-image: radial-gradient(circle, rgba(0, 0, 0, 0.06) 1px, transparent 1px);
  background-size: 24px 24px;
}

/* All content above dot pattern */
.ps-slide > *:not(::before) {
  position: relative;
  z-index: 2;
}
```

**Dot pattern parameters:**
- Dot color: `rgba(0, 0, 0, 0.06)` (6% opacity black)
- Dot size: 1px radius
- Grid interval: 24x24px
- Applied via `radial-gradient`

### App Icon Cards

Primary visual element for showcasing tools and products.

```css
/* App icon card */
.ps-icon-card {
  width: 52px;
  height: 52px;
  background: #ffffff;
  border-radius: 12px;
  box-shadow:
    0 2px 8px rgba(0, 0, 0, 0.06),
    0 0 0 1px rgba(0, 0, 0, 0.04);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;        /* For emoji icons */
  flex-shrink: 0;
}

/* Icon card in a row with label */
.ps-tool-row {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 14px;
}

.ps-tool-name {
  font-family: 'Poppins', sans-serif;
  font-size: 16px;
  font-weight: 600;
  color: #2d2a26;
}

.ps-tool-desc {
  font-family: 'Poppins', sans-serif;
  font-size: 13px;
  font-weight: 400;
  color: #5a5651;
  margin-top: 2px;
}

/* Grid layout for multiple icon cards */
.ps-icon-grid {
  display: grid;
  grid-template-columns: repeat(3, 52px);
  gap: 16px;
  justify-content: center;
  margin: 20px 0;
}

/* Usage:
   <!-- Row layout -->
   <div class="ps-tool-row">
     <div class="ps-icon-card">📊</div>
     <div>
       <div class="ps-tool-name">Google Analytics</div>
       <div class="ps-tool-desc">Web analytics platform</div>
     </div>
   </div>

   <!-- Grid layout -->
   <div class="ps-icon-grid">
     <div class="ps-icon-card">📊</div>
     <div class="ps-icon-card">📈</div>
     <div class="ps-icon-card">🎯</div>
   </div>
*/
```

### Pagination Dots

Visual indicator showing which category is active.

```css
/* Pagination dots container */
.ps-pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin: 20px 0;
}

/* Standard (inactive) dot */
.ps-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #c4bdb4;
}

/* Active dot — color matches current category accent */
.ps-dot.active {
  width: 10px;
  height: 10px;
  /* Background set to category accent color inline or via modifier class */
}

/* Category-specific active dot colors */
.ps-dot.active.teal { background: #7d9e92; }
.ps-dot.active.brown { background: #8b6f5c; }
.ps-dot.active.coral { background: #c4665a; }

/* Usage:
   <div class="ps-pagination">
     <div class="ps-dot active teal"></div>
     <div class="ps-dot"></div>
     <div class="ps-dot"></div>
   </div>
*/
```

### Wordmark

Bottom-center branding element, unique to Product Showcase.

```css
/* Wordmark — bottom center */
.ps-wordmark {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  font-family: 'Poppins', sans-serif;
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 2px;
  color: #a09890;
  text-transform: uppercase;
  z-index: 2;
}

/* Usage:
   <div class="ps-wordmark">JEEVAN BAVANDLA</div>
*/
```

### Slide Layout

```css
/* Product Showcase — Hero slide */
.ps-hero {
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.ps-hero .ps-heading {
  font-size: 48px;
  margin-bottom: 12px;
}

.ps-hero .ps-body {
  margin-bottom: 20px;
}

/* Product Showcase — Content slide */
.ps-content {
  padding: 60px 32px 48px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ps-content .ps-category-number {
  margin-bottom: 8px;
}

.ps-content .ps-heading {
  font-size: 40px;
  margin-bottom: 20px;
}
```

---

## SHARED CTA SLIDE (All Styles)

The final slide in every carousel is a white CTA slide. This design is identical regardless of which style the rest of the carousel uses.

### Layout

- White background (`#ffffff`)
- Large circular profile photo centered
- Name heading below photo
- Handle / tagline below name
- "Follow for more" or custom CTA text
- Progress bar at bottom (final position filled)

### CSS

```css
/* CTA slide — shared across all styles */
.cta-slide {
  width: 420px;
  height: 525px;
  background: #ffffff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  position: relative;
  font-family: 'Poppins', sans-serif;
}

.cta-photo {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 20px;
}

.cta-name {
  font-size: 28px;
  font-weight: 700;
  color: #0c1729;
  margin-bottom: 6px;
}

.cta-handle {
  font-size: 15px;
  font-weight: 400;
  color: #8899aa;
  margin-bottom: 24px;
}

.cta-button {
  display: inline-block;
  padding: 14px 36px;
  background: #0c1729;
  color: #ffffff;
  font-family: 'Poppins', sans-serif;
  font-size: 16px;
  font-weight: 600;
  border-radius: 100px;
  border: none;
  cursor: pointer;
}

/* No arrow indicator on CTA slide */
/* No handle watermark on CTA slide */
/* Progress bar IS present on CTA slide (final dot filled) */
```

---

## Quick Reference: Style Selection Guide

| Content Type | Recommended Style |
|---|---|
| Numbered tips/mistakes/shifts | Bold Impact |
| Hot takes, opinions, quotes | Tweet Post (Dark) |
| Reflective, calm wisdom | Tweet Post (Sage) |
| Frameworks, tutorials, how-tos | Clean Editorial |
| Checklists, step-by-step | Clean Editorial |
| Tool reviews, product lists | Product Showcase |
| Software comparisons | Product Showcase |
| Bold statements with detail | Bold Impact |
| Case studies with data | Clean Editorial |

When no style is specified, default to **Bold Impact**.
