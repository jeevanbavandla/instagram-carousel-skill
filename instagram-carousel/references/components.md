# Reusable Components

All components are shared across every carousel style. Brand values come from `brand-config.json`. Colors adapt based on whether the slide background is light or dark.

---

## Brand Config Reference

All components use these variables from `brand-config.json`:

| Variable | Example | Used in |
|----------|---------|---------|
| `brand.name` | "Alex Johnson" | Profile header name |
| `brand.handle` | "@alexjohnson" | Handle watermark |
| `brand.subtitle` | "Growth Marketing | SaaS" | Profile header subtitle |
| `brand.accent_color` | "#6EE421" | Profile border, progress fill, CTA elements |
| `brand.profile_photo` | "carousels/photo.jpg" | Profile photo (base64 encoded) |
| `brand.initials` | "AJ" | Fallback if no photo |

---

## Profile Header (Every Slide)

Top-left of every slide. Shows brand identity.

```html
<div class="profile-header">
  <div class="profile-photo">
    <!-- If brand.profile_photo exists: base64 img -->
    <!-- Else: initials circle using brand.initials + brand.accent_color -->
  </div>
  <div class="profile-info">
    <div class="profile-name">{{ brand.name | uppercase }}</div>
    <div class="profile-subtitle">{{ brand.subtitle | uppercase }}</div>
  </div>
</div>
```

### Profile Photo Logic

1. Check if `brand.profile_photo` path exists on disk
2. If yes: read file → base64 encode → embed as `<img src="data:image/jpeg;base64,..." style="width:100%;height:100%;object-fit:cover;">`
3. If no: use initials fallback — circle using `brand.accent_color` background with `brand.initials` text in dark color

```python
# Python: profile photo or initials fallback
import base64, os

def get_profile_element(brand):
    photo_path = brand.get("profile_photo", "")
    if photo_path and os.path.exists(photo_path):
        with open(photo_path, "rb") as f:
            b64 = base64.b64encode(f.read()).decode("utf-8")
        # Detect format
        ext = photo_path.lower().split(".")[-1]
        mime = "image/jpeg" if ext in ("jpg","jpeg") else f"image/{ext}"
        return f'<img src="data:{mime};base64,{b64}" style="width:100%;height:100%;object-fit:cover;">'
    else:
        initials = brand.get("initials", "?")
        accent = brand.get("accent_color", "#6EE421")
        return f'<span style="font-size:13px;font-weight:700;color:#0c0c0c;">{initials}</span>'
```

### CSS

```css
.profile-header {
  position: absolute;
  top: 20px;
  left: 24px;
  z-index: 5;
  display: flex;
  align-items: center;
  gap: 10px;
}

.profile-photo {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid var(--accent);    /* uses brand accent color */
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--accent);          /* shows under initials fallback */
  flex-shrink: 0;
}

.profile-name {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 1.5px;
  text-transform: uppercase;
}

.profile-subtitle {
  font-size: 8px;
  font-weight: 500;
  letter-spacing: 1px;
  text-transform: uppercase;
  opacity: 0.6;
}
```

### Color Adaptation

| Background | .profile-name | .profile-subtitle |
|-----------|---------------|-------------------|
| Dark (navy) | `color: #ffffff` | `color: #ffffff` |
| Light (white) | `color: #0c1729` | `color: #0c1729` |
| Warm gradient | `color: #2d2a26` | `color: #2d2a26` |

---

## Progress Bar (Bottom of Every Slide)

Shows position in the carousel. Fills progressively.

```javascript
function progressBar(index, total, isLightSlide, accentColor) {
  const pct = ((index + 1) / total) * 100;
  const trackColor = isLightSlide ? 'rgba(0,0,0,0.08)' : 'rgba(255,255,255,0.12)';
  const fillColor  = isLightSlide ? accentColor : '#ffffff';
  const labelColor = isLightSlide ? 'rgba(0,0,0,0.3)' : 'rgba(255,255,255,0.4)';

  return `<div style="position:absolute;bottom:0;left:0;right:0;padding:14px 28px 18px;z-index:10;display:flex;align-items:center;gap:10px;">
    <div style="flex:1;height:3px;background:${trackColor};border-radius:2px;overflow:hidden;">
      <div style="height:100%;width:${pct}%;background:${fillColor};border-radius:2px;"></div>
    </div>
    <span style="font-family:'Poppins',sans-serif;font-size:10px;color:${labelColor};font-weight:500;">${index + 1}/${total}</span>
  </div>`;
}
```

