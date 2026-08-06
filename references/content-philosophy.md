# Content Philosophy (paper-to-course)

> **When to read this:** During Phase 2.5 (writing module briefs) and Phase 3 (writing module HTML). These principles guide every content decision — what to show, how to explain it, and how to provoke thinking.

These principles are what separate a great paper course from a paper summary. They should guide every content decision:

### Show, Don't Tell — Aggressively Visual

People's eyes glaze over text blocks — especially when the underlying material is an academic paper. The course should feel closer to an infographic than a journal club handout. Hard rules:

**Text limits:**
- Max **2-3 sentences** per text block (Chinese reads denser — err shorter). Writing a fourth sentence? Stop and convert it into a visual.
- Every screen must be **at least 50% visual** (flow animations, translation blocks, figure walkthroughs, cards, chat animations — anything that isn't a paragraph).

**Convert text to visuals:**
- Study design description → **study flow animation** (筛选 → 随机 → 随访 → 分析), not a paragraph
- PICO / eligibility criteria → **cards or icon rows**, not a bulleted list
- "The authors then did X, then Y" → **numbered step cards**
- Explaining a paper passage → **原文↔大白话 translation block** (not a paragraph *about* the passage)
- Results with numbers → **figure walkthrough** on the real figure, or a comparison layout — never a wall of statistics
- Competing viewpoints (author vs. critic) → **group chat animation**

**Visual breathing room:**
- Generous spacing between elements; alternate full-width visuals and narrow text for rhythm
- Every module should have one "hero visual" that teaches the core concept at a glance

### 原文 ↔ 大白话 — Fidelity First

Every important claim gets a side-by-side translation block: verbatim paper excerpt (with page/section citation) on the left, sentence-by-sentence plain Chinese on the right.

**Critical: Quote the original exactly as-is.** Never trim, paraphrase, or "improve" the authors' sentences on the original side. The learner should be able to open the PDF at the cited page and find the exact text — that builds trust. Choose naturally short, punchy passages (2-4 sentences) rather than butchering long paragraphs.

**Critical: Numbers are sacred.** Every number in the course (effect sizes, CIs, p-values, sample sizes, costs, QALYs) must match the paper exactly, with its context (which population, which analysis). A course that misquotes one number loses all credibility. When in doubt, re-check the PDF.

**Interpretation is labeled as interpretation.** The plain-Chinese side may explain *why* the authors did something, but if you are inferring beyond what the paper states, say so ("论文没明说，但通常这么做是为了…"). Never present your inference as the paper's claim.

### One Concept Per Screen

Each screen teaches exactly one idea. A hazard ratio and a confidence interval are two screens, not one. If you need more space, add another screen — don't cram.

### Metaphors First, Then Reality — 大白话优先

Introduce every method or statistical concept with a metaphor from everyday life, then immediately ground it in this paper: "在这篇论文里，这一步对应的是…" The metaphor builds intuition; the paper grounds it.

**Rules for metaphors:**
- **No recycled metaphors.** Each concept deserves a metaphor that feels natural to *that specific idea*. Randomization is shuffling a deck before dealing. A confidence interval is a fishing net (we're fairly sure the fish is in the net, not where exactly). ITT is class-roster attendance. Blinding is a taste test with unmarked cups. Propensity scores are matchmaking profiles. Pick the metaphor that makes *this* concept click.
- **Never sacrifice correctness for cuteness.** If a metaphor breaks down in a way that would mislead (e.g. implying a CI has 95% probability of containing the truth in the Bayesian sense), either add the caveat in one sentence or pick a better metaphor.
- For a reader who is **对主题熟悉** (familiar with the topic), metaphors are still welcome but shorter — one line of intuition, then straight to the technical meat. For **对主题不熟悉** (unfamiliar), lead with the metaphor and let it carry the concept before any jargon.

### Start From What the Reader Already Knows

The reader picked up this paper for a reason — a drug they're evaluating, a method they keep hearing about, a decision they need to make. Module 1 always opens with the real-world question the paper answers ("如果你需要向医保局说明这个药值不值,这篇论文就是那块拼图"), not with "this paper was published in NEJM in 2024."

### Make It Memorable

Use "举一反三" callout boxes for methodology insights that transfer beyond this paper. Give the study's actors personality where natural — the ever-skeptical Reviewer 2, the overworked statistician. Humor is welcome; sloppiness is not.

### Glossary Tooltips — No Term Left Behind

Every technical term gets a dashed-underline tooltip on first use in each module, following the 中文 (English original) convention. Hover/tap shows a 1-2 sentence plain-Chinese definition, ideally with a mini-metaphor.

**Be extremely aggressive with tooltips.** If there is even a 1% chance the reader (especially if they picked **对主题不熟悉**) doesn't know a word, tooltip it. This includes:
- Statistical terms (HR, OR, RR, CI, p 值, ITT, per-protocol, Kaplan-Meier, Cox 模型…)
- Trial design terms (双盲, 分层随机, 非劣效, 交叉设计, washout…)
- HEOR terms (QALY, ICER, WTP 阈值, Markov 模型, 贴现…)
- Acronyms — ALWAYS tooltip acronyms on first use
- Journal/regulatory jargon (peer review, preprint, FDA 加速批准, NICE TA…)

**The vocabulary IS part of the learning.** Tooltips should teach the term so the reader can *use* it in a meeting — e.g. "HR (hazard ratio)：任意时刻两组事件风险之比。HR 0.70 可以口头说成'治疗组的瞬时风险比对照组低 30%'。"

For a reader who is **对主题熟悉**: still tooltip domain-specific and less-common terms, but skip basics the profile already covers (a HEOR professional doesn't need "p 值" defined — but might appreciate a precise note on "restricted mean survival time"). For **对主题不熟悉**: tooltip aggressively — even field-standard terms may need a plain-Chinese definition on first use.

### Open-Ended Discussion — Questions Without Verdicts

The critical-thinking module uses discussion cards, and their integrity matters:
- The course NEVER hands down a verdict ("这篇论文的缺陷是…"). It raises questions and offers thinking angles.
- Every discussion card is anchored to something specific in THIS paper — a design choice, a number, a sentence in the limitations section, something the authors *didn't* report.
- At least one angle per card should steelman the authors. Critical thinking includes understanding constraints (ethics, feasibility, regulatory requirements), not just finding flaws.
- Good sources of discussion prompts: comparator choice, endpoint choice (surrogate vs hard), subgroup emphasis, funding/COI, generalizability to the reader's own setting, what a NICE/HTA reviewer would flag.

### Quizzes That Test Reading Skill, Not Memory

Quizzes appear only in key modules (typically Methods and Results). They test whether the learner can *use* what they learned on new material, not recall facts.

**What to quiz (in order of value):**
1. **Interpretation scenarios** — "另一篇论文报告 HR=0.85, 95% CI 0.70–1.03，该怎么理解？" Applying the skill to new numbers is the gold standard.
2. **Misreading traps** — present a plausible-sounding but wrong reading and ask the learner to catch it ("同事说 p=0.07 就是'没效果'，对吗？"). Trains against the most common real-world misreadings.
3. **Design judgment** — "为什么作者用安慰剂对照而不是标准治疗？这是优点还是局限？" Tests whether they understood the *reasoning*.

**What NOT to quiz:**
- Definitions ("HR 是什么？") — that's what tooltips are for
- Numeric recall of exact values ("本试验 OS 是多少个月？") — that's a copy-paste test, not understanding
- Anything answered by scrolling up — tests scrolling, not comprehension

**Quiz tone:**
- Wrong answers get encouraging, non-judgmental explanations that teach something new
- No score, no "3/5!" — the quiz is a thinking exercise, not an exam
- For a reader who picked **更侧重方法** or **都深入**, lean into interpretation and judgment questions; for **对主题不熟悉**, anchor with more concrete scenario framing

**How many quizzes:** A small set (2-4 questions) in the key module(s). Placed after all the content for that module. Discussion cards are the open-ended counterpart — keep them separate from scored quizzes.