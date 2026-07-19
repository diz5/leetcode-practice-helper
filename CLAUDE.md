# LeetCode Practice — Instructions for Claude

This folder is a personal LeetCode study workspace. In **other sessions** I will paste a
problem (or just a problem number/name) and ask for the solution. Follow the rules below
exactly so every answer is consistent and good for long-term learning.

> **📌 Bilingual sync rule.** This file (English) is the **authoritative** version that Claude
> loads. `CLAUDE.zh.md` is a Chinese mirror for the user's reading only. **Whenever you edit this
> file, you MUST make the equivalent edit to `CLAUDE.zh.md` in the same turn**, so the two never
> drift. If they ever conflict, this English file wins.

## 🟢 Daily Practice Briefing — MANDATORY on the FIRST reply of every new session
On your **very first reply in a session** in this folder, **before** handling whatever I asked,
output a short **Daily Practice Briefing**, then continue with my actual request in the same reply.
(Only on the first reply of the session — do not repeat it on later turns. If I explicitly say
"skip the briefing", skip it.)

Steps:
1. **Read `PROGRESS.md`** — the source of truth for what I've solved.
2. **Completed by category (total):** a compact per-topic count + the grand total. Where useful,
   note the mastery split (how many `Self ✅` mastered vs `Assisted`).
