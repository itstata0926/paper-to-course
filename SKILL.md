---
name: paper-to-course
description: "Turn any academic paper (PDF) into a beautiful, interactive single-page HTML course that helps you actually understand it — and be able to brief others and think critically about it. Use this skill whenever someone wants to '把这篇论文做成课程', '帮我读懂这篇 paper', '论文讲解课程', 'make a course from this PDF', 'teach me this paper', or '把论文变成可读的讲解'. The course teaches the paper through scroll-based modules, animated study-flow diagrams, 原文↔大白话 (verbatim-excerpt ↔ plain-Chinese) translation blocks, embedded real figures, open-ended Socratic discussion cards, and a one-page briefing pack. Primary focus: 医药 / HEOR / HTA / 临床试验 / 药物经济学 / 统计方法学 papers; also handles general empirical and methodology papers. Everything runs locally — the PDF never leaves the machine."
---

# Paper-to-Course

Transform any paper into a stunning, interactive course that helps you *read and understand* it — no prior mastery of the methods required. The output is a **directory** containing pre-built `styles.css`, `main.js`, per-module HTML files, and an assembled `index.html` you open directly in the browser with no setup (only external dependency: Google Fonts CDN). The course explains the paper through scroll-based modules, animated study-flow diagrams, verbatim-excerpt ↔ plain-Chinese translation blocks, embedded real figures, Socratic discussion cards, and a reusable one-page briefing pack.

## First-Run Welcome

When the skill is first triggered and the user hasn't given a paper yet, introduce yourself and explain what you do:

> **我可以把一篇论文变成一门交互式课程，帮你真正读懂它——不需要你提前懂那些方法。**
>
> 把论文给我就行：
> - **本地 PDF** —— 例如"把 ./paper.pdf 做成课程"（推荐，全程本地，论文不出本机）
> - **粘贴正文** —— 也可以直接把摘要/正文文本贴给我
>
> 我会先读一遍论文，搞清楚它在研究什么、怎么做、结果怎么读，然后生成一门中文交互课程：有原文对照大白话、研究流程图、关键图表的看图步骤、开放式讨论题帮你做批判性思考，最后一页还能直接拿去汇报。整个东西就在你浏览器里打开，不用装任何东西。

Once you have the PDF, scan the title/abstract/type quickly, then ask **two reader-profile questions** (see "Reader Profile" below) before building. Do not ask for a curriculum approval — design internally and build.

## Reader Profile (first run: ask 2 questions, build right after)

The course is shaped by two independent axes. Ask both questions at first run (batch them into one message with 2 questions). The two answers together decide the course's **background thickness** and **where the depth goes** — never ask for curriculum approval; design internally from these two answers.

**Axis 1 · 对这篇论文主题的熟悉度（话题背景，不是通用专业水平）**
- **对主题熟悉** —— 你本来就在这话题里（比如你是做 HTA 的，读 HTA 能力框架论文）。领域词汇都懂，少铺垫背景，直接进论文本身。
- **对主题不熟悉** —— 这个话题对你比较新。要从"这是什么"开始搭背景（术语解释、比喻、领域地图），每步都能接住一个外行。

**Axis 2 · 你更想深入哪一块（解读侧重）**
- **更侧重方法部分** —— 重点讲"研究是怎么做出来的"：设计选择、统计/建模逻辑、为什么这么分析、还能怎么分析。方法模块给足篇幅。
- **更侧重结果解读** —— 重点讲"数字和图表到底在说什么"：效应量/置信区间怎么读、框架图/森林图怎么看、结论能不能站住。结果模块给足篇幅。
- **方法和结果都深入** —— 均衡铺开，两块都讲透（最稳的默认组合）。

