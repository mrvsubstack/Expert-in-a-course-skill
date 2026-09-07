---
name: expert-in-a-course
description: "Design, build, and refine a complete interactive course on any topic — structure, lessons, activities, assessments, learner support, and a live open-source site with source video embedded lesson-by-lesson. Use when building a course, curriculum, tutoring system, or training program."
---

# Expert in a Course

Expert in a Course turns a topic (and, where one exists, a source video/podcast/transcript) into a complete, structured course: aligned goals, a curriculum map, real lessons, practice activities, assessments, and a plan for learner support and improvement — delivered as a live, interactive site, not a static document. It is built to work on ANY subject — a programming language, a soft skill, a hobby, a certification, or a technical discipline.

The skill runs as five phases, always in this order. Each phase produces a concrete artifact that the next phase consumes — never skip a phase or jump straight to writing lesson content.

```
ALIGN → STRUCTURE → CREATE → ASSESS → IMPROVE
```

This loop mirrors agentic-loop design: each phase is a goal with a stopping condition, produces an observable output, and feeds forward into the next phase's input — goal → action → observation → adjustment. Course design and agent design share a discipline: give a learner the smallest set of high-signal material, in the right order, with feedback loops that catch drift before it compounds.

## Phase 1 — ALIGN

Never start writing content before this phase is done. Establish, in this order:

1. **Who is this for.** Get or infer: starting skill level, why they're taking it, how much time per week they realistically have.
2. **What "done" looks like.** Write 3–5 outcome statements: "By the end, the learner can [do X] without [common failure]." Vague outcomes produce a course that never converges.
3. **Scope cuts.** List what this course explicitly will NOT cover, and say so in the description. A course that covers everything teaches nothing well — the smallest high-signal set beats more material.
4. **Delivery constraints.** Format, total length budget, required platform. If the course is built from a source video, podcast, or transcript, ask (or state as an assumption if unattended) whether the learner wants that source embedded lesson-by-lesson — see "Delivery format" below; this determines how Phase 3 gathers timestamps.

Output: a one-paragraph course brief (audience, outcomes, non-goals, format). Do not proceed without it, even as a stated assumption.

## Phase 2 — STRUCTURE

1. **Modules first, lessons second.** A module should leave the learner feeling "I can now do X" — typically 3–5 modules for a short course.
2. **Order by dependency, not topic tidiness.** What must the learner already know before lesson N makes sense?
3. **One idea per lesson.** If the title needs "and," split it. Target 10–20 minutes per lesson.
4. **Name modules/lessons as outcomes, not topics.**
5. **Decide the assessment shape now** — quiz, build task, or review — per module, before writing content toward it.

Output: a module → lesson map (titles + one-line objective per lesson) — the fixed table of contents.

## Phase 3 — CREATE

Write content lesson by lesson against the fixed map — don't silently add/cut lessons; if a gap appears, update Phase 2's map first.

Per lesson, produce:

1. **A short mini-lesson** (2–4 paragraphs), motivation before mechanism.
2. **One worked example** — concrete, not abstract.
3. **One practice activity** — a specific, checkable task, ideally in the same tool the skill will be used in.
4. **A quick knowledge check** (1–3 questions) testing the stated objective.

Constraint rule for any prompt/exercise: it must be complete and runnable as written, use `[BRACKETED VARIABLES]`, and include an explicit constraint that prevents a generic answer. A prompt without a constraint is a one-time demo, not a repeatable skill.

If the course is grounded in a single source (a video, podcast episode, or article) rather than the learner's or Claude's general knowledge, flag which claims are the source's own opinion/synthesis versus established fact, in-line, per lesson — don't let a single interview's framing read as settled consensus.

## Phase 4 — ASSESS

1. **Per-lesson checks** validate recall and immediate application.
2. **Per-module assessment** is heavier — a small project, scenario, or teach-it-back task, not multiple choice.
3. **Write the rubric before the task is taken** — 3–5 criteria with concrete pass bars.
4. **Course-level capstone** combining skills across modules (for multi-module courses).

## Phase 5 — IMPROVE

1. **Instrument before you need it** — decide what signals mean a lesson is broken (drop-off, a commonly-missed check, repeated support questions).
2. **Route feedback to the specific lesson**, not the whole course.
3. **Treat learner questions as signal** for what Phase 2's map missed — recurring questions mean a skipped dependency.
4. **Version the course explicitly** — log what changed and why.

## Output contract

