---
name: lovstudio-bp
category: Business
tagline: "Evidence → investor-ready BP deck, charts, and review report."
description: >
  Build or improve an investor-ready business plan (BP / pitch deck) from project
  evidence. Produces a source-backed narrative, 12–15 slide outline, professional
  chart plan, PPTX/PDF via lovstudio-any2deck, and a scored review report. Use for
  seed fundraising decks, investor presentations, BP rewrites, pitch-deck audits,
  and financing materials. Trigger on "做 BP", "商业计划书", "融资 PPT", "投资人材料",
  "BP 大纲", "改 BP", "pitch deck", "business plan", "investor deck", or "fundraising deck".
license: MIT
compatibility: >
  Portable Agent Skills format. Python 3.8+ is required for workspace initialization
  and BP auditing. Final PPTX/PDF generation requires lovstudio-any2deck. Current
  market, competitor, policy, and financing claims require access to authoritative
  sources. Brand and output paths come from flags, environment variables, or the
  shared user profile.
depends_on:
  - lovstudio-any2deck
metadata:
  author: lovstudio
  version: "0.1.0"
  tags: business-plan pitch-deck fundraising investor startup evidence charts pptx pdf
---

# BP — Investor-Ready Business Plan

Turn scattered product notes, repositories, metrics, interviews, and founder
context into a concise investor deck that is simple, specific, credible, and
ambitious. This skill owns the investment narrative and quality bar;
`lovstudio-any2deck` owns the final slide rendering and export.

## User Experience Contract

- Start from the user's files, links, screenshots, dashboards, and conversation.
- Prefill known facts before asking anything.
- Ask at most one compact round of questions, and only when the answers materially
  change the deck. If the user says “不要问”“直接做”“按推荐方案”, use the defaults.
- Never invent traction, revenue, customers, testimonials, market size, or quotes.
- Label every uncertain statement as **fact**, **inference**, or **assumption**.
- A completed task includes editable PPTX, PDF, preview/contact sheet, source notes,
  and a BP review report—not merely an outline.

## Defaults

When the user does not specify otherwise:

| Decision | Default |
|---|---|
| Audience | Early-stage investors who may not understand the technology |
| Stage | Seed |
| Length | 12–15 slides, 8–10 minutes |
| Language | Same as the user's language |
| Visual tone | Clean editorial: launch clarity + consulting credibility + startup reality |
| Background | White or warm gray with one brand accent |
| Body size | At least 20 pt |
| Output | PPTX + PDF + full-deck preview + review report |

## Workflow (MANDATORY)

### Step 0: Resolve the skill and user configuration

Resolve this `SKILL.md` directory as `SKILL_DIR`. For manual script execution:

```bash
export SKILL_DIR="/path/to/lovstudio-bp"
```

Resolve workspace, output, brand profile, and design guide in this order:
explicit arguments → skill-specific environment variables → shared profile →
safe current-directory defaults. Read `references/user-config.md`.

### Step 1: Determine the work mode

Choose the smallest mode that fulfills the request:

1. **Build** — create a complete investor BP from source material.
2. **Improve** — diagnose and rewrite an existing BP, preserving useful evidence.
3. **Review** — score an existing outline/deck and produce an actionable report.

Before asking questions, inspect the current directory and user-provided sources.
If essential decisions are still unknown, use `AskUserQuestion` once to collect up
to three items: investor audience/stage, financing ask, and required deliverables.
Skip the question when the user has answered them or requested autonomous execution.

### Step 2: Initialize a portable BP workspace

```bash
python3 "$SKILL_DIR/scripts/init_bp.py" \
  --name "Project Name" \
  --stage seed \
  --output ./business-plan
```

This creates:

```text
business-plan/
├── brief.md
├── evidence-ledger.md
├── outline.md
├── assets/
└── reports/bp-review.md
```

Do not overwrite an existing BP workspace. Continue in it or choose a new output
directory.

### Step 3: Build the evidence ledger before writing slides

Inspect local product docs, repository history, analytics exports, customer notes,
screenshots, financial assumptions, team material, and previous applications. For
time-sensitive or external claims, verify against current primary sources.

Write every material claim into `evidence-ledger.md` with:

- claim;
- status: fact / inference / assumption / missing;
- source URL or local source description;
- as-of date;
- where it will appear;
- verification owner or next action.

Use `references/evidence-and-market.md`. If a dashboard is inaccessible, leave a
clearly named evidence gap; never replace it with a plausible number.

### Step 4: Find the investor-readable “What”

Draft the deck's first-layer product definition before the long vision.

