---
name: instagram-carousel
description: "Design Instagram carousel posts as self-contained HTML files in the user's own brand. Runs a one-time brand setup on first use (name, handle, subtitle, photo, accent color, style preference). 4 design styles: Bold Impact (dark premium), Tweet Post (minimal text), Clean Editorial (light/white), Product Showcase (soft gradient). Fixed 420x525px slides, Poppins font, progress bar, arrow indicators, white CTA slide, PNG + PDF export. Triggers: instagram carousel, carousel design, ig carousel, create carousel, design carousel, carousel post, make a carousel, instagram post design, carousel html, slide deck for instagram, visual carousel"
---

# Instagram Carousel Designer

Generate Instagram carousel posts as self-contained HTML files in **your brand**. Each carousel is a single HTML file with fixed-dimension slides (420x525px) that export as pixel-perfect 1080x1350px PNGs via Playwright.

This skill handles **both copy and design**. Give it a topic, pick a style, and it writes the slides + builds the visual — using your colors, your photo, your name.

---

## Architecture

### Fixed Dimensions

Every slide is a **fixed 420x525px box**. Not responsive. Fixed pixel dimensions.

```
Design:  420 x 525px  (what you see in browser)
Export:  1080 x 1350px (what gets uploaded to Instagram)
Scale:   2.5714x (via Playwright device_scale_factor)
Ratio:   4:5 (Instagram carousel standard)
```

### Modular Reference Files

| File | Contains |
|------|----------|
| `references/design-system.md` | Dimensions, typography, color tokens, slide architecture |
| `references/components.md` | Profile header, progress bar, arrow, watermark, CTA slide, IG frame |
| `references/styles.md` | All 4 visual styles with complete CSS |
| `references/export-guide.md` | Playwright export script, critical rules, common mistakes |

---

## 4 Design Styles

| Style | Name | Best For | Vibe |
|-------|------|----------|------|
| 1 | **Bold Impact** | Numbered insights, shifts, mistakes, tips | Dark, massive text, accent color, film grain, premium |
| 2 | **Tweet Post** | Hot takes, opinions, quotes, wisdom | Minimal, clean, zero effects, conversational |
| 3 | **Clean Editorial** | Frameworks, tutorials, case studies, listicles | White bg, grid overlay, coral accent, educational |
| 4 | **Product Showcase** | Tool reviews, comparisons, curated lists | Soft warm gradient, app cards, pagination dots |

### Style Selection Logic

1. **Explicit request** — user says "bold impact", "tweet style", "editorial", "product showcase"
2. **Auto-detection by content:**
   - Hot take / opinion / conversational / quote → **Tweet Post**
   - List of tools / product comparison / category breakdown → **Product Showcase**
   - Framework / tutorial / educational / how-to / case study → **Clean Editorial**
   - Everything else (numbered insights, tips, shifts, mistakes) → **Bold Impact** (default)
3. **If ambiguous** — ask 1 question max, default to Bold Impact

---

## Workflow

Follow these phases in order.

### Phase 0: Brand Setup (First Use Only)

**Check if `carousels/brand-config.json` exists** in the current working directory.

- **If it exists:** Read it and use those values for all brand elements. Skip to Phase 1.
- **If it does NOT exist:** Run the brand onboarding below before doing anything else.

#### Brand Onboarding Interview

Ask the user these 6 questions (can be asked all at once):

```
Before we build your first carousel, I need to set up your brand. This only runs once.

1. What's your full name? (shown on every slide as the profile name)
2. What's your @handle? (watermark on every slide, e.g. @yourname)
3. What's your subtitle or tagline? (e.g. "Paid Media | Growth Marketing")
4. Path to your profile photo? (e.g. carousels/photo.jpg — or type "skip" to use initials)
5. What's your brand accent color? (paste a hex code like #6EE421, or describe: green / coral / blue / purple / orange)
6. Default style preference? (dark / light / minimal — or type "auto" to let me decide per content)
```

#### Saving Brand Config

After collecting answers, save `carousels/brand-config.json`:

```json
{
  "name": "User's Full Name",
  "handle": "@userhandle",
  "subtitle": "Their Tagline Here",
  "profile_photo": "carousels/photo.jpg",
  "accent_color": "#HEXCODE",
  "initials": "XX",
  "default_style": "dark",
  "setup_complete": true
}
```

**Accent color mapping (if user describes instead of hex):**
| Description | Hex |
|-------------|-----|
| green | `#6EE421` |
| coral / orange-red | `#e8553d` |
| blue | `#3b82f6` |
| purple | `#8b5cf6` |
| orange | `#f97316` |
| pink | `#ec4899` |
| teal | `#14b8a6` |
| yellow | `#eab308` |