### Rules
- Present on EVERY slide including CTA
- CTA slide: progress at 100%
- Track height: 3px, rounded corners
- Counter label: "1/7" format, 10px, weight 500
- On light slides: fill uses `brand.accent_color`
- On dark slides: fill is white

---

## Arrow Indicator (Every Slide Except CTA)

Bottom-right circle with arrow. Pulses gently.

```css
.arrow-indicator {
  position: absolute;
  bottom: 36px;
  right: 24px;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  z-index: 5;
  animation: pulse-arrow 2s ease-in-out infinite;
}

@keyframes pulse-arrow {
  0%, 100% { opacity: 0.6; }
  50% { opacity: 1; }
}
```

### Color Adaptation

| Background | Border | Text color |
|-----------|--------|------------|
| Dark | `border: 1.5px solid rgba(255,255,255,0.3)` | `color: rgba(255,255,255,0.6)` |
| Light | `border: 1.5px solid rgba(0,0,0,0.2)` | `color: rgba(0,0,0,0.5)` |
| Warm gradient | `border: 1.5px solid rgba(0,0,0,0.15)` | `color: rgba(0,0,0,0.4)` |

### Rules
- **NEVER** show on the CTA (last) slide
- Use `display: none` on CTA slide

---

## Handle Watermark (Every Slide)

Bottom-left text. Uses `brand.handle`.

```html
<div class="handle-watermark">{{ brand.handle | uppercase }}</div>
```

```css
.handle-watermark {
  position: absolute;
  bottom: 28px;
  left: 24px;
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  z-index: 5;
  opacity: 0.35;
}
```

- Dark backgrounds: `color: #ffffff`
- Light backgrounds: `color: #0c1729`

---

## CTA Slide (Last Slide — Same Design for ALL Styles)

Always white/textured regardless of carousel style. Pattern break that drives action.

### Background

```css
.slide-cta {
  background: #f5f5f0;
  position: relative;
}

.slide-cta::before {
  content: '';
  position: absolute;
  inset: 0;
  z-index: 1;
  opacity: 0.08;
  background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='420' height='525'%3E%3Cfilter id='n'%3E%3CfeTurbulence baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.4'/%3E%3C/svg%3E");
}
```

### Layout

```
┌──────────────────────────────────┐
│  Profile header (top-left)       │
│                                  │
│  Headline with accent-color      │
│  highlighted word(s)             │
│                                  │
│  Engagement question (centered)  │
│                                  │
│  Casual CTA line (centered)      │
│                                  │
│  "Share, Save & Follow"  ⟶  Photo│
│  (accent color)          120px   │
│  Progress bar (100%)             │
└──────────────────────────────────┘
```

### Elements

1. **Headline** — Poppins 700, 28px, `#0c1729`. Key word(s) highlighted: `background: brand.accent_color; color: #0c0c0c; padding: 2px 8px; border-radius: 4px;`
2. **Engagement question** — Poppins 600, 22px, `#0c1729`, centered
3. **Casual CTA** — Poppins 600, 20px, `#0c1729`, centered
4. **Profile photo** — 120×120px circle, bottom-right. Accent-color semi-circle (130px) behind it, offset -5px
5. **"Share, Save & Follow"** — Poppins 700, 22px, `brand.accent_color`, bottom-left
6. **Curved arrow** — from "Share" text toward profile photo

### Rules
- No arrow indicator on CTA
- Progress bar at 100%
- Profile header still present
- Handle watermark still present

---

## Instagram Preview Frame

Wraps the carousel for in-browser preview.

```css
.ig-frame {
  width: 420px;
  max-width: 420px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.12);
  overflow: hidden;
  font-family: 'Poppins', sans-serif;
  margin: 20px auto;
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
