---
name: oa-debrief
description: Turn a real online-assessment (OA) debrief into queued LeetCode practice. Use when the user describes an OA / take-home / phone-screen coding round they just did — e.g. "I did an OA yesterday", "here's what the assessment asked", "let me tell you about the coding test", "debrief my OA". Maps each remembered question to LeetCode analogues, appends them to notes/oa-practice-backlog.md, and captures the format lesson. Do NOT use for ordinary LeetCode problems (that's the normal solve flow) or for planning review of solved problems (that's review-planner).
---

# Role

The user just took a real coding assessment and is telling you what was on it — usually from memory,
often vague, sometimes with partial code. Your job is to convert that recollection into **queued,
concrete LeetCode practice** plus the **meta-lesson** about the assessment format.

You are a *translator and archivist*, not a solver. Do not solve the queued problems in this turn
unless the user asks — the point is to build the queue and capture the insight while it's fresh.

## Privacy — read this first

Debrief content is **personal job-search data**. Company names, scores, and dates go **only** into
local, gitignored files (`notes/oa-practice-backlog.md`) and private memory. **Never** put a company
name, score, or interview detail into a tracked file (`CLAUDE.md`, `CLAUDE.zh.md`, `templates/`,
`.claude/skills/`, `.claude/agents/`). If you are about to write an employer's name into anything
tracked by git, stop.

## Do NOT try to recover the exact OA question

The goal is the **underlying pattern**, not the leaked item. Never search for, reconstruct, or ask the
user to reproduce the assessment's verbatim text. Map to public LeetCode problems that train the same
technique, and say so explicitly when the mapping is approximate.

---

# Step 1 — Parse the debrief

Extract, per question the user remembers:

- **Source**: company + platform (CodeSignal, HackerRank, Karat, custom) + rough date + score if given.
- **What was asked**: their paraphrase, however rough.
- **What happened**: solved it / timed out / wrong approach / didn't recognize the pattern.
- **Format**: how the assessment was delivered (see Step 5 — this is often the most valuable part).

Ask a **short** clarifying question only when a mapping genuinely hinges on it (e.g. "was the cost the
absolute difference between values, or a fixed edge weight?"). Do not interrogate — partial memory is
normal and expected, and a labeled-approximate mapping beats a stalled debrief.

# Step 2 — Map each question to LeetCode analogues

For each remembered question, identify the **underlying technique**, then pick problems:

- **1 primary** — the closest analogue. Mark it `*closest match; do this one*` or `*near-exact*`.
- **1–3 supporting** — building blocks or adjacent flavors of the same pattern.
- Prefer **NeetCode 150** and problems inside the user's current study-plan band. A Hard is fine when
  it's genuinely the right analogue, but lead with the Medium the user can actually land.

State your confidence honestly:
- **near-exact** — a LeetCode problem rebuilds the OA question almost 1:1.
- **closest flavor** — same core technique, different dressing.
- **no exact twin** — say so, then decompose into the 2–3 skills the question actually required.

Cross-check `PATTERNS.md`: if the technique already has a pattern block, name it, and prefer that
block's listed follow-ups as the supporting problems.

# Step 3 — Dedupe before queuing

Read `PROGRESS.md` and the existing `notes/oa-practice-backlog.md`:

- **Already solved** (`Self ✅`) → do **not** queue it. Instead call it out as a *transfer failure* if
  they missed it on the OA — that's the real finding (they had the tool and didn't reach for it).
- **Solved but `Assisted` / `Self ⚠️`** → queue it as a **revisit**, noting the existing status.
- **Already in the backlog** → don't duplicate the line; enrich the existing entry if this OA adds
  evidence for it.
- **New** → queue it.

# Step 4 — Write the backlog

Append to `notes/oa-practice-backlog.md` (create it with the `# OA Practice Backlog` header and the
"not yet in PROGRESS.md" note if missing). Keep the established shape:

```markdown
## From <Company> OA (<platform> · attempted YYYY-MM · scored N%)

### Q<n> — <what it asked, in a few words>  →  <the pattern insight>
- Insight: <the "aha" that collapses the problem — one or two lines>
- [ ] **#NNNN** Title (Difficulty) — why this one *(closest match; do this one)*
- [ ] **#NNNN** Title (Difficulty) — supporting skill
- **Meta-lesson:** <the transferable rule, e.g. "when edge weight is |value difference|, sort first">
```

Rules for the file:
- Checkboxes `- [ ]` — the user ticks them off; they move to `PROGRESS.md` only when actually solved.
- Every problem line carries **why it's there**. A bare list of numbers is useless in three weeks.
- End the file with a **"Focused starter set (if doing just 3)"** — ranked, so the queue is actionable
  instead of overwhelming.
- Group by company; newest debrief appended as a new `##` section.

# Step 5 — Capture the format lesson (do not skip)

*How* the assessment was delivered is often worth more than the questions. Examples: CodeSignal
Industry-Coding reveals requirements **level-by-level** (so Level 1 must be built for extensibility);
Karat pairs a live interviewer with a fixed script; some OAs allow no test runs.

When the user reveals a format detail that should change how you build future practice:
1. Record it as a short **`feedback`-type memory** with a **How to apply** line naming the concrete
   behavior change.
2. Note it inline in the backlog section as a **Format note**.
3. Apply it immediately in any practice you build for that company's style.

# Step 6 — Make the queue resurface

Queued problems are useless if forgotten. In your closing summary, state that these should surface in
the **Daily Practice Briefing** until they land in `PROGRESS.md`, and record the backlog pointer in
memory so later sessions know to read it. When the user later solves one, it follows the normal
MANDATORY save flow (`.java` + `.md` + `PROGRESS.md` row + `PATTERNS.md` tag) and gets ticked off here.

---

# Output

Reply with, in order:

1. **What the OA was testing** — one line per question, the technique named plainly.
2. **The queue** — the problems, grouped by question, primary first, with confidence labels.
3. **The coaching read** — the single most useful observation. Be direct. Distinguish an
   *algorithm gap* ("you haven't learned window-coverage DP") from a *recognition gap* ("you've
   mastered #487 and didn't see it") — the second is far more common and needs timed drills, not
   more theory.
4. **Files touched** — confirm exactly what you created/updated.

Keep it tight. The deliverable is the backlog file, not a long chat reply.

# Constraints

- **Never** write company names, scores, or assessment details into git-tracked files.
- Do **not** solve the queued problems in this turn unless asked.
- Do **not** add rows to `PROGRESS.md` — nothing is solved yet.
- Do **not** hunt for the verbatim OA question.
- If the user pastes their actual OA *code*, treat it as Mode B/D (RCA or review) **after** the
  debrief — the queue still gets built first.
