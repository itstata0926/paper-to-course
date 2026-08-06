# Interactive Elements Reference (paper-to-course)

Implementation patterns for every interactive element type used in paper courses. Pick the elements that best serve each module's teaching goal.

> **Architecture note:** All CSS and JavaScript for these elements live in `references/styles.css` and `references/main.js`, which are copied verbatim into every course directory. When writing module HTML files, use only the HTML patterns below — do **not** inline `<style>` or `<script>` tags for these elements. The engines in `main.js` auto-initialize on page load by scanning for the relevant class names and `data-*` attributes described here.

## Table of Contents
1. [原文 ↔ 大白话 Translation Blocks](#原文--大白话-translation-blocks)
2. [Multiple-Choice Quizzes](#multiple-choice-quizzes)
3. [Drag-and-Drop Matching](#drag-and-drop-matching)
4. [Group Chat Animation](#group-chat-animation)
5. [Study Flow Animation](#study-flow-animation)
6. [Interactive Architecture Diagram](#interactive-architecture-diagram)
7. [Scenario Quiz](#scenario-quiz)
8. [Callout Boxes](#callout-boxes)
9. [Pattern/Feature Cards](#patternfeature-cards)
10. [Flow Diagrams](#flow-diagrams)
11. [Glossary Tooltips](#glossary-tooltips)
12. [Icon-Label Rows](#icon-label-rows)
13. [Numbered Step Cards](#numbered-step-cards)
14. [Discussion Cards ★new](#discussion-cards-open-ended-critical-thinking)
15. [Figure Walkthrough ★new](#figure-walkthrough)
16. [Briefing Pack ★new](#briefing-pack)

---

## 原文 ↔ 大白话 Translation Blocks

The most important teaching element. Shows a verbatim excerpt from the paper on the left (dark panel, keeps the "primary source" feel) and a sentence-by-sentence plain-Chinese translation + interpretation on the right.

Reuses the `translation-block` component unchanged — the left panel simply holds paper text instead of code. Break the original excerpt into short sentence-level `.code-line` spans so each maps to one plain-Chinese line on the right.

**HTML:**
```html
<div class="translation-block animate-in">
  <div class="translation-code">
    <span class="translation-label">原文 · p.6, Methods</span>
    <pre><code>
<span class="code-line">The primary endpoint was progression-free survival,</span>
<span class="code-line">assessed by blinded independent central review</span>
<span class="code-line">in the intention-to-treat population.</span>
    </code></pre>
  </div>
  <div class="translation-english">
    <span class="translation-label">大白话</span>
    <div class="translation-lines">
      <p class="tl">研究最关心的指标是无进展生存期，病人从入组到肿瘤恶化（或去世）撑了多久。</p>
      <p class="tl">恶化没恶化不是主治医生说了算，而是由一组不知道分组情况的独立影像专家来判，防止自家孩子自家夸。</p>
      <p class="tl">统计时按意向性分析 (ITT)：只要随机进组就算数，中途退出也不剔除，保住随机化的公平性。</p>
    </div>
  </div>
</div>
```

**CSS:**
```css
.translation-block {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0;
  border-radius: var(--radius-md);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  margin: var(--space-8) 0;
}
.translation-code {
  background: var(--color-bg-code);
  color: #CDD6F4;
  padding: var(--space-6);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  line-height: 1.7;
  position: relative;
  overflow-x: hidden;  /* NO horizontal scrollbar — ever */
}
.translation-code pre,
.translation-code code {
  white-space: pre-wrap;       /* wrap long lines instead of scrolling */
  word-break: break-word;      /* break mid-word if needed */
  overflow-x: hidden;
}
.translation-english {
  background: var(--color-surface-warm);
  padding: var(--space-6);
  font-size: var(--text-sm);
  line-height: 1.7;
  border-left: 3px solid var(--color-accent);
}
.translation-label {
  position: absolute;
  top: var(--space-2);
  right: var(--space-3);
  font-size: var(--text-xs);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  opacity: 0.5;
}
.translation-english .translation-label {
  color: var(--color-text-muted);
}
/* Responsive: stack vertically on mobile */
@media (max-width: 768px) {
  .translation-block { grid-template-columns: 1fr; }
  .translation-english { border-left: none; border-top: 3px solid var(--color-accent); }
}
```

**Rules:**
- Each plain-Chinese line should correspond to one sentence (or clause) of the original — never a whole paragraph
- The label on the left MUST cite the location: page number and section (e.g. `原文 · p.6, Methods`) so the reader can find it in the PDF
- Quote the original **verbatim** — never paraphrase, trim ellipses silently, or "fix" the authors' wording
- Translate the "why" not just the "what" — e.g. "由不知道分组的独立专家来判，防止自家孩子自家夸" beats "采用盲态独立中央评审"
- Technical terms in the plain-Chinese side follow the 中文 (English original) convention on first use, wrapped in a glossary tooltip

---

## Multiple-Choice Quizzes

For testing understanding with instant feedback. Each question has options, one correct answer, and per-question explanations.

**Wiring:** `main.js` exposes `window.selectOption(btn)`, `window.checkQuiz(containerId)`, and `window.resetQuiz(containerId)`. Call them via `onclick`. Per-question explanations go in `data-explanation-right` and `data-explanation-wrong` on the `.quiz-question-block`.

**HTML:**
```html
<div class="quiz-container" id="quiz-module3">
  <div class="quiz-question-block"
       data-correct="option-b"
       data-explanation-right="Exactly — because X is responsible for Y in this architecture."
       data-explanation-wrong="Not quite. Think about where Y lives in the codebase...">
    <h3 class="quiz-question">Question text here?</h3>
    <div class="quiz-options">
      <button class="quiz-option" data-value="option-a" onclick="selectOption(this)">
        <div class="quiz-option-radio"></div>
        <span>Answer A</span>
      </button>
      <button class="quiz-option" data-value="option-b" onclick="selectOption(this)">
        <div class="quiz-option-radio"></div>
        <span>Answer B (correct)</span>
      </button>
      <button class="quiz-option" data-value="option-c" onclick="selectOption(this)">
        <div class="quiz-option-radio"></div>
        <span>Answer C</span>
      </button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <button class="quiz-check-btn" onclick="checkQuiz('quiz-module3')">Check Answers</button>
  <button class="quiz-reset-btn" onclick="resetQuiz('quiz-module3')">Try Again</button>
</div>
```

**CSS for quiz states:**
```css
.quiz-option {
  display: flex; align-items: center; gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface);
  cursor: pointer; width: 100%;
  transition: border-color var(--duration-fast), background var(--duration-fast);
}
.quiz-option:hover { border-color: var(--color-accent-muted); }
.quiz-option.selected { border-color: var(--color-accent); background: var(--color-accent-light); }
.quiz-option.correct { border-color: var(--color-success); background: var(--color-success-light); }
.quiz-option.incorrect { border-color: var(--color-error); background: var(--color-error-light); }
.quiz-option-radio {
  width: 18px; height: 18px; border-radius: 50%;
  border: 2px solid var(--color-border);
  transition: all var(--duration-fast);
}
.quiz-option.selected .quiz-option-radio {
  border-color: var(--color-accent);
  background: var(--color-accent);
  box-shadow: inset 0 0 0 3px white;
}
.quiz-feedback {
  max-height: 0; overflow: hidden; opacity: 0;
  transition: max-height var(--duration-normal), opacity var(--duration-normal);
}
.quiz-feedback.show { max-height: 200px; opacity: 1; padding: var(--space-3); margin-top: var(--space-2); border-radius: var(--radius-sm); }
.quiz-feedback.success { background: var(--color-success-light); color: var(--color-success); }
.quiz-feedback.error { background: var(--color-error-light); color: var(--color-error); }
```

---

## Drag-and-Drop Matching

For matching concepts to descriptions. Supports both mouse (HTML5 Drag API) and touch.

**HTML:**
```html
<div class="dnd-container">
  <div class="dnd-chips">
    <div class="dnd-chip" draggable="true" data-answer="actor-a">Actor A</div>
    <div class="dnd-chip" draggable="true" data-answer="actor-b">Actor B</div>
    <div class="dnd-chip" draggable="true" data-answer="actor-c">Actor C</div>
  </div>
  <div class="dnd-zones">
    <div class="dnd-zone" data-correct="actor-a">
      <p class="dnd-zone-label">Description for Actor A</p>
      <div class="dnd-zone-target">Drop here</div>
    </div>
    <!-- more zones -->
  </div>
  <button onclick="checkDnD()">Check Matches</button>
  <button onclick="resetDnD()">Reset</button>
</div>
```

**JS (mouse + touch):**
```javascript
// MOUSE: HTML5 Drag API
chips.forEach(chip => {
  chip.addEventListener('dragstart', (e) => {
    e.dataTransfer.setData('text/plain', chip.dataset.answer);
    chip.classList.add('dragging');
  });
  chip.addEventListener('dragend', () => chip.classList.remove('dragging'));
});

zones.forEach(zone => {
  const target = zone.querySelector('.dnd-zone-target');
  target.addEventListener('dragover', (e) => { e.preventDefault(); target.classList.add('drag-over'); });
  target.addEventListener('dragleave', () => target.classList.remove('drag-over'));
  target.addEventListener('drop', (e) => {
    e.preventDefault();
    target.classList.remove('drag-over');
    const answer = e.dataTransfer.getData('text/plain');
    const chip = document.querySelector(`[data-answer="${answer}"]`);
    target.textContent = chip.textContent;
    target.dataset.placed = answer;
    chip.classList.add('placed');
  });
});

// TOUCH: Custom implementation (HTML5 drag doesn't work on mobile)
chips.forEach(chip => {
  chip.addEventListener('touchstart', (e) => {
    e.preventDefault();
    const touch = e.touches[0];
    const clone = chip.cloneNode(true);
    clone.classList.add('touch-ghost');
    clone.style.cssText = `position:fixed; z-index:1000; pointer-events:none;
      left:${touch.clientX - 40}px; top:${touch.clientY - 20}px;`;
    document.body.appendChild(clone);
    chip._ghost = clone;
    chip._answer = chip.dataset.answer;
  }, { passive: false });

  chip.addEventListener('touchmove', (e) => {
    e.preventDefault();
    const touch = e.touches[0];
    if (chip._ghost) {
      chip._ghost.style.left = (touch.clientX - 40) + 'px';
      chip._ghost.style.top = (touch.clientY - 20) + 'px';
    }
    // Highlight zone under finger
    const el = document.elementFromPoint(touch.clientX, touch.clientY);
    zones.forEach(z => z.querySelector('.dnd-zone-target').classList.remove('drag-over'));
    if (el && el.closest('.dnd-zone-target')) {
      el.closest('.dnd-zone-target').classList.add('drag-over');
    }
  }, { passive: false });

  chip.addEventListener('touchend', (e) => {
    if (chip._ghost) { chip._ghost.remove(); chip._ghost = null; }
    const touch = e.changedTouches[0];
    const el = document.elementFromPoint(touch.clientX, touch.clientY);
    if (el && el.closest('.dnd-zone-target')) {
      const target = el.closest('.dnd-zone-target');
      target.textContent = chip.textContent;
      target.dataset.placed = chip._answer;
      chip.classList.add('placed');
    }
  });
});
```

---

## Group Chat Animation

iMessage/WeChat-style chat showing the paper's "actors" talking to each other. Messages appear one by one with typing indicators.

For papers, cast the actors from the research world, e.g.: 研究者 (PI)、统计师、审稿人 (Reviewer 2 走起)、患者代表、HTA 评审员、药厂 Medical、临床医生. Great uses: a "reviewer vs author" exchange to surface limitations, a "statistician explains to the PI" chat to unpack a method, or a "HTA 评审 vs 厂商" chat for economic papers. Keep it rigorous — the fun is in the framing, the content must stay faithful to the paper.

**Wiring:** `main.js` auto-initializes every `.chat-window` on page load. Give each chat window a unique `id`. Control buttons need these classes: `.chat-next-btn`, `.chat-all-btn`, `.chat-reset-btn`. The typing indicator avatar element should have `id="{chatWindowId}-typing-avatar"` or simply be the first `.chat-avatar` inside `.chat-typing`.

**HTML:**
```html
<div class="chat-window" id="chat-module2">
  <div class="chat-messages">
    <div class="chat-message" data-msg="0" data-sender="actor-a" style="display:none">
      <div class="chat-avatar" style="background: var(--color-actor-1)">A</div>
      <div class="chat-bubble">
        <span class="chat-sender" style="color: var(--color-actor-1)">Actor A</span>
        <p>Hey Background, I need the data for this item.</p>
      </div>
    </div>
    <!-- more messages... -->
  </div>

  <div class="chat-typing" id="chat-typing" style="display:none">
    <div class="chat-avatar" id="typing-avatar">?</div>
    <div class="chat-typing-dots">
      <span class="typing-dot"></span>
      <span class="typing-dot"></span>
      <span class="typing-dot"></span>
    </div>
  </div>

  <div class="chat-controls">
    <button class="btn chat-next-btn">Next Message</button>
    <button class="btn chat-all-btn">Play All</button>
    <button class="btn chat-reset-btn">Replay</button>
    <span class="chat-progress"></span>
  </div>
</div>
```

**CSS for typing dots:**
```css
.typing-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--color-text-muted);
  animation: typingBounce 1.4s infinite;
}
.typing-dot:nth-child(2) { animation-delay: 0.2s; }
.typing-dot:nth-child(3) { animation-delay: 0.4s; }
@keyframes typingBounce {
  0%, 60%, 100% { transform: translateY(0); }
  30% { transform: translateY(-6px); }
}
```

---

## Study Flow Animation

Step-by-step visualization of how the study ran — the same `flow-animation` engine, recast for research. User clicks "Next Step" to advance.

Typical paper uses: **试验流程**（筛选 → 随机 → 干预 → 随访 → 分析）、**数据流**（原始数据 → 清洗 → 模型 → 结果）、**模型结构**（健康状态之间的转移，CEA 论文）、**文献筛选**（检索 → 去重 → 初筛 → 全文 → 纳入，Meta 分析的 PRISMA 流程）。Actors are stages or states instead of software components; the moving packet represents patients / records / studies flowing through.

**Wiring:** `main.js` auto-initializes every `.flow-animation` on page load. Pass steps as JSON in `data-steps`. Each step object: `{ highlight: "flow-actor-id", label: "description", packet: true, from: "actor-id-suffix", to: "actor-id-suffix" }`. Actor element IDs must be `flow-actor-1`, `flow-actor-2`, etc. Control buttons need classes `.flow-next-btn` and `.flow-reset-btn`.

> **⚠️ Single quotes in step labels will break parsing.** The `data-steps` attribute is delimited by single quotes (`data-steps='[...]'`), so any single quote inside a label (e.g. `"the user's request"`) will terminate the attribute early and cause `JSON.parse` to fail silently — the entire animation will stop working. Either avoid apostrophes in labels, replace them with `&apos;`, or rewrite the attribute using double-quote delimiters with escaped inner quotes (`data-steps="[{\"label\":\"...\"}]"`).

**HTML:**
```html
<div class="flow-animation" data-steps='[
  {"highlight":"flow-actor-1","label":"User clicks the button"},
  {"highlight":"flow-actor-1","label":"Frontend sends request","packet":true,"from":"actor-1","to":"actor-2"},
  {"highlight":"flow-actor-2","label":"Backend calls the database","packet":true,"from":"actor-2","to":"actor-3"}
]'>
  <div class="flow-actors">
    <div class="flow-actor" id="flow-actor-1">
      <div class="flow-actor-icon">A</div>
      <span>Actor 1</span>
    </div>
    <div class="flow-actor" id="flow-actor-2">
      <div class="flow-actor-icon">B</div>
      <span>Actor 2</span>
    </div>
    <div class="flow-actor" id="flow-actor-3">
      <div class="flow-actor-icon">C</div>
      <span>Actor 3</span>
    </div>
  </div>

  <div class="flow-packet" id="flow-packet"></div>

  <div class="flow-step-label" id="flow-label">Click "Next Step" to begin</div>

  <div class="flow-controls">
    <button class="btn flow-next-btn">Next Step</button>
    <button class="btn flow-reset-btn">Restart</button>
    <span class="flow-progress"></span>
  </div>
</div>
```

**CSS for active actor glow:**
```css
.flow-actor.active {
  box-shadow: 0 0 0 3px var(--color-accent), 0 0 20px rgba(217, 79, 48, 0.2);
  transform: scale(1.05);
  transition: all var(--duration-normal) var(--ease-out);
}
```

---

## Interactive Architecture Diagram

Full-picture diagram where hovering/clicking a component shows a description tooltip. For papers: a CEA model structure (health states + transitions), a trial's arm structure, or the evidence network of a network meta-analysis. Zones group related components (e.g. "干预组 / 对照组", "模型内 / 模型外部输入").

**HTML:**
```html
<div class="arch-diagram">
  <div class="arch-zone arch-zone-browser">
    <h4 class="arch-zone-label">Browser</h4>
    <div class="arch-component" data-desc="Injects UI into the web page, reads DOM, captures user actions"
         onclick="showArchDesc(this)">
      <div class="arch-icon">📄</div>
      <span>Component A</span>
    </div>
    <!-- more components -->
  </div>
  <div class="arch-zone arch-zone-external">
    <h4 class="arch-zone-label">External Services</h4>
    <!-- API cards -->
  </div>
  <div class="arch-description" id="arch-desc">Click any component to learn what it does</div>
</div>
```

---



## Scenario Quiz

如果你是评审专家或临床医生，你会怎么判断？（情境题，附讲解）

Same HTML/CSS/JS pattern as Multiple-Choice Quizzes, but with longer scenario descriptions and more detailed explanations. Wrap each question in a scenario context block:

```html
<div class="scenario-block">
  <div class="scenario-context">
    <span class="scenario-label">场景</span>
    <p>论文报告 HR=0.75，95% CI 为 0.55–1.02，p=0.07。同事说差一点就显著了，基本等于有效。你怎么看？</p>
  </div>
  <!-- quiz-options here -->
</div>
```

Note: scenario quizzes have a defensible best answer (comprehension check). For questions that genuinely have no right answer, use a **Discussion Card** instead (see below).

---

## Callout Boxes

"Aha!" moments — universal methodology insights that transfer beyond this one paper. Max 2 per module.

```html
<div class="callout callout-accent">
  <div class="callout-icon">💡</div>
  <div class="callout-content">
    <strong class="callout-title">举一反三</strong>
    <p>随机化保证的是两组在已知和未知因素上都大体均衡，这就是为什么 RCT 被称为因果推断的金标准。以后看到没有随机化的研究，第一反应就该是：两组人本来就不一样怎么办？</p>
  </div>
</div>
```

**Variants:**
- `callout-accent`: vermillion left border, light accent background (for transferable methodology insights)
- `callout-info`: teal left border, light info background (for "good to know" context: guideline positions, field background)
- `callout-warning`: red left border, light error background (for common misreadings, e.g. "p>0.05 不等于没效果")

---

## Pattern/Feature Cards

Grid of cards highlighting engineering patterns, tech stack components, or key concepts.

```html
<div class="pattern-cards">
  <div class="pattern-card" style="border-top: 3px solid var(--color-actor-1)">
    <div class="pattern-icon" style="background: var(--color-actor-1)">🔄</div>
    <h4 class="pattern-title">Caching</h4>
    <p class="pattern-desc">Store results to avoid redundant work, like keeping leftovers instead of cooking a new meal every time.</p>
  </div>
  <!-- more cards -->
</div>
```

```css
.pattern-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: var(--space-4);
}
.pattern-card {
  background: var(--color-surface);
  border-radius: var(--radius-md);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
  transition: transform var(--duration-normal) var(--ease-out), box-shadow var(--duration-normal);
}
.pattern-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
}
```

---

## Flow Diagrams

**Horizontal flow (desktop):**
```html
<div class="flow-steps">
  <div class="flow-step">
    <div class="flow-step-num">1</div>
    <p>User clicks button</p>
  </div>
  <div class="flow-arrow">→</div>
  <div class="flow-step">
    <div class="flow-step-num">2</div>
    <p>Component A detects click</p>
  </div>
  <div class="flow-arrow">→</div>
  <!-- more steps -->
</div>
```

Arrows rotate to `↓` on mobile via CSS transform.

---


## Glossary Tooltips

The most important accessibility feature. Any technical term in the course text should be wrapped in a tooltip that shows a plain-Chinese definition on hover (desktop) or tap (mobile). The learner never has to leave the page or Google anything.

**Term convention for papers:** first use per module = 中文名 (English original) with tooltip. The tooltip definition gives the plain-Chinese explanation, ideally with a mini-metaphor. Later uses in the same module can drop the English and the tooltip.

**HTML — mark up terms inline:**
```html
<p>统计时采用
  <span class="term" data-definition="Intention-to-treat：只要被随机分到某组，无论后来吃没吃药、退没退出，都按原分组统计。像点名，分到你们班就算你们班的人，转学了也先记在册上。这样保住随机化带来的公平。">意向性分析 (ITT)</span>
  的原则。
</p>
```

**CSS:**
```css
.term {
  border-bottom: 1.5px dashed var(--color-accent-muted);
  cursor: pointer;    /* NOT cursor: help — pointer feels clickable and inviting */
  position: relative;
}
.term:hover, .term.active {
  border-bottom-color: var(--color-accent);
  color: var(--color-accent);
}

/* The tooltip bubble — uses position: fixed and is appended to document.body
   via JS so it is NEVER clipped by ancestor overflow: hidden containers
   (like translation blocks). See JS section below for positioning logic. */
.term-tooltip {
  position: fixed;        /* CRITICAL: fixed, not absolute — prevents clipping */
  background: var(--color-bg-code);
  color: #CDD6F4;
  padding: var(--space-3) var(--space-4);
  border-radius: var(--radius-sm);
  font-size: var(--text-sm);
  font-family: var(--font-body);
  line-height: var(--leading-normal);
  width: max(200px, min(320px, 80vw));
  box-shadow: var(--shadow-lg);
  pointer-events: none;
  opacity: 0;
  transition: opacity var(--duration-fast);
  z-index: 10000;        /* Above everything, including nav */
}
/* Arrow pointing down */
.term-tooltip::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 6px solid transparent;
  border-top-color: var(--color-bg-code);
}
.term-tooltip.visible {
  opacity: 1;
}

/* If tooltip goes off-screen top, flip to below */
.term-tooltip.flip {
  bottom: auto;
  top: calc(100% + 8px);
}
.term-tooltip.flip::after {
  top: auto;
  bottom: 100%;
  border-top-color: transparent;
  border-bottom-color: var(--color-bg-code);
}
```

**JS — position: fixed tooltips appended to body (never clipped by overflow):**
```javascript
// Tooltip container — appended to body so it's never clipped
let activeTooltip = null;

function positionTooltip(term, tip) {
  const rect = term.getBoundingClientRect();
  const tipWidth = 300; // approximate
  let left = rect.left + rect.width / 2 - tipWidth / 2;
  // Clamp to viewport
  left = Math.max(8, Math.min(left, window.innerWidth - tipWidth - 8));

  // Try above first
  let top = rect.top - 8;
  tip.style.left = left + 'px';

  // Position above by default, flip below if no room
  document.body.appendChild(tip);
  const tipHeight = tip.offsetHeight;
  if (rect.top - tipHeight - 8 < 0) {
    // Flip below
    tip.style.top = (rect.bottom + 8) + 'px';
    tip.classList.add('flip');
  } else {
    tip.style.top = (rect.top - tipHeight - 8) + 'px';
    tip.classList.remove('flip');
  }
}

document.querySelectorAll('.term').forEach(term => {
  const tip = document.createElement('span');
  tip.className = 'term-tooltip';
  tip.textContent = term.dataset.definition;

  // Hover for desktop
  term.addEventListener('mouseenter', () => {
    if (activeTooltip && activeTooltip !== tip) {
      activeTooltip.classList.remove('visible');
      activeTooltip.remove();
    }
    positionTooltip(term, tip);
    requestAnimationFrame(() => tip.classList.add('visible'));
    activeTooltip = tip;
  });

  term.addEventListener('mouseleave', () => {
    tip.classList.remove('visible');
    setTimeout(() => { if (!tip.classList.contains('visible')) tip.remove(); }, 150);
    activeTooltip = null;
  });

  // Tap for mobile
  term.addEventListener('click', (e) => {
    e.stopPropagation();
    if (activeTooltip && activeTooltip !== tip) {
      activeTooltip.classList.remove('visible');
      activeTooltip.remove();
    }
    if (tip.classList.contains('visible')) {
      tip.classList.remove('visible');
      tip.remove();
      activeTooltip = null;
    } else {
      positionTooltip(term, tip);
      requestAnimationFrame(() => tip.classList.add('visible'));
      activeTooltip = tip;
    }
  });
});

// Close tooltips when clicking elsewhere
document.addEventListener('click', () => {
  if (activeTooltip) {
    activeTooltip.classList.remove('visible');
    activeTooltip.remove();
    activeTooltip = null;
  }
});
```

**Rules:**
- Mark up EVERY technical term on first use in each module (API, DOM, callback, async, endpoint, middleware, etc.)
- Keep definitions to 1-2 sentences max, in everyday language
- Use a metaphor in the definition when it helps — e.g., "A **callback** is like leaving your phone number at a restaurant so they can call you when your table is ready"
- Don't mark the same term twice within the same screen — only on first appearance per module
- The dashed underline should be subtle enough not to distract but visible enough that curious learners discover it

---


## Icon-Label Rows

For listing components, features, or concepts visually. Replaces bullet-point paragraphs.

```html
<div class="icon-rows">
  <div class="icon-row">
    <div class="icon-circle" style="background: var(--color-actor-1)">🖥️</div>
    <div>
      <strong>Frontend (Next.js)</strong>
      <p>What the user sees and interacts with</p>
    </div>
  </div>
  <div class="icon-row">
    <div class="icon-circle" style="background: var(--color-actor-2)">⚡</div>
    <div>
      <strong>API Routes</strong>
      <p>Backend logic that runs on the server</p>
    </div>
  </div>
  <div class="icon-row">
    <div class="icon-circle" style="background: var(--color-actor-3)">🗄️</div>
    <div>
      <strong>Database (Supabase)</strong>
      <p>Where all the data is stored permanently</p>
    </div>
  </div>
</div>
```

```css
.icon-rows { display: flex; flex-direction: column; gap: var(--space-4); }
.icon-row {
  display: flex; align-items: center; gap: var(--space-4);
  padding: var(--space-4);
  background: var(--color-surface);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
}
.icon-row p { margin: 0; color: var(--color-text-secondary); font-size: var(--text-sm); }
.icon-circle {
  width: 48px; height: 48px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 1.25rem; flex-shrink: 0;
}
```

---

## Numbered Step Cards

For sequences that would otherwise be a numbered paragraph list. Visual, scannable, and each step stands alone.

```html
<div class="step-cards">
  <div class="step-card">
    <div class="step-num">1</div>
    <div class="step-body">
      <strong>User pastes a YouTube URL</strong>
      <p>The frontend captures the URL and extracts the video ID</p>
    </div>
  </div>
  <div class="step-card">
    <div class="step-num">2</div>
    <div class="step-body">
      <strong>API fetches the transcript</strong>
      <p>A server-side route calls an external service to get the video's text</p>
    </div>
  </div>
  <div class="step-card">
    <div class="step-num">3</div>
    <div class="step-body">
      <strong>AI analyzes the content</strong>
      <p>The transcript is sent to an AI model that extracts key moments</p>
    </div>
  </div>
</div>
```

```css
.step-cards { display: flex; flex-direction: column; gap: var(--space-3); }
.step-card {
  display: flex; align-items: flex-start; gap: var(--space-4);
  padding: var(--space-4) var(--space-5);
  background: var(--color-surface);
  border-radius: var(--radius-md);
  border-left: 3px solid var(--color-accent);
  box-shadow: var(--shadow-sm);
}
.step-num {
  width: 32px; height: 32px; border-radius: 50%;
  background: var(--color-accent);
  color: white; font-weight: 700;
  display: flex; align-items: center; justify-content: center;
  font-family: var(--font-display);
  flex-shrink: 0;
}
.step-body p { margin: var(--space-1) 0 0; color: var(--color-text-secondary); font-size: var(--text-sm); }
```

---

## Discussion Cards (Open-Ended Critical Thinking)

**★ Paper-specific. The signature element of the critical-thinking module.** A Socratic prompt with NO right answer. The reader is asked to think first; clicking the toggle reveals *thinking angles* — not conclusions. CSS/JS already in `styles.css`/`main.js` (`toggleDiscussion`).

**Design contract (non-negotiable):**
- The badge always reads 「开放讨论 · 没有标准答案」 — never present angles as "the answer"
- Angles are phrased as questions or considerations ("可以想想…", "注意到…了吗"), never as verdicts ("这是缺陷")
- 3-5 angles per card; at least one angle should *defend* the authors (steelman), so the reader learns balanced critique
- Ground every card in something specific from THIS paper (a choice, a number, an omission) — no generic "样本量够吗" filler

**HTML:**
```html
<div class="discussion-card animate-in">
  <span class="discussion-badge">开放讨论 · 没有标准答案</span>
  <p class="discussion-question">对照组用安慰剂而不是现有标准治疗，你觉得合理吗？</p>
  <p class="discussion-context">论文 p.4 提到，本试验的对照组接受安慰剂 + 最佳支持治疗。而同期已有两种获批药物在临床使用。</p>
  <button class="discussion-toggle" onclick="toggleDiscussion(this)">展开</button>
  <div class="discussion-angles">
    <p class="discussion-angles-label">可以从这些角度想</p>
    <div class="discussion-angle"><span class="discussion-angle-icon">🧭</span><p>如果比现有标准治疗，试验需要更大样本才能显出差异，作者是不是在可行性和临床意义之间做了取舍？</p></div>
    <div class="discussion-angle"><span class="discussion-angle-icon">⚖️</span><p>伦理角度：当已有有效治疗时给患者安慰剂，需要什么条件才说得过去？论文有没有交代？</p></div>
    <div class="discussion-angle"><span class="discussion-angle-icon">🏥</span><p>对决策者：一个胜过安慰剂的结果，能直接回答该不该替代现有药吗？还差哪块证据？</p></div>
    <div class="discussion-angle"><span class="discussion-angle-icon">🤝</span><p>替作者想想：监管注册路径可能就要求安慰剂对照，批评之前，先看这项研究的定位是什么。</p></div>
  </div>
</div>
```

---

## Figure Walkthrough

**★ Paper-specific.** Embeds a real figure extracted from the PDF (screenshot, never redrawn) with numbered "how to read this figure" steps below. CSS already in `styles.css` (`figure-walkthrough`, auto-numbered `.figure-step`).

**Rules:**
- The image is a verbatim crop/render from the PDF, stored in `course-name/assets/` — never redraw or "improve" the authors' figure
- Caption cites the source: figure number + page (`Figure 2 · p.8`)
- Steps teach the reader **how to read this figure type** (axes first, then reference lines, then the punchline), in the order a pro's eyes would move
- 3-6 steps; the last step states what this figure means for the paper's conclusion

**HTML:**
```html
<div class="figure-walkthrough animate-in">
  <div class="figure-frame">
    <img src="assets/fig2-km-curve.png" alt="总生存期的 Kaplan-Meier 曲线">
    <p class="figure-caption">Figure 2 · p.8 · Kaplan-Meier estimates of overall survival</p>
  </div>
  <div class="figure-steps">
    <p class="figure-steps-label">看图步骤</p>
    <div class="figure-step"><p>先看坐标轴：横轴是随机化后的月数，纵轴是还活着的患者比例，曲线只会往下走。</p></div>
    <div class="figure-step"><p>两条线分开的距离就是疗效：分得越开、分开得越早，药效越明显。</p></div>
    <div class="figure-step"><p>看下方的处于风险人数 (No. at Risk)：越到后面人越少，曲线尾巴越不可信，别被末端的大幅波动骗了。</p></div>
    <div class="figure-step"><p>结论：两条曲线在第 6 个月后持续分开，对应正文报告的 HR=0.70，这是支持主要结论的核心证据。</p></div>
  </div>
</div>
```

---

## Briefing Pack

**★ Paper-specific. The final module's centerpiece** — a one-page report kit the reader can reuse at work. Three parts: the one-liner, the 3-minute elevator briefing, and a likely-questions Q&A accordion. CSS/JS already in `styles.css`/`main.js` (`toggleQA`).

**Rules:**
- The one-liner must fit in one breath: 对象 + 干预 + 比较 + 核心结果 + 一个限定词
- Elevator briefing = 5-7 numbered points, each one sentence, in speaking order (背景 → 设计 → 核心结果 → 安全性/成本 → 局限 → 意义)
- Q&A items are the questions a boss/client/reviewer would *actually* ask (3-5 items); each answer gives a response strategy in 2-3 sentences, admitting uncertainty where the paper is genuinely uncertain
- Numbers in the pack must match the paper exactly — this is the part people will quote in meetings

**HTML:**
```html
<div class="briefing-oneliner animate-in">
  <span class="briefing-oneliner-label">一句话总结</span>
  <p class="briefing-oneliner-text">在晚期 XX 癌患者中，A 药相比安慰剂将中位总生存期延长了 4.2 个月（HR 0.70），但尚无与现有标准治疗的头对头比较。</p>
</div>

<div class="briefing-points">
  <div class="briefing-point"><span class="briefing-point-num">1</span><p>背景：晚期 XX 癌二线治疗选择有限，五年生存率不足 10%。</p></div>
  <div class="briefing-point"><span class="briefing-point-num">2</span><p>设计：多中心、双盲、安慰剂对照 III 期 RCT，共入组 480 人。</p></div>
  <div class="briefing-point"><span class="briefing-point-num">3</span><p>核心结果：中位 OS 10.5 vs 6.3 个月，HR 0.70（95% CI 0.55–0.89，p<0.001）。</p></div>
  <div class="briefing-point"><span class="briefing-point-num">4</span><p>局限：对照组为安慰剂而非标准治疗；亚洲人群亚组样本小。</p></div>
  <div class="briefing-point"><span class="briefing-point-num">5</span><p>意义：为同道提供了新选择，但报销/替代决策还需头对头证据。</p></div>
</div>

<div class="qa-list">
  <div class="qa-item">
    <button class="qa-question" onclick="toggleQA(this)">Q：和现在用的标准治疗比，到底好不好？ <span class="qa-chevron">▾</span></button>
    <div class="qa-answer">答：这篇恰恰没比。它只证明比安慰剂好。要回答替代现有药，得等头对头试验或网络 Meta。汇报时把这个边界讲清楚，别让人误以为它赢了标准治疗。</div>
  </div>
  <div class="qa-item">
    <button class="qa-question" onclick="toggleQA(this)">Q：p<0.001 是不是说明效果特别强？ <span class="qa-chevron">▾</span></button>
    <div class="qa-answer">答：p 小只说明差异不像偶然。效果强不强看 HR 和 CI 的宽度。可以补充：样本量、随访成熟度都会影响 p，关键还是临床意义（多活 4 个月对你关心的患者群算不算多）。</div>
  </div>
</div>
```