- Chinese: ideally 14–20 characters; English: ideally 8–12 words.
- State what the product is or what job it completes—not merely an aspiration.
- Prefer user value and a recognizable category over implementation details.
- A comparator such as “X for Y” is allowed only when both halves are accurate.
- Separate the early wedge, adjacent users, and long-term market.
- Treat technical modes as mechanisms unless users actually buy that mechanism.

Test three versions: literal category, comparator, and category-creation. Select the
one a non-technical investor can repeat after ten seconds. See
`references/investor-story.md`.

### Step 5: Build the narrative spine

Use the 12-page base sequence below; expand to 13–15 pages only when evidence or
product demonstration needs room:

1. One sentence: who you are.
2. The concrete user problem.
3. How the product solves it.
4. Product demo or core experience.
5. Why now.
6. Real validation / traction.
7. Business model.
8. Market size and wedge.
9. Competition and differentiation.
10. Growth plan.
11. Why this team.
12. Financing ask, use of funds, and next milestones.

Each slide must make exactly one claim. Its title states the conclusion; the chart,
screenshot, workflow, quote, or number proves it. Read
`references/deck-architecture.md`.

### Step 6: Specify professional visuals, not decorations

For every slide in `outline.md`, include:

- conclusion headline;
- one-sentence investor takeaway;
- evidence and source;
- chart/visual type and exact data mapping;
- layout instruction;
- speaker purpose;
- unresolved evidence gap, if any.

Prefer product screenshots, user scenes, business process diagrams, interview
evidence, orders, and simple charts over stock photos. Use real axes, units,
legends, dates, and source notes. Concept charts must say “示意 / illustrative”.
Read `references/charts-and-visuals.md`.

### Step 7: Run the content audit before rendering

```bash
python3 "$SKILL_DIR/scripts/audit_bp.py" \
  --input ./business-plan/outline.md \
  --output ./business-plan/reports/bp-review.md \
  --strict
```

Fix blockers and rerun. Target score: **85+**, with no fabricated data, unlabeled
assumption, missing source for a core number, absent financing ask, or placeholder.
The script is a deterministic lint layer; human/agent judgment still owns the final
investment logic.

### Step 8: Produce the deck through `lovstudio-any2deck`

After the storyline passes review, invoke `lovstudio-any2deck` on `outline.md` with:

- audience: executives / investors;
- 12–15 slides;
- a clean `corporate` or `minimal` base, customized by the brand guide;
- presentation mode where appropriate;
- supplied logos, product screenshots, team photos, and contact QR codes.

Do not delegate the narrative to the rendering skill. The BP outline and evidence
ledger remain the source of truth.

### Step 9: Inspect every rendered page

Render a contact sheet and review slides at presentation size. Check:

- clean hierarchy, alignment, whitespace, and optical logo balance;
- body text ≥20 pt and titles readable in one glance;
- no clipped text, broken CJK, stretched images, or inconsistent branding;
- charts match source data, labels, units, and dates;
- screenshots are genuine and legible;
- QR codes decode to the intended destination;
- cover is simple and the last page has one clear action;
- filenames are normalized and investor-safe.

Update `reports/bp-review.md` with both content and visual findings. Read
`references/review-rubric.md`.

### Step 10: Deliver and report gaps honestly

Return clickable paths to:

- editable PPTX;
- PDF;
- full-deck preview/contact sheet;
- outline;
- evidence ledger;
- final BP review report.

End with at most five unresolved evidence gaps or decisions. Do not bury blockers in
a long process summary.

## Scripts

| Script | Purpose |
|---|---|
| `scripts/init_bp.py` | Create a safe, portable BP workspace from templates |
| `scripts/audit_bp.py` | Score story coverage, evidence hygiene, visual specification, and delivery readiness |

## References

| File | Use |
|---|---|
| `references/investor-story.md` | Positioning, wedge, narrative, and anti-patterns |
| `references/deck-architecture.md` | 12–15 page investor deck architecture |
| `references/evidence-and-market.md` | Evidence ledger and source-backed TAM/SAM/SOM |
| `references/charts-and-visuals.md` | Chart selection and professional visual standards |
| `references/review-rubric.md` | Content and visual acceptance rubric |
| `references/case-yoda.md` | Real Yoda BP iteration case |
| `references/user-config.md` | Portable workspace and brand configuration |

## Non-Negotiables

- Do not fabricate data, users, testimonials, orders, retention, or revenue.
- Do not size a market by copying a broad AI forecast without a bridge to the buyer.
- Do not define the company by a technical implementation users do not buy.
- Do not make every slide a card grid; choose the chart that proves the claim.
- Do not finish at “outline generated” when the request is for a complete BP.
