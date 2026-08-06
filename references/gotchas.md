# Gotchas — Common Failure Points (paper-to-course)

> **When to read this:** During Phase 3 (writing module HTML) and Phase 4 (review). Check every one of these before considering a course complete.

### 可见文字禁止双引号与破折号
读者会看到的所有文字（模块正文、按钮、课程标题、导航标签、术语悬停、测验题干、讨论问题、汇报包）都不得出现双引号（"）或破折号（—）。这两种符号会露出机器腔或翻译腔，破坏人话感。
替换方式：
- 要引用论文名或强调某个词 → 用《》，或干脆不加任何符号
- 插入说明 → 用全角括号（）
- 本想用破折号做停顿或强调 → 改成逗号（，），或拆成两句用句号（。）
- 行内英文或代码用反引号，不要包双引号
Phase 4 复查时，打开生成的 index.html，搜索可见文字里的 " 和 —，逐处替换后再交付。

### Numbers That Don't Match the Paper
The #1 credibility killer. Every effect size, CI, p-value, sample size, cost, and QALY in the course must match the PDF exactly — including which analysis population it came from (ITT vs per-protocol, base case vs sensitivity). During Phase 4 review, spot-check every number in the briefing pack and every translation block against the PDF. If a number was rounded, keep the paper's precision or say "约".

### Over-Interpretation Presented as Fact
Writing "作者这样做是因为 X" when the paper never says why. Inference is allowed but must be labeled ("论文没明说，但通常…"). The discussion module raises questions — the teaching modules must not smuggle in verdicts.

### Discussion Cards With Verdicts
The open-ended discussion cards must NOT conclude ("所以这是一个重大缺陷"). Angles are phrased as questions or considerations. At least one angle steelmans the authors. If a card reads like a review report, rewrite it.

### Verbatim Quote Drift
Paraphrasing the original text on the 原文 side of translation blocks, or quoting without page/section citation. The learner must be able to open the PDF and find the exact sentence. Always cite like `原文 · p.6, Methods`.

### Figure Extraction Failures
Two distinct failure modes — both must be caught in Phase 4.

**Mode A — broken/blank/low-res image.** If extraction produces a dead image, do NOT ship a broken `<img>`. Fallbacks in order: (1) re-render the page region at higher zoom; (2) crop the full-page render; (3) drop the image and describe the figure with a numbered step-card walkthrough referencing the figure number so the reader can look at the PDF side by side. Never redraw the figure from imagination.

**Mode B — cropped but incomplete (most common, easy to miss).** The image renders but a part is cut off: the author's "Figure N." title, the top/bottom axis-label rows, the legend, the No.-at-Risk table under KM curves, or the caption (with body text from the next section bleeding in at the bottom). **Root cause:** the crop rectangle was eyeballed from a preview PNG instead of read from `page.get_image_info()`. The image's real page-coordinate top is higher than it looks, so the crop silently drops the title/top rows. **Fix:** always derive the crop from `get_image_info()` (see `extract-figures.md`), start the crop a few points *above* the image's `y0` and end a few points *below* the caption, then run both the programmatic and visual completeness checks before referencing the figure.

### Tooltip Clipping
Translation blocks use `overflow: hidden`. Tooltips must use `position: fixed` appended to `document.body` — already handled by `main.js`, but verify on review; it is the most recurrent rendering bug.

### Not Enough Tooltips
Under-tooltipping is the most common content failure. For the **对主题不熟悉** reader, terms like 随机对照试验、置信区间、HR、终点、队列 all need tooltips. For the **对主题熟悉** reader, skip domain basics but still tooltip niche or paper-specific terms. Acronyms always get tooltips on first use.

### Walls of Text
The course looks like a journal club handout instead of an infographic. More than 2-3 sentences without a visual break = rewrite. Chinese text reads denser than English — err even shorter. Every screen at least 50% visual.

### Recycled Metaphors
Using the same metaphor for every statistical concept. Each concept deserves its own organically fitting metaphor. Also: never let a metaphor mislead — if it breaks down in a materially wrong way, add the caveat or replace it.

### Quiz Questions That Test Memory
"本试验入组了多少人？" tests copy-paste, not understanding. Every quiz question presents new material (new numbers, a colleague's claim, another study) and asks the learner to *apply* the reading skill. Quizzes only in key modules (Methods/Results); no scores.

### Mixed Punctuation / CN-EN Spacing
Half-width commas inside Chinese prose, or missing spaces between Chinese and inline English/numbers ("HR0.70表示"). Follow the Chinese typography notes in `design-system.md`.

### Scroll-Snap Mandatory
Using `scroll-snap-type: y mandatory` traps users inside long modules. Always use `proximity`.

### Module Quality Degradation
Writing all modules in one pass makes later modules thin. Build one module at a time and verify each. For long/complex papers, use the parallel path with module briefs.

### Missing Interactive Elements
A module with only text is a summary, not a course. Every module needs at least one of: translation block, flow animation, group chat, figure walkthrough, discussion card, quiz. Check the five mandatory element types (see SKILL.md) are all present across the course.

### Audience Level Ignored
The course was asked for **对主题熟悉** but explains what a p-value is for three screens (or vice versa: **对主题不熟悉** reader handed naked jargon). Re-read the chosen reader profile (familiarity + emphasis) before writing each module brief.