When actually building a course, produce a structured course description (or `course.json` if the destination is a dashboard/app) covering: title, subtitle, audience, outcomes, non-goals, format, modules (each with lessons: objective, content, worked example, activity with prompt, knowledge checks, and a module assessment with rubric), a capstone, resources, an FAQ/support section, and a version note. Keep prose plain and markdown-safe; keep every activity prompt fully copy-pasteable with `[VARIABLES]` and a constraint.

### Delivery format — default to a live, interactive page, not a static document

Unless the learner explicitly wants a plain document (to print, paste elsewhere, or read in an app with no code execution), build the course as a working single-page app, not a markdown file. A markdown file is the fallback, not the default. Treat the course as a small piece of software with a UI, using this checklist:

1. **Data-driven, one page.** Model the whole course as one JS object (modules → lessons → {objective, mini-lesson, example, activity, quiz, assessment}) and render it — don't hand-write repetitive HTML per lesson. This is what makes the page a real "course app" instead of a long scroll.
2. **Dashboard chrome, not a plain document.** Give it: a stats row (module/lesson/activity/resource counts, computed from the data, not hardcoded), a search box that filters lessons live, an expand-all/collapse-all control, and a persistent progress tracker (checkbox per lesson, saved to `localStorage`, with an overall "X / Y lessons" counter). Use an accordion of module cards (click a module header to expand its lessons inline) as the default navigation pattern — it reads as a real product, matches how open-source project/course dashboards are usually laid out, and works on mobile without a separate sidebar breakpoint.
3. **Embed the source, session by session, when there is one.** If the course is grounded in a video/podcast, do not just cite it once at the top. For each lesson, try to find or reasonably estimate the timestamp in the source where that lesson's material is discussed (search a transcript or transcript-summary service for the topic; if only approximate, say so explicitly next to the timestamp rather than presenting it as exact), and give that lesson its own embedded player cued to that moment, with a caption showing the approximate timestamp and a link to open the source directly at that point. Never fabricate a precise timestamp with no basis — an approximate, clearly-labeled one is honest; a confident wrong one is not.
4. **Two builds when the destination is a hosted Artifact.** A platform's own hosted-artifact renderer may block cross-origin iframes (video embeds, maps, etc.) even though a self-hosted copy of the exact same page would render them fine. Build the page once with a single boolean flag controlling embed style (real `<iframe>` vs. a styled "watch at 12:34 ↗" link that opens the source in a new tab), so the standalone/downloadable copy can use real embeds and the hosted-artifact copy can flip the flag rather than needing a second design.
5. **Ship it both ways when asked for "open source," and name the file exactly `index.html`.** If the learner wants something they can host themselves (e.g. GitHub Pages), deliver a complete, self-contained standalone HTML file (own `<!DOCTYPE>`/`<head>`/`<body>`, no external build step, fonts/scripts loaded from public CDNs) as a downloadable file — that's the repo-ready copy — separate from, and in addition to, any live hosted-artifact copy. Tell the learner explicitly, up front, that the file must be uploaded/committed with the exact filename `index.html` at the repo root for GitHub Pages (or any static host) to serve it as the homepage. This matters because it's the single most common failure mode in practice: uploading the same file more than once causes GitHub's web UI to silently rename later copies (`index_2.html`, `index (1).html`, etc.), and a project repo's site also only ever lives at `https://<user>.github.io/<repo-name>/`, never at the bare `https://<user>.github.io/` unless the repo itself is named exactly `<user>.github.io`. Flag both of these proactively rather than waiting for a 404 report.
6. **Design it, don't template it.** Before building, pick a real palette (named hex values), a real type pairing (a display face + body face + a mono/utility face for data and labels), and a layout concept specific to the course's subject — reuse of a generic dashboard skin the learner has already rejected is a design failure, not a content failure. Support both light and dark viewer themes.

## Anti-patterns to refuse or push back on

- Writing lesson content before a brief and module map exist — do Phases 1–2 first, even compressed, and say so.
- A course promising mastery of "everything" in a broad field — force a scope cut in Phase 1.
- Assessments that only test recall of the mini-lesson's wording.
- Prompts/activities without a `[VARIABLE]` and without a constraint.
- Skipping Phase 5 because the course "just launched."
- Defaulting to a static markdown document when the learner wants something they'll actually use as a tool — a course they'll revisit is a small app, not a report.
- Citing a single source video once at the top and calling it "grounded" — when the course is built from one interview/video, ground it lesson-by-lesson with timestamps, not a single link in the header.
- Handing over a standalone HTML file for self-hosting without warning the learner about the `index.html` naming and project-repo-path requirements — silence here is what turns a finished course into a confusing 404.
