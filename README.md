# Expert in a Course

A [Claude](https://claude.ai) skill for designing, building, and refining a complete interactive course on any topic — structure, lessons, activities, assessments, learner support, and (optionally) a live, self-hostable site with a source video embedded lesson-by-lesson.

Works on any subject: a programming language, a soft skill, a hobby, a certification, or a technical discipline.

## What it does

The skill runs a fixed five-phase loop, in order, so Claude never jumps straight to writing lesson content before the course actually has a shape:

```
ALIGN → STRUCTURE → CREATE → ASSESS → IMPROVE
```

- **ALIGN** — pins down the audience, outcomes, scope cuts, and delivery format before anything is written.
- **STRUCTURE** — builds the module → lesson map (the fixed table of contents) ordered by dependency, not topic tidiness.
- **CREATE** — writes each lesson against the map: a mini-lesson, a worked example, a practice activity, and a knowledge check.
- **ASSESS** — designs per-lesson checks, per-module assessments with rubrics, and a course-level capstone.
- **IMPROVE** — plans how the course gets better after launch: what signals mean a lesson is broken, and how feedback routes back to a specific lesson.

By default, the finished course ships as a working single-page app (data-driven, with a progress tracker, search, and an accordion-style dashboard) rather than a static document — and when asked for something "open source," it produces a self-contained `index.html` that's ready to drop into a GitHub Pages repo.

## Installation

Copy the `expert-in-a-course` folder into your Claude skills directory:

```
your-project/.claude/skills/expert-in-a-course/SKILL.md
```

Or, if you're using this with a tool that supports Claude skills/plugins generally, point it at this repo's `expert-in-a-course/` folder.

## Usage

Once installed, just ask Claude to build a course:

> "Build me a course that teaches beginners how to use Python for data analysis."

> "Turn this podcast transcript into a self-paced course, embedded lesson by lesson."

Claude will walk through ALIGN → STRUCTURE → CREATE → ASSESS → IMPROVE and, unless you ask for a plain document, hand back a working course site.

## License

MIT — see [LICENSE](LICENSE).

## Contributing

Issues and pull requests welcome. If you use this skill to build a course and find gaps in the phases or anti-patterns, open an issue.