**Initials:** Extract first letter of first name + first letter of last name. If only one name, use first two letters.

**Profile photo fallback:** If user skips or file doesn't exist, use a circle with their initials in the accent color.

#### Create the `carousels/` folder

Also create `carousels/[year]/[month]/` structure if it doesn't exist.

---

### Phase 1: Research

1. Check `carousels/` for any past carousels — scan titles/filenames to avoid repeating topics
2. Determine style (see selection logic above)

### Phase 2: Write Slide Copy

Follow these copy rules:

**Voice:**
- Direct and opinionated — clear stances, not wishy-washy
- Experience-backed — real examples, real specifics
- Anti-fluff — no corporate speak, no filler words
- Punchy — fragments fine, contrast wins

**Banned words:** "just", "really", "actually", "basically", "simply", "literally"

**Hook formulas (Slide 1):**
- Contrarian: "Your [strategy] is backwards."
- Number promise: "5 [topic] mistakes burning your [resource]."
- Challenge: "I bet you're still doing this in [year]."
- Bold statement: "[Thing] didn't kill [people]. [Behaviour] killed [people]."

**Slide count:** 5-10 slides (7 is the sweet spot). Every slide readable in 3-4 seconds.

### Phase 3: Build HTML

Generate a single self-contained HTML file using brand config values. Read all reference files:
- `references/design-system.md` — dimensions, fonts, colors
- `references/components.md` — shared components
- `references/styles.md` — chosen style's CSS and layout

**Critical build rules:**
1. Every slide: `width: 420px; height: 525px;` — FIXED, never responsive
2. Use `brand.accent_color` for all accent/highlight elements
3. Use `brand.name`, `brand.handle`, `brand.subtitle` in profile header and watermark
4. Profile photo: if `brand.profile_photo` file exists → base64 encode and embed; else → initials circle using `brand.initials` and `brand.accent_color`
5. All images: base64 encoded inline (self-contained HTML)
6. **Always use Python to generate HTML** — never shell/bash (corrupts special characters)
7. IG preview frame wraps carousel for in-browser preview
8. Navigation: touch swipe + mouse drag + keyboard arrows
9. Export buttons: PNG per slide + PDF (all slides)
10. Save to: `carousels/[year]/[month]/carousel-XX-topic.html`
11. **MANDATORY — accent color sanitisation:** The reference files contain example hex values (`#6EE421`, `#85F040`, `#5BC01C`) from a sample brand. After generating the full HTML string, run a global replace in Python before writing the file:

```python
# Replace all sample accent colors with the user's brand color
SAMPLE_ACCENTS = ["#6EE421", "#85F040", "#5BC01C", "#6ee421", "#85f040", "#5bc01c"]
for sample in SAMPLE_ACCENTS:
    html = html.replace(sample, brand["accent_color"])
```

This guarantees no one else's carousel ever shows your design's green.

### Phase 4: Preview and Iterate

Show the carousel. Present a slide-by-slide summary. Ask if any slides need changes.

### Phase 5: Export

**Primary (Playwright — pixel-perfect):**
Run the Python export script from `references/export-guide.md`. Produces 1080x1350px PNG per slide.

**Fallback (Browser):**
HTML file includes Export buttons using html2canvas + jsPDF.

### Phase 6: Save and Suggest

1. Save to `carousels/[year]/[month]/carousel-XX-topic.html`
2. Suggest an Instagram caption with relevant hashtags
3. Present file path and next steps

---

## Quality Checklist

Before presenting the carousel, verify:

- [ ] Brand config was read (not hardcoded values)
- [ ] Poppins font loads via Google Fonts CDN
- [ ] Every slide is exactly 420x525px — no responsive widths
- [ ] Slide count is 5-10 (ideally 7)
- [ ] Every slide readable in 3-4 seconds
- [ ] Hero slide has a scroll-stopping hook
- [ ] Profile header on every slide (photo or initials fallback)
- [ ] Progress bar on every slide
- [ ] Arrow indicator on every slide EXCEPT CTA
- [ ] Handle watermark on every slide
- [ ] CTA slide uses white/textured design
- [ ] Accent color used correctly throughout (not hardcoded green)
- [ ] Text sized correctly for style
- [ ] Visual effects applied correctly per style
- [ ] Navigation works (touch / mouse / keyboard)
- [ ] IG preview frame wraps carousel at exactly 420px
- [ ] PNG + PDF export buttons present and functional
- [ ] File saved to correct path
- [ ] Instagram caption suggested
