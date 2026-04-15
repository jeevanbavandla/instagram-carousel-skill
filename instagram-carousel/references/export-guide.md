# Instagram Carousel Export Guide

## How the Export Works

Each carousel is built as a self-contained HTML file with all slides laid out in a horizontal `.carousel-track`. The design viewport is **420x525px** -- this is the size everything is authored at inside the browser.

For Instagram, we need **1080x1350px PNG** files (one per slide). Instead of redesigning at 1080px, we use Playwright's `device_scale_factor` to render the 420px layout at a higher pixel density:

```
Scale factor = 1080 / 420 = 2.5714
```

Playwright captures a 420x525 CSS-pixel viewport, but the underlying bitmap is 1080x1350 physical pixels. Each slide is exported by translating the `.carousel-track` left and clipping the screenshot to the visible frame.

---

## Critical Rules

1. **Always use Python for HTML generation.** Shell scripts (`bash`, `zsh`) corrupt variables, mangle special characters, and silently break embedded CSS/JS. Python string handling is reliable for building HTML with inline styles, base64 images, and nested quotes.

2. **Always base64-encode embedded images.** Profile photos, logos, and any image asset must be converted to a base64 data URI and inlined directly into the HTML. This makes the file fully self-contained -- no broken images when the file moves or opens on a different machine.

3. **Keep `.ig-frame` at exactly 420px width.** The entire export pipeline depends on the design being 420px wide. Never change this value. The scale factor handles the rest.

4. **Check image formats before embedding.** Verify that image files are valid (PNG, JPG, WEBP) before base64-encoding. Corrupted or missing images will render as broken placeholders in the export.

---

## Playwright Export Script

This is the full async Python script that exports each slide as a 1080x1350px PNG.

```python
import asyncio
import os
import sys
from playwright.async_api import async_playwright

async def export_carousel(html_path: str, output_dir: str):
    """
    Export each slide of an Instagram carousel HTML file as a 1080x1350px PNG.
    
    Args:
        html_path: Absolute path to the carousel HTML file.
        output_dir: Directory where slide PNGs will be saved.
    """
    os.makedirs(output_dir, exist_ok=True)

    # Design dimensions (CSS pixels)
    DESIGN_WIDTH = 420
    DESIGN_HEIGHT = 525

    # Export dimensions (physical pixels)
    EXPORT_WIDTH = 1080
    EXPORT_HEIGHT = 1350

    # Scale factor: 1080 / 420 = 2.5714...
    SCALE_FACTOR = EXPORT_WIDTH / DESIGN_WIDTH

    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page(
            viewport={"width": DESIGN_WIDTH, "height": DESIGN_HEIGHT},
            device_scale_factor=SCALE_FACTOR,
        )

        # Load the HTML file
        file_url = f"file://{os.path.abspath(html_path)}"
        await page.goto(file_url, wait_until="networkidle")

        # Wait for Google Fonts to load (Poppins, etc.)
        await page.wait_for_timeout(3000)

        # Hide Instagram preview frame chrome so only the slide content is captured
        await page.evaluate("""
            () => {
                const selectors = ['.ig-header', '.ig-dots', '.ig-actions', '.ig-caption'];
                selectors.forEach(sel => {
                    const el = document.querySelector(sel);
                    if (el) el.style.display = 'none';
                });
            }
        """)

        # Disable transitions so slide repositioning is instant
        await page.evaluate("""
            () => {
                const track = document.querySelector('.carousel-track');
                if (track) track.style.transition = 'none';
            }
        """)

        # Count the number of slides
        slide_count = await page.evaluate("""
            () => document.querySelectorAll('.carousel-track .slide').length
        """)

        if slide_count == 0:
            print("No slides found in the carousel.")
            await browser.close()
            return

        print(f"Found {slide_count} slides. Exporting...")

        for i in range(slide_count):
            # Move the carousel track to show the current slide
            offset = i * DESIGN_WIDTH
            await page.evaluate(f"""
                () => {{
                    const track = document.querySelector('.carousel-track');
                    if (track) track.style.transform = 'translateX(-{offset}px)';
                }}
            """)

            # Brief pause to let the DOM settle
            await page.wait_for_timeout(100)

            # Screenshot the visible 420x525 area -- Playwright renders it at 1080x1350
            output_path = os.path.join(output_dir, f"slide_{i + 1}.png")
            await page.screenshot(
                path=output_path,
                clip={
                    "x": 0,
                    "y": 0,
                    "width": DESIGN_WIDTH,
                    "height": DESIGN_HEIGHT,
                },
            )
            print(f"  Exported: {output_path}")

        await browser.close()
        print(f"Done. {slide_count} slides exported to {output_dir}")


if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: python export_carousel.py <path_to_html> [output_dir]")
        sys.exit(1)

    html_file = sys.argv[1]
    out_dir = sys.argv[2] if len(sys.argv) > 2 else os.path.join(
        os.path.dirname(os.path.abspath(html_file)), "exports"
    )

    asyncio.run(export_carousel(html_file, out_dir))
```