3. **Completed in the past 7 days:** list problems whose Date is within 7 days of **today** (use
   today's date from the session context), showing each one's status. If none, say so plainly.
4. **Today's recommendation:** based on my study plan (phase heuristic below; full plan in memory
   `leetcode-study-plan`) and my progress **and mastery status**, give:
   - the **focus category** for today (the current phase's topic band), and
   - **3–4 specific real LeetCode problems** (number + title + difficulty). Prefer band problems I have
     **NOT** solved yet (cross-check `PROGRESS.md`), **NeetCode 150**, ramping Easy→Medium. You may also
     resurface a `Self ⚠️` (suboptimal) problem for a revisit when it fits — but never re-recommend a
     `Self ✅` mastered one.
5. Keep the whole briefing **tight** — a few lines + one small table. Then address my request.

**Study-plan phase heuristic** (pick by the current month; full plan in memory `leetcode-study-plan`):
- **Jul–Aug 2026 (Phase 1):** foundations → arrays/hashing → two-pointers → sliding-window → stack.
- **Sep–Oct 2026 (Phase 2):** trees → BFS/DFS → graphs → DP.
- **Nov–Dec 2026 (Phase 3):** timed, company-tagged high-frequency Mediums (25-min limit).

If a problem I then ask about is a real LeetCode problem, the MANDATORY save rule below still applies.

## 🔁 Review Planner — a SEPARATE subagent for re-practice (NOT the Briefing)
A custom subagent lives at `.claude/agents/review-planner.md`. It ranks **already-solved** problems
in `PROGRESS.md` for spaced-repetition **re-practice** and returns a small "today's review set"
(top 3–5, triaged — never the whole log). It is **read-only** and **distinct from the Daily Practice
Briefing**: the Briefing recommends brand-NEW problems; the Review Planner only resurfaces solved
ones. Trigger it by asking e.g. "plan my review for today" / "what should I redo?".

`PROGRESS.md` has a **`Last Reviewed`** column: the date I last re-practiced a problem. It defaults
to the solve **Date** and is bumped to today whenever I redo the problem — that's what the planner's
"time since last practiced" decays against. The planner does **not** write it; it is updated by the
main session when a review is actually completed (see Conventions).

## 📊 Skill Coach — a SEPARATE subagent that evaluates my skill (NOT the Briefing / planner)
A custom subagent lives at `.claude/agents/skill-coach.md`. It assesses **where my skill stands** —
reading `PROGRESS.md` for the quantitative picture (mastery by topic/difficulty, independence rate,
trend) plus a **spot-read of a few actual solutions** for code-quality evidence — then returns a
skill scorecard + prioritized advice and appends a dated snapshot to **`SKILLS.md`**. It is the
third distinct tool: the **Daily Briefing** recommends brand-NEW problems, **review-planner** schedules
re-practice of solved ones, and the **Skill Coach** evaluates and advises. Trigger it by asking e.g.
"evaluate my skill" / "how am I doing?" / "am I ready to move on?". It only ever writes `SKILLS.md`
(never `PROGRESS.md` or solutions).

## ⚠️ MANDATORY — do not skip, even if I only ask for "the answer"
Every problem you help with produces **persistent files**, not just a chat reply. After you
answer, you MUST, in the same turn, without waiting to be asked:
1. **Write** `topic/NNNN-slug.java` — the solution code.
2. **Write** `topic/NNNN-slug.md` — the full writeup (from `templates/writeup-template.md`).
3. **Edit** `PROGRESS.md` — append one row for this problem.
4. **Confirm** at the end of your reply which files you created/updated.
A chat-only answer is an INCOMPLETE answer. If you find yourself about to end the turn without
having called Write/Edit, stop and do the saves first.

## Solution language
- **Java**, always. Use idiomatic, clean, interview-ready Java.
- Class name `Solution`, standard LeetCode method signatures.
- Prefer clarity over cleverness; add brief inline comments on the non-obvious lines.

## Interaction modes — figure out which one I'm in
I won't always ask the same way. Detect the mode from my message and respond accordingly.

**Mode A — Question only.**
I paste just the problem (or its number/name) and want the solution.
→ Respond with the full Answer Format below.

**Mode B — Question + my wrong answer (RCA mode).**
I paste the problem *and* my attempt that failed or is wrong.
→ **Before** giving the correct solution, do a root-cause analysis:
  1. **What's wrong** — the specific bug/flaw, quoting the exact line(s).
  2. **Why it fails** — a concrete failing input and what my code does vs. should do (trace it).
  3. **Root cause** — the underlying misconception or pattern I missed (this is the real lesson).
  4. **Minimal fix** — can my own approach be salvaged? Show the smallest change that makes it correct,
     if my approach is viable at all.
  5. **Then** the full Answer Format for the clean optimal solution.
Be direct about the mistake — that's the point — but focus on the *lesson*, not just the patch.

**Mode S — "I solved it myself" (self-solved / mastery review).**
I tell you I resolved a problem on my own (usually pasting my code, sometimes just naming the problem).
I understand it — I only want you to **check whether I used the best approach**, and to **record that I
mastered it**. This is the key signal for my mastery tracking.
→ Do a focused review (a lighter Mode D):
  1. **Correctness** — quick sanity check; flag any real bug or missed edge case, else confirm it's right.
  2. **Best approach?** — clearly label: **optimal / near-optimal / suboptimal**. If suboptimal, name the
     better approach + its complexity + what to change. If optimal, just confirm (no long walkthrough —
     I already understand it).
→ Then **record the mastery status** in `PROGRESS.md` (see Status column):
  - `Self ✅` if I used the optimal / near-optimal approach.
  - `Self ⚠️` if my approach works but is suboptimal (so it can be resurfaced for revisit).
→ Save **my** solution as the `.java` (lightly cleaned only if needed) + the `.md` writeup, noting it was
  self-solved. If the problem is already in `PROGRESS.md`, **update** its Status row (and set its
  `Last Reviewed` to today) instead of adding a duplicate. Keep the review concise — mastery-marking is
  the goal, not re-teaching.

**Mode D — Question + my working answer, review it (review mode).**
I paste the problem *and* a solution I believe is correct, and I want you to review/explain it.
Distinguish from Mode B: here my code works (or I think it does) — I want validation, not a bug fix.
→ Respond with:
  1. **Correctness** — is it actually right? Verify the logic; call out any bug, edge case it misses,
     or hidden assumption. If it's correct, say so plainly.
  2. **Complexity** — the true time/space of *my* code (not the ideal), with justification.
  3. **Is it the best?** — compare against the optimal. State clearly: optimal / near-optimal /
     suboptimal. If suboptimal, name the better approach and its complexity, and what specifically
     I'd change. If already optimal, confirm it and note any minor style/readability improvements.
  4. **Walkthrough** — explain how *my* solution works, section by section, so I understand my own
     code deeply (this is the point — I asked you to explain *my* answer).
  5. Only write the full separate optimal solution to files if mine is suboptimal or buggy; if mine
     is already good, save **my** solution (lightly cleaned if needed) as the `.java`, and note in the
     writeup that it was my own accepted solution.

**Mode C — Multiple valid approaches (e.g. BFS vs DFS, DP vs greedy).**
When a problem has several legitimate solutions:
→ **Lead with the single best resolution** and solve it fully (Answer Format).
→ Briefly say **why it's the best** here (complexity, clarity, or constraints), and
  **name the main alternative(s) in one line each** with their trade-off — so I know they exist and
  when they'd win — without solving them in full. Only expand an alternative if I ask.

These modes compose: I might paste a wrong answer *and* the problem has multiple approaches — do the
RCA, then give the best resolution.

**Answer-only input — identify the problem first.**
Sometimes (Mode B or D) I'll paste **just a solution, with no problem statement.** Before analyzing:
1. **Try to identify the LeetCode problem** from the code — method signature, class/var names, the
   algorithm, and any telltale constants (e.g. `1_000_000_007`, board/grid, `TreeNode`). If you can
   name it **confidently** (title + number), state your identification in one line and proceed with
   the review/RCA as normal.
2. **If you are NOT confident** which problem it is (or it could be several), **do not guess** — stop
   and **ask me to paste the problem statement.** A wrong guess wastes the whole answer.
3. If confident on the *approach* but unsure of the exact LeetCode number, say so, solve/review
   against the problem as you understand it, but flag the assumption so I can correct it.
Always confirm the identification before writing files, so we don't file it under the wrong number/topic.

## Quick reference / language questions (NOT a LeetCode problem)
Sometimes I'll ask a small language/syntax/API question instead of a problem — e.g.
"Java char[] to String", "how to sort a Map by value in Java", "Python defaultdict", "reverse a list".
Detect these (no problem number/statement; it's a how-do-I / syntax question) and handle them lightly:
- **Do NOT** use the full teaching format, create `NNNN-*` files, or touch `PROGRESS.md`.
- Give a **short, direct answer**: the idiomatic snippet + one or two lines on when/why + the common gotcha.
  Show more than one idiom only if they have meaningfully different trade-offs.
- Default language is **Java** (my main study language); if I name another (Python, etc.), use that.
- **Do** append it to a running cheatsheet so I can review later: add an entry to
  `notes/<lang>-cheatsheet.md` (e.g. `notes/java-cheatsheet.md`) — create the file if missing, and
  group entries by a `## Topic` heading (Strings, Collections, Sorting, Streams, ...). Keep each entry
  tight: the question as a sub-heading, the snippet, and the one-line gotcha. Tell me you saved it.
- If a "quick" question is really a full algorithm problem in disguise, treat it as a LeetCode problem.

## Answer format (Full Teaching Mode)
When I ask a question, structure your **chat reply** with these sections, in order:

1. **Problem restated** — one or two sentences in plain language, plus constraints that matter.
2. **Brute force idea** — the naive approach and its complexity (even if we won't use it).
3. **Key insight** — the "aha" that unlocks the optimal solution. When you're solving something I don't
   already know, **always illustrate the insight with a small, concrete worked example** (a real input
   with specific numbers) that makes it click — the example must *show why* the insight holds, not just
   restate it abstractly.
4. **Optimal approach (step by step)** — walk the algorithm concretely, ideally with a small example.
5. **Code** — full Java `Solution`, commented.
6. **Complexity** — time and space, with a one-line justification for each.
7. **Edge cases** — inputs that break naive attempts (empty, single element, overflow, duplicates, etc.).
8. **Common pitfalls** — mistakes people make on this specific problem.

Keep it rigorous but readable. Show the reasoning, don't just dump code.

## Saving the work (do this every time, without being asked)
For each problem, create **two files** in the matching topic folder:

- `NNNN-slug.java`  — the `Solution` code (NNNN = zero-padded problem number, e.g. `0001-two-sum.java`)
- `NNNN-slug.md`    — the full writeup, using `templates/writeup-template.md`

Pick the topic folder by the problem's primary pattern. Current topics:

```
arrays/        two-pointers/   sliding-window/  hashing/
strings/       linked-list/    stack-queue/     trees/
graphs/        dp/             greedy/          binary-search/
backtracking/  heap/           math/            bit-manipulation/
intervals/     design/         misc/
```

**The public tool ships NO topic folders — they are created on your machine as you practice.**
Before saving a problem, check whether the target `topic/` folder exists locally and **create it if
it doesn't**, then write `NNNN-slug.java` + `NNNN-slug.md` into it. (A fresh clone of the tool has
none of these category folders until you start solving — and your solutions are gitignored/local-only.)

If none fit, create a new sensibly-named topic folder (kebab-case) and use it.
If a problem spans several patterns, file it under the one the *optimal* solution uses,
and mention the cross-references in the writeup.

## Conventions
- File naming: `NNNN-kebab-case-title` (LeetCode number + slug). Example: `0200-number-of-islands`.
- Always include the LeetCode URL and difficulty at the top of the writeup.
- Update `PROGRESS.md` in the repo root: append a row (number, title, topic, difficulty, **status**, date,
  **last reviewed**). On a first solve, set **Last Reviewed = Date**.
  Status values: `Self ✅` (self-solved, optimal — mastered), `Self ⚠️` (self-solved but suboptimal —
  revisit), `Assisted` (I provided/fixed it). Default to `Assisted` for Modes A/B/C; use `Self ✅`/`Self ⚠️`
  only for Mode S. If a problem's row already exists, **update its status in place** — don't duplicate the row
  (e.g. an `Assisted` problem I later re-solve myself becomes `Self ✅`). **When I re-practice an existing
  problem, also set its `Last Reviewed` to today** (this is what resets the review planner's decay clock).
- If I got a problem before (file already exists), don't silently overwrite — mention it and ask
  whether to add an alternate approach or replace.

## When I paste only a number or a vague name
Confirm the exact problem (title + LeetCode number) before solving, so we don't solve the wrong one.
