# Figure Extraction Guide (paper-to-course)

> **When to read this:** During Phase 1.5, after analyzing the paper and deciding which figures the course needs. Extract only the figures you will actually use (typically 2-5).

All extraction runs **locally** — the PDF never leaves the machine.

## Setup (once per machine)

Install PyMuPDF into a virtual environment:

```bash
# Create and activate a venv, then install PyMuPDF
python -m venv venv
source venv/bin/activate   # macOS/Linux
# or: venv\Scripts\activate  # Windows
pip install pymupdf
```

## Strategy

Two approaches, in order of preference:

1. **Page-region render (preferred):** render the page at high zoom, crop the figure's rectangle. Works for every figure, including vector figures, and preserves exactly what the reader sees in the PDF.
2. **Embedded image extraction:** pull the raw embedded image. Only works when the figure is a single raster image; often fails for vector plots (KM curves, forest plots are usually vector). Use only if region render is unsatisfactory.

> **CRITICAL — NEVER eyeball the crop rectangle.** The single most common figure bug is **cutting off the top or bottom** (the author's "Figure N." title, the axis-label rows, or the caption). The root cause is estimating the crop rectangle by eye from a preview PNG: the image's real *page coordinates* drift from what you see, and at 2x zoom the numbers are easy to misread. **Always locate the figure's rectangle programmatically with `page.get_image_info()` first**, then expand it by hand to include the title and caption. Use the visual preview only as a cross-check, never as the primary locator.

## Script Template — Precise Bounding-Box Detection (RECOMMENDED — do this FIRST)

This returns the exact image rectangle in *page coordinates* (points), so your crop starts at the real top edge — not where your eye guessed. Run it before any crop.

```python
# bbox.py — find the real figure rectangles on a page
import fitz

doc = fitz.open(r"path/to/paper.pdf")
for pno in [4, 5, 6]:                 # 0-based page indices you care about
    page = doc[pno]
    print(f"===== PAGE {pno+1} (rect {page.rect.width:.0f} x {page.rect.height:.0f}) =====")
    for im in page.get_image_info(xrefs=True):
        b = im["bbox"]                  # (x0, y0, x1, y1) in points
        print(f"  image bbox: x[{b[0]:.0f},{b[2]:.0f}] y[{b[1]:.0f},{b[3]:.0f}]")
```

Then, to also know where the **"Figure N." title** and the **caption** sit (so you can expand the crop to include them), dump the text-line bounding boxes:

```python
# textboxes.py — locate title + caption so the crop includes them
import fitz
doc = fitz.open(r"path/to/paper.pdf")
page = doc[5]                           # the page holding the figure
for b in page.get_text("dict")["blocks"]:
    for line in b.get("lines", []):
        for span in line["spans"]:
            t = span["text"].strip()
            if t and (t.startswith("Figure") or "depicts" in t or "frontier" in t.lower()):
                bb = span["bbox"]
                print(f"  y={bb[1]:.0f}-{bb[3]:.0f} | {t[:70]}")
```

**Rule for the final clip rectangle:** start the crop a few points *above* the image's `y0` (to catch the "Figure N." title that usually sits just above the plot) and end a few points *below* the caption's bottom `y` (to catch the caption that sits just below the plot). Never let the image's own `y0`/`y1` be your crop edge — that guarantees a cut-off title or caption.

## Script Template — Full-Page Preview (cross-check only)

```python
# preview.py — render whole pages to eyeball figure locations
import fitz  # pymupdf

PDF = r"path/to/paper.pdf"
OUT = r"course-name/assets"

doc = fitz.open(PDF)
for pno in [7, 8, 9]:                      # pages you care about (0-based)
    page = doc[pno]
    pix = page.get_pixmap(matrix=fitz.Matrix(2, 2))   # 2x zoom preview
    pix.save(f"{OUT}/preview-p{pno+1}.png")
print("previews written")
```

Read the previews with the Read tool only to **confirm** the figure sits where the bbox says — not to re-estimate coordinates.

## Script Template — High-Res Region Crop

```python
# crop.py — render a page region at high resolution
import fitz

PDF = r"path/to/paper.pdf"
OUT = r"course-name/assets"

doc = fitz.open(PDF)
page = doc[7]                               # page 8 (0-based index 7)
print(page.rect)                            # e.g. Rect(0, 0, 595, 842)

# From bbox.py: image y[463, 694]; "Figure 2." title at y=444; caption below y=694.
# Start a few points ABOVE the image top, end a few points BELOW the caption.
clip = fitz.Rect(90, 435, 505, 700)         # left, top, right, bottom in points
pix = page.get_pixmap(matrix=fitz.Matrix(4, 4), clip=clip)   # 4x zoom = crisp
pix.save(f"{OUT}/fig2-km-curve.png")
print("saved", pix.width, "x", pix.height)
```

After saving, run the **Completeness Check** below before referencing the image in any module.

## Completeness Check (MANDATORY before referencing any figure)

A crop that "looks fine" is the trap. Verify in two passes:

**Pass 1 — programmatic (does the crop even contain the whole image?):**
```python
# verify_crop.py — confirm clip fully contains image + caption, nothing bleeds
import fitz
doc = fitz.open(r"path/to/paper.pdf")
page = doc[7]
imgs = page.get_image_info(xrefs=True)
clip = fitz.Rect(90, 435, 505, 700)        # the SAME clip you used to render
for im in imgs:
    b = im["bbox"]
    inside = (b[0] >= clip[0] and b[1] >= clip[1] and
               b[2] <= clip[2] and b[3] <= clip[3])
    print(f"image y[{b[1]:.0f},{b[3]:.0f}] fully inside clip? {inside}")
```
If `inside` is `False`, your clip is cutting the image — widen it. (The image's `y0`/`y1` must fall strictly inside the clip's `y1`/`y3`.)

**Pass 2 — visual (does anything bleed in, is text legible?):**
Read the saved PNG with the Read tool and confirm:
- ✅ The author's **"Figure N." title** is fully visible (top edge not clipped)
- ✅ The **axis labels, legends, and No.-at-Risk table** (under KM curves) are all inside
- ✅ The **caption** is fully visible and NO neighboring body text / text from the next section bleeds in at the bottom
- ✅ Text is crisp and legible at 4x zoom

If the visual shows a clipped title or a bleed of body text, recompute the clip from `get_image_info()` + the caption's text bbox and re-render. **Do not ship a figure you have not visually verified.**

## Script Template — Embedded Image Extraction (fallback)

```python
# extract_embedded.py — pull raster images embedded in a page
import fitz

doc = fitz.open(r"path/to/paper.pdf")
page = doc[7]
for i, img in enumerate(page.get_images(full=True)):
    xref = img[0]
    pix = fitz.Pixmap(doc, xref)
    if pix.n - pix.alpha >= 4:              # CMYK → RGB
        pix = fitz.Pixmap(fitz.csRGB, pix)
    pix.save(f"course-name/assets/p8-embedded-{i}.png")
print("done")
```

## Rules

- **File naming:** `figN-short-slug.png` (e.g. `fig2-km-curve.png`, `fig3-tornado.png`). Tables extracted as images: `tabN-slug.png`.
- **Resolution:** 4x zoom (`Matrix(4,4)`) is usually right — crisp on retina, file stays reasonable. Bump to 6x for dense forest plots.
- **Always verify visually** with the Read tool before referencing the image in module HTML — see the Completeness Check above (programmatic + visual, both passes).
- **Locate the crop from `get_image_info()`, never by eye.** Eyeballed rectangles silently cut the top (author's "Figure N." title, top axis rows) or bottom (caption, neighboring body text). This is the #1 figure-extraction defect.
- **Fidelity:** never redraw, recolor, annotate over, or "improve" the authors' figure. Crops must not cut off captions' key parts, footnotes with significance markers, or legends.
- **Failure fallback (in order):** re-render at higher zoom → widen crop to full page → skip the image and use a numbered step-card walkthrough that references the figure number so the reader can follow along in the PDF. Never ship a broken `<img>`.
- **Two-column PDFs:** figures can span both columns or sit in one — check the preview carefully before cropping; column text bleeding into a crop looks sloppy.