### Running the script

```bash
# Install Playwright if not already installed
pip install playwright
playwright install chromium

# Export slides
python export_carousel.py /path/to/carousel.html /path/to/output/
```

This produces `slide_1.png`, `slide_2.png`, etc. -- each exactly 1080x1350px, ready to upload to Instagram.

---

## Why This Approach Works

### `device_scale_factor` instead of a larger viewport

Setting the viewport to 1080px would reflow the entire layout -- text wrapping, padding, and element sizing would all change because CSS is responsive to viewport width. By keeping the viewport at 420px and using `device_scale_factor=2.5714`, we tell the browser: "Render the layout as if the screen is 420px wide, but paint it onto a 1080px bitmap." The CSS layout stays identical; only the pixel density increases.

### `clip` for per-slide isolation

The `clip` parameter tells Playwright to capture only the 420x525 CSS-pixel rectangle at the top-left of the page. Combined with translating the `.carousel-track`, each slide lands in that exact rectangle. No cropping or post-processing needed.

### Font wait (3000ms)

Google Fonts are loaded via `<link>` tags from fonts.googleapis.com. Even after `networkidle`, the browser may still be parsing and applying the font files. A 3000ms wait ensures Poppins (and any other web fonts) are fully rendered before screenshots begin. Without this, slides may export with fallback system fonts.

### `transition: none`

The carousel track normally has CSS transitions for smooth swiping in the preview. During export, these transitions would cause the track to animate between positions, producing blurred or mid-animation screenshots. Setting `transition: none` makes repositioning instant.

---

## Common Mistakes to Avoid

1. **Setting the viewport to 1080x1350.** This breaks the layout. The design is authored at 420px -- a 1080px viewport causes text reflow, changes padding ratios, and produces slides that look nothing like the preview. Always use 420x525 viewport with `device_scale_factor`.

2. **Using shell scripts for HTML generation.** Bash/zsh cannot safely handle the nested quotes, special characters, and multi-line strings in carousel HTML. Variables get expanded, quotes get stripped, and CSS breaks silently. Always use Python to generate the HTML file.

3. **Not waiting for Google Fonts to load.** If you screenshot immediately after `networkidle`, fonts may not be applied yet. The result is slides rendered in Times New Roman or Arial instead of Poppins. Always include the 3000ms wait after page load.

4. **Not hiding Instagram frame chrome.** The HTML preview includes `.ig-header` (username bar), `.ig-dots` (slide indicators), `.ig-actions` (like/comment/share icons), and `.ig-caption` (text below the image). These are preview-only UI elements. If you don't hide them before export, they will appear in the PNG output and waste space on non-content elements.

5. **Changing `.ig-frame` width.** The `.ig-frame` container must stay at exactly 420px. Changing it breaks the relationship between slide width, track translation offsets, and the clip rectangle. Every part of the export pipeline assumes 420px as the base unit.

---

## Alternative: Browser-Based Export Fallback

If Playwright is not installed (or cannot be installed in the current environment), the carousel HTML files include built-in export buttons that use **html2canvas** as a client-side fallback.

### How it works

Each carousel HTML file contains an export UI (typically a "Download Slides" button) that:

1. Iterates through each slide in the carousel
2. Uses html2canvas to rasterize the slide DOM into a canvas element
3. Scales the canvas output to 1080x1350px
4. Triggers a PNG download for each slide

### Limitations

html2canvas is a JavaScript library that re-renders DOM elements onto a `<canvas>`. It does **not** use the browser's actual rendering engine for the screenshot. This means:

- **Font rendering differences** -- text may appear slightly different (weight, kerning, anti-aliasing)
- **CSS property gaps** -- some CSS features (certain gradients, filters, blend modes, backdrop-filter) are not fully supported
- **Shadow and border-radius issues** -- complex box-shadows or rounded corners may render incorrectly
- **Image quality** -- the scaling algorithm may produce softer results compared to Playwright's native rendering

### Recommendation

**Playwright is the recommended method** for pixel-perfect output. The html2canvas fallback exists as a convenience for quick exports when you cannot run a Python script, but the output quality will not match Playwright exports. For any content going to a live Instagram account, use the Playwright script above.
