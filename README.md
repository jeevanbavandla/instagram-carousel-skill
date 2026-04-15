<div align="center">

# Instagram Carousel Skill

**Generate pixel-perfect Instagram carousels in your brand — just by asking Claude**

[![License: MIT](https://img.shields.io/badge/license-MIT-22c55e?style=flat-square)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-5B8DEF?style=flat-square)](https://claude.ai/code)
[![Claude.ai](https://img.shields.io/badge/Claude.ai-compatible-f97316?style=flat-square)](claude-ai/)
[![Free & Open Source](https://img.shields.io/badge/free-open%20source-22c55e?style=flat-square)](LICENSE)

<img src="assets/preview.gif" alt="Instagram Carousel Skill Preview" width="480" />

**Works with Claude Code (CLI) and Claude.ai (web browser)**<br>
No design tools. No templates. No Canva. Your brand, auto-applied on every slide.

</div>

---

## Install in 30 Seconds

```bash
cp -r instagram-carousel/ ~/.claude/skills/
```

Open Claude Code → ask for a carousel → done. Brand setup runs once, never again.

> **No terminal?** Use the [Claude.ai version](claude-ai/README.md) — paste one file into a Claude Project and you're ready.

---

## What It Does

You describe a topic. Claude writes the copy, picks the right design, and builds a complete HTML file with your name, handle, photo, and brand color on every slide — ready to export as 1080×1350px PNGs for Instagram.

```text
"Create a carousel about 5 Meta Ads mistakes"
```

That's it. No prompting, no iteration, no design work.

---

## 4 Design Styles

Auto-detected based on your content. Override any time.

| Style | Best For | Look |
| ----- | -------- | ---- |
| **Bold Impact** | Numbered tips, mistakes, shifts | Dark navy · massive text · film grain · premium |
| **Tweet Post** | Hot takes, opinions, quotes | Ultra minimal · zero effects · conversational |
| **Clean Editorial** | Frameworks, tutorials, case studies | White bg · grid overlay · coral accent |
| **Product Showcase** | Tool lists, comparisons, stacks | Warm gradient · app icon cards · soft |

Every carousel includes:

- Fixed 420×525px slides → exports at **1080×1350px** (Instagram 4:5 ratio)
- **Profile header** on every slide (name, handle, photo or initials)
- **Progress bar** + **arrow indicator** + **handle watermark** on every slide
- **White CTA slide** — consistent across all styles
- **Touch / mouse / keyboard** navigation in the browser preview
- **Export as PNG** (one per slide) or **PDF** (all slides)

---

## Which Version Is Right for You?

| | [Claude Code](instagram-carousel/) | [Claude.ai](claude-ai/) |
| --- | --- | --- |
| **Who it's for** | Technical users, developers | Anyone — no coding needed |
| **Setup** | `cp` command · 30 seconds | Paste a file · 5 minutes |
| **Export quality** | Pixel-perfect via Playwright | Browser PNG/PDF export |
| **Brand config** | Saved in `brand-config.json` | Stored in Project Instructions |
| **Get started** | [→ Claude Code setup](#claude-code-setup) | [→ Claude.ai setup](claude-ai/README.md) |

---

## Claude Code Setup

### Requirements

- [Claude Code](https://claude.ai/code) — free CLI by Anthropic
- Python 3
- Optional: `playwright` for Playwright-quality PNG export

### Install

```bash
cp -r instagram-carousel/ ~/.claude/skills/
```

Or drop into your project's `.claude/skills/` folder instead. Claude auto-detects it.

### Brand Setup (First Time Only)

The first time you ask for a carousel, Claude runs a one-time onboarding:

```text
1. Your full name
2. Your @handle
3. Your subtitle / tagline
4. Path to your profile photo  (or skip — uses your initials)
5. Your brand accent color  (hex code or describe it: "green", "coral", "blue"...)
6. Default style preference  (dark / light / minimal / auto)
```

Saved to `carousels/brand-config.json`. Never asked again.

### Usage

```text
"Create a carousel about 5 Google Ads mistakes"
"Make a tweet-style carousel on AI replacing media buyers"
"Build an editorial carousel explaining the AIDA framework"
"Showcase my top 5 AI tools for performance marketers"
```

### Export

**In-browser (quick):** Click "Export All (PNG)" or "Export as PDF" in the carousel preview.

**Playwright (pixel-perfect):**

```bash
pip install playwright
playwright install chromium
python export_carousel.py /path/to/carousel.html
```

Full export script in [instagram-carousel/references/export-guide.md](instagram-carousel/references/export-guide.md).

---

## Claude.ai Setup

**→ Full guide: [claude-ai/README.md](claude-ai/README.md)**

Short version:

1. Open [claude-ai/skill.md](claude-ai/skill.md) in any text editor
2. Fill in your brand details at the top (name, handle, color)
3. Create a Claude.ai Project → paste the file into Project Instructions
4. Ask for carousels → copy the HTML → open in Chrome → export PNG

---

## File Structure

```text
instagram-carousel-skill/
│
├── assets/
│   └── preview.gif                  ← Demo
│
├── instagram-carousel/              ← Claude Code skill
│   ├── SKILL.md                     ← Auto-loaded by Claude Code
│   └── references/
│       ├── design-system.md         ← Dimensions, fonts, color tokens
│       ├── components.md            ← Profile header, progress bar, CTA slide
│       ├── styles.md                ← All 4 style specs with complete CSS
│       └── export-guide.md          ← Playwright PNG export script
│
└── claude-ai/                       ← Claude.ai version
    ├── skill.md                     ← Self-contained — paste into Project Instructions
    └── README.md                    ← Non-technical setup guide
```

---

## Made By

**Jeevan Bavandla** — performance marketer and agency co-founder building AI workflows for marketing teams.

[![Instagram](https://img.shields.io/badge/@jeevanbavandla-E4405F?style=flat-square&logo=instagram&logoColor=white)](https://www.instagram.com/jeevanbavandla)
[![LinkedIn](https://img.shields.io/badge/jeevanbavandla-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jeevanbavandla)

If this saved you time, share it with someone who's still making carousels the slow way.