> **两个维度组合 = 解读策略**（决定课程怎么"展开"）：
> - 熟悉 + 方法 → 背景从简，方法详细解读（"我懂这领域，但想看清它的方法学取舍"）
> - 熟悉 + 结果 → 背景从简，把结果和图表讲深讲透（"我想快速判断这结论能不能用"）
> - 熟悉 + 都深入 → 背景从简，两块均衡讲透
> - 不熟悉 + 方法 → 先把领域铺垫足，再详细解读方法
> - 不熟悉 + 结果 → 先把领域铺垫足，再把结果讲深讲透
> - 不熟悉 + 都深入 → 领域铺垫足 + 两块均衡讲透（最友好的默认）
>
> **如果用户没给偏好**：两个维度都按"最友好"默认（**对主题不熟悉** + **方法和结果都深入**），不必反复追问。但提问本身每次都要问——熟悉度与侧重会随论文主题变化，不能沿用上一次的选择。

This is NOT a study guide for becoming a researcher. It's a reading aid: the goal is that after the course, the user can (a) explain what the paper found in their own words, (b) put it to use in a meeting/report, and (c) ask the right critical questions about it.

## Why This Approach Works

Traditional paper-reading advice says "read more papers and you'll get it." That fails because a single paper sits on a mountain of unstated method assumptions. This skill inverts it: **start from the result you care about, then peel back the layers** — what question, what design, what method, what the numbers mean, where it's shaky. Each module answers "why should I care?" before "how does it work?", and the answer is always practical: *because this helps you judge the evidence, brief a colleague, or spot the spin.*

The course meets the reader where they are: "You need to tell your payer board whether this drug is worth it — this paper is the key piece of evidence. Here's how to read it so you don't get fooled." The final briefing pack turns understanding into something usable at work the same week.

The directory-based output is intentional: separating CSS/JS from content means the engine is never regenerated, each module is written independently (small, high-quality output), and the assembled `index.html` works offline with zero setup.

---

## The Process

### Phase 0: Intake & Audience

Get the PDF. If the user gave a path, read it. If they pasted text, use it. If they gave a GitHub/link, prefer a local download first (keep it on-machine for confidentiality); only fetch remotely if no local copy exists, and note it.

Quickly skim the title, abstract, and section headings. Identify the **paper type**:

- **RCT / 临床试验** — intervention, randomization, primary endpoint, blinding, ITT analysis
- **RWE / 真实世界研究（观察性）** — cohort / case-control / 真实世界数据 (RWD/RWE)：无随机分组，靠设计（匹配/加权/工具变量等）控制混杂
- **研究方案 / Protocol** — 研究设计文档或预注册方案（RCT 或 RWE 的方案），通常**没有 Results 章节**。课程重点放在"设计逻辑 + 分析计划"：先讲清楚打算怎么得出结论、靠什么控制偏倚、主要/次要终点怎么定，再讨论方案的强弱与可行性；结果模块改为"预期会看到什么 / 怎么判断方案能否产出可信结论"
- **CEA · 药物经济学模型** — Markov / decision tree, costs, QALYs, ICER
- **Meta 分析 / 网络 Meta** — PRISMA screening, pooled effect
- **方法学 / 统计论文** — a method, not a trial
- **综述 / 指南 / 框架共识** — synthesis

Then ask the **two reader-profile questions** (topic familiarity + emphasis — see "Reader Profile" above). Batch both into one message if possible. After that, proceed without further approval.

### Phase 1: Paper Analysis

Before writing any course HTML, deeply understand the paper. Read the PDF pages you need (title, abstract, intro, methods, results, discussion, limitations, tables/figures). Trace:

- The **research question** and the PICO (Population, Intervention, Comparator, Outcome(s))
- The **study design** end-to-end (screening → randomization → intervention → follow-up → analysis)
- The **methodology** chain — the statistical/modelling moves and *why* each was chosen
- The **key results** with numbers: effect sizes + 95% CIs (and/or p-values), sample sizes, costs/QALYs for CEA
- The authors' **conclusion** and stated **limitations**
- **Funding & conflicts of interest** (methods or acknowledgements)
- The **figure & table inventory** — which visuals carry the core story

**Figure out what the paper does yourself** by reading it — never ask the user to explain the paper. The course should open (Module 1) by stating what the paper investigates in plain language and why a reader would care, grounded in a concrete real-world scenario ("如果你要回答…这份证据是关键"), then trace into the design.

### Phase 1.5: Figure Extraction

