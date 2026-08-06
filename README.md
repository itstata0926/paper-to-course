# Paper-to-Course

Turn any academic paper (PDF) into a beautiful, interactive single-page HTML course that helps you actually understand it.

This is an AI agent workflow — give the `SKILL.md` to your agent along with a PDF, and it will build an interactive course that includes: scroll-based modules, animated study-flow diagrams, verbatim-excerpt to plain-Chinese translation blocks, embedded real figures, Socratic discussion cards, and a one-page briefing pack.

## Quick Start (for Codex / any AI agent)

1. Drop this entire folder into your project directory.
2. Put your PDF there too.
3. Tell your agent:

> Read `paper-to-course/SKILL.md` and follow its instructions. Build an interactive HTML course from `./your-paper.pdf`. Use `paper-to-course/references/` for all templates, CSS, and JS.

The agent will:
- Read and analyze the PDF
- Ask you two questions (your familiarity with the topic, what you want to learn most)
- Extract key figures from the PDF (requires Python + PyMuPDF)
- Write per-module HTML files
- Assemble everything into `index.html`
- Open it in your browser — runs entirely locally, no setup needed

## What You Need

- A PDF of an academic paper (works best with clinical trials, HTA/HEOR papers, meta-analyses, methodology papers)
- Python 3 + PyMuPDF (`pip install pymupdf`) for figure extraction
- A browser to view the output — that's it

## File Structure

```
paper-to-course/
  SKILL.md                  ← The instructions the agent reads and follows
  README.md                 ← This file
  references/
    _base.html              ← Base HTML shell (the agent customizes title + accent color)
    _footer.html            ← Page footer
    build.sh                ← Shell script that assembles modules into index.html
    styles.css              ← All CSS (copied verbatim, never regenerated)
    main.js                 ← All JS (copied verbatim, never regenerated)
    content-philosophy.md   ← Design rules for explanations, metaphors, quizzes
    design-system.md        ← Visual tokens: colors, typography, spacing, shadows
    gotchas.md              ← Common failure points to check before shipping
    interactive-elements.md ← HTML patterns for every interactive component
    extract-figures.md      ← How to crop figures from PDF locally using PyMuPDF
    module-brief-template.md← Template for module briefs (complex papers)
```

## How It Works

The course is generated as a **directory of files**, not a single blob:

```
course-name/
  styles.css       ← copied from references/
  main.js          ← copied from references/
  _base.html       ← customised shell
  _footer.html     ← copied from references/
  build.sh         ← copied from references/
  assets/          ← extracted figures
  modules/
    01-intro.html
    02-design.html
    03-methods.html
    04-results.html
    05-discussion.html
    06-briefing.html
  index.html       ← assembled by build.sh
```

Open `index.html` in any browser. No server, no build step, no external dependencies (only Google Fonts CDN for typography).

## Primary Focus

Built for medical / HEOR / HTA / clinical trial / pharmacoeconomics / statistical methodology papers. Also handles general empirical and methodology papers.

## Credits

Created by and for the HEOR/HTA community. Everything runs locally — your PDF never leaves your machine.