For the 2-5 figures the course will actually use, extract them locally per `references/extract-figures.md` (PyMuPDF, high-res region crops, stored in `course-name/assets/`). **CRITICAL:** locate every crop rectangle from `page.get_image_info()` (the real image bbox in page coordinates) — never eyeball it from a preview PNG. Eyeballed rectangles silently cut the top (author's "Figure N." title, top axis rows) or bottom (caption / neighboring body text). Start the crop a few points above the image `y0` and end below the caption, then run the **two-pass Completeness Check** (programmatic containment + visual) before referencing it. If a figure can't be cleanly extracted, fall back (re-render at higher zoom → full-page crop → drop the image and use a numbered step-card walkthrough referencing the figure number). Never redraw or "improve" the authors' figure.

### Phase 2: Curriculum Design

Structure the course as **4–6 modules**. Fewer, better modules beat more, thinner ones. The arc always starts from the reader's world (why this paper matters) and moves toward the machinery underneath. It is a **menu, not a checklist** — pick the modules that serve this paper.

| Module Position | Purpose | Why it matters |
|---|---|---|
| 1 | 这篇论文在研究什么 — 一句话问题 + 为什么值得关心，从真实场景切入 | Ground everything in something concrete before any method |
| 2 | 研究是怎么做的 — 研究设计流程动画 + PICO 卡片 | Know the design so you can judge whether the conclusion is trustworthy |
| 3 | 方法拆解 — 原文↔大白话对照块 + 统计/建模方法比喻式讲解（+ 关键模块测验） | The "how it works" core; this is where a 更侧重方法 reader wants depth |
| 4 | 结果怎么读 — 原图截图 + 看图步骤 + 数字解读 | Turn numbers into meaning: what HR/CI/cost actually say here |
| 5 | 换你来审：批判性思考 — 开放式讨论卡片（无标准答案），可含"审稿人群聊" | Build the habit of asking the right questions, not just accepting |
| 6 | 汇报包 — 一句话总结 + 3 分钟电梯汇报 + 预期 Q&A | Make the understanding reusable at work this week |

Adapt the arc to the paper type. Examples:
- **CEA / 药经模型** → add a module on the **model structure** (states + transitions, via Interactive Architecture Diagram or study-flow).
- **Meta 分析** → Module 2 becomes the **PRISMA 筛选流程**; Module 4 walks a **森林图 (forest plot)**.
- **方法学论文** → Module 3 is the whole point (the method, in depth); Module 4 shows a worked example.

**解读侧重 (emphasis axis) 决定模块篇幅的分配** —— 第 0 步选了"偏方法 / 偏结果 / 都深入"，据此在 4–6 模块基础上微调：
- **偏方法** → Module 3 方法拆解给足篇幅（≥2 屏深度，必要时加一个方法深挖模块）；Module 4 结果保持标准。群聊、测验优先服务方法理解。
- **偏结果** → Module 4 结果模块讲深讲透、给足篇幅（更多看图步骤、更多数字情境卡、把"结论站不站得住"讲透）；Module 3 方法保持标准。
- **都深入**（默认）→ Module 3 + Module 4 都讲深讲透，均衡铺开。

**主题熟悉度 (familiarity axis) 决定背景铺垫的多少** —— 选了"熟悉主题"，Module 1 少铺垫、直接进论文，后续术语悬停密度也调低；选了"不熟悉"，Module 1 先把领域地图 / 关键术语 / 比喻搭起来，后续模块首遇术语必带中文 (English) 悬停。

**Mandatory interactive elements (every course must include ALL of these):**
- **原文↔大白话 Translation Blocks** — verbatim paper excerpt (with page/section citation) on the left, sentence-by-sentence plain Chinese on the right. At least one per module. Fidelity is non-negotiable: quote exactly, cite the location.
- **Study Flow Animation** — at least one across the course (试验流程 / 数据流 / 模型状态转移 / PRISMA 筛选). Every paper has a process to animate.
- **Group Chat Animation** — at least one (actors: 研究者 / 统计师 / 审稿人 / 患者代表 / HTA 评审员 / 临床医生). Always present, even if you must frame a concept as a conversation.
- **Open-Ended Discussion Cards** — at least 3 in the critical-thinking module. Questions WITH NO verdict; click reveals thinking angles, never "the answer." At least one angle steelmans the authors.
- **Glossary Tooltips** — on every technical term, first use per module, in 中文 (English original) format.

These five are the backbone. Other elements (figure walkthrough, quiz, architecture diagram, pattern cards, drag-and-drop) are added where they fit.

**Quizzes** are conditional: include a small set (2–4 questions) in key modules only — typically **Methods** and **Results** — testing reading *skill* (new numbers, a colleague's misreading, a design judgment), never recall. See `references/content-philosophy.md` > "Quizzes That Test Reading Skill." No scores, no pass/fail.

**Do NOT present the curriculum for approval — just build it.** Design internally, then build. If they want changes, they'll say so after seeing the result.

**After designing the curriculum, decide the build path:**
- **Simple paper** (single study, clear design, ≤5 modules, no heavy modelling) → Phase 3 Sequential.
- **Complex paper** (CEA model, network meta, multi-arm, 6+ modules, or many figures) → Phase 2.5 first, then Phase 3 Parallel.

### Phase 2.5: Module Briefs (complex papers only)

For complex papers, write a brief per module before any HTML (enables parallel writing). Read `references/module-brief-template.md` for the structure (it's paper-adapted: excerpts + page citations + key numbers + figure list instead of code snippets). Pre-extract the exact excerpts, numbers, and figure crops into each brief so writing agents never touch the PDF.

### Phase 3: Build the Course

The course output is a **directory**, not a single file. All CSS and JS are pre-built reference files — never regenerate them. Your job is only the HTML content.

**Output structure:**
```
course-name/
  styles.css       ← copied verbatim from references/styles.css
  main.js          ← copied verbatim from references/main.js
  _base.html       ← customized shell (title, accent color, nav dots, Chinese lang)
  _footer.html     ← copied verbatim from references/_footer.html
  build.sh         ← copied verbatim from references/build.sh
  assets/          ← extracted figures (Phase 1.5)
  briefs/          ← module briefs (complex papers only, can delete after build)
  modules/
    01-intro.html
    02-design.html
    ...
  index.html       ← assembled by build.sh (do not write manually)
```

**Step 1 (both paths): Setup** — Create the course directory and `assets/`. Copy these four files verbatim using Read + Write (do not regenerate their contents):
- `references/styles.css` → `course-name/styles.css`
- `references/main.js` → `course-name/main.js`
- `references/_footer.html` → `course-name/_footer.html`
- `references/build.sh` → `course-name/build.sh`

**Step 2 (both paths): Customize `_base.html`** — Read `references/_base.html`, then write it to `course-name/_base.html` with exactly three substitutions:
- Both instances of `COURSE_TITLE` → the actual course title (e.g. "读懂《XX 药治疗 XX 的 III 期 RCT》")
- The four `ACCENT_*` placeholders → the chosen accent color values (pick one palette from the comments in `_base.html`; teal/forest read well for medical topics)
- `NAV_DOTS` → one `<button class="nav-dot" ...>` per module

**Step 3: Write modules** — paths diverge here.

#### Sequential path (simple papers)
Read `references/content-philosophy.md` and `references/gotchas.md`. Then write modules one at a time. For each module, write `course-name/modules/0N-slug.html` containing only the `<section class="module" id="module-N">` block and its contents. Do not include `<html>`, `<head>`, `<body>`, `<style>`, or `<script>` tags. Read `references/interactive-elements.md` for each element's HTML pattern, and `references/design-system.md` (esp. "Chinese Typography Notes") when composing.

#### Parallel path (complex papers)
Process modules in parallel batches of up to 3. For each module batch, prepare:
- Its module brief (from `course-name/briefs/`)
- `references/content-philosophy.md` and `references/gotchas.md`
- Only the needed sections of `references/interactive-elements.md` and `references/design-system.md` listed in the brief

Write each module's file(s) to `course-name/modules/`. After finishing all modules, do a consistency check: nav dots match modules, transitions are coherent, the reader profile (familiarity + emphasis) is consistent, no tone shifts, numbers match the briefs.

**Step 4 (both paths): Assemble** — Run `build.sh` from the course directory:
```bash
cd course-name && bash build.sh
```
This produces `index.html`.

**Critical rules:**
- **Never regenerate** `styles.css` or `main.js` — always copy from references
- Module files contain only `<section>` content — no boilerplate
- Use CSS `scroll-snap-type: y proximity` (NOT `mandatory`)
- Use `min-height: 100dvh` with `100vh` fallback on `.module`
- Interactive element JS is in `main.js`; wire up via `data-*` attributes and CSS class names as shown in `references/interactive-elements.md`
- Chat containers need `id` attributes; flow animations need `data-steps='[...]'` JSON on `.flow-animation`
- Figures live in `assets/` and are referenced as `<img src="assets/figN-slug.png">`
- **Numbers and verbatim quotes must match the paper exactly** — spot-check during Phase 4

### Phase 4: Review and Open

After `build.sh`, **read `references/gotchas.md` and verify every item**, especially: numbers match the paper; translation blocks quote verbatim with citations; discussion cards have no verdicts; figures render and are uncropped; Chinese punctuation/CN-EN spacing is correct; tooltips present on first use; the five mandatory interactive elements all appear; reader profile (familiarity + emphasis) is consistent.

Then open `index.html` in the browser, walk the user through what was built, and ask for feedback on content, design, and interactivity. Offer to adjust the reader profile (familiarity or emphasis), add a module, or expand any explanation.

---

## Design Identity

The visual design feels like a **beautiful reading notebook** — warm, inviting, distinctive, the kind of thing you'd actually want to read a dense paper in.

- **Warm palette**: off-white backgrounds (aged paper), warm grays, NO cold whites or blues
- **Bold accent**: one confident accent color — teal or forest reads especially well for medical/clinical topics (NOT purple gradients)
- **Distinctive typography**: display font with personality for headings (Bricolage Grotesque) + Noto Sans SC for Chinese; clean sans-serif body (DM Sans + Noto Sans SC); JetBrains Mono for codes/numbers/captions
- **Generous whitespace**: modules breathe; max 2-3 short sentences per screen in Chinese
- **Alternating backgrounds**: even/odd modules alternate between two warm tones
- **Dark code/quote blocks**: IDE-style dark panel (`#1E1E2E`) for the verbatim 原文 side — visually signals "this is the primary source"
- **Depth without harshness**: subtle warm shadows, never black drop shadows

---

## Reference Files

The `references/` directory contains detailed specs. **Read them only when you reach the relevant phase** — not upfront. This keeps context lean.

- **`references/content-philosophy.md`** — Visual density rules, 原文↔大白话 fidelity rules, metaphor guidelines, open-ended-discussion principles, tooltip rules, quiz design. Read during Phase 2.5 (briefs) and Phase 3 (writing modules).
- **`references/gotchas.md`** — Paper-specific failure-point checklist (number drift, over-interpretation, discussion verdicts, figure failure, tooltip clipping, CN-EN spacing). Read during Phase 3 and Phase 4 (review).
- **`references/module-brief-template.md`** — Paper-adapted template for Phase 2.5 module briefs (excerpts + citations + key numbers + figures). Read only for complex papers using the parallel path.
- **`references/design-system.md`** — CSS tokens, color palette, typography (incl. Chinese typography notes), spacing, shadows, animations. Read during Phase 3 when writing module HTML.
- **`references/interactive-elements.md`** — HTML patterns for every interactive element: 原文↔大白话 blocks, multiple-choice & scenario quizzes, drag-and-drop, group chat, study-flow animation, architecture diagram, callout boxes, pattern cards, flow diagrams, glossary tooltips, icon rows, numbered step cards, **discussion cards ★**, **figure walkthrough ★**, **briefing pack ★**. Read the relevant sections during Phase 3.
- **`references/extract-figures.md`** — PyMuPDF scripts for local, high-res figure extraction (preview → crop → verify) and failure fallbacks. Read during Phase 1.5.
