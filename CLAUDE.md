# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **docs-only repository** that defines the internal training path for new hires at [Radicle](https://www.radicle.it), an Italian Oracle-focused software company. New hires are called **Radiclers**. There is no source code, no build, no tests, no package manifest. All "work" here is editing Markdown.

The repo's primary job is onboarding: tell a new Radicler which Oracle certifications to earn and by when. Public/external readability is a secondary concern.

## Repository structure

The structure is small but each directory has a specific role:

- **`README.md`** is the onboarding entry point. It must stay minimal: welcome, MyLearn registration step, and pointers into `levels/`. Detailed content belongs in level pages, not here.
- **`levels/`** holds one page per training level (`level-1.md`, future `level-2.md`, ...). Each page follows the same template: (1) a roadmap-style title image from `images/` placed immediately under the H1, (2) a short intro, (3) a certification table with columns `# | Certification | Exam code | Deadline | Course on Oracle MyLearn`, (4) a Notes section. Deadlines are **relative to the Radicler's start date**, not absolute. When adding a new level, follow `levels/level-1.md` as the template and add a one-line pointer in the README.
- **`planning/`** holds **archived** syllabus-design notes from before the certification-path approach. The three files form a chain: `01-original-brief.md` (the user's initial brief), `02-brief-review.md` (a critical analysis of the brief), `03-taxonomy-v2.md` (a revised 21-domain taxonomy responding to the analysis). They are kept aside and intentionally preserved as-is. **Do not edit them** unless the user explicitly asks. The 21-domain plan (`docs/`, `labs/`, `exercises/`) they describe is **not** the current direction.
- **`images/`** holds visuals referenced from level pages (relative path from a level page: `../images/<file>`).

The numeric prefix in `planning/` (`01-`, `02-`, `03-`) reflects the chain order above. Preserve it if adding more historical notes.

## Writing conventions

- **Content language**: public docs (`README.md`, `levels/`) are in **English**, even when the user prompts in Italian. The archived files in `planning/` stay in **Italian** (their original language); preserve Italian if asked to polish or rewrite them.
- **No em-dashes (`—`) in prose.** User preference. Use commas, periods, colons, semicolons, or parentheses instead.
- **No emoji** unless the user explicitly asks.
- **Headings** use sentence-case with a colon for two-part titles (e.g. `Step 1: Register on Oracle MyLearn`, `Level 1: Foundations`), not dashes.
- **Oracle MyLearn course links** in level pages use the label `[Open on MyLearn](url)`; if a link is a known temporary placeholder for a future course (e.g. APEX 2026 pointing to the 2025 version), label it `[Open on MyLearn (<note>)](url)` and explain the placeholder in the Notes section below the table.

## Git workflow

- `main` is the working branch; commits land directly. There is no PR template or CI.
- Use `git mv` when renaming so renames are detected (the `planning/` files were renamed this way).
- Commit subjects in this repo are short, sentence-case, present-imperative ("Add Level 1 certification path and rename planning docs"). Match that style.

## When in doubt

- The current direction is the **certification path** described in `README.md` + `levels/`. The 21-domain syllabus in `planning/` is a parallel earlier plan that may be revived later; treat it as reference, not as the current spec.
- If a request touches level deadlines, exam codes, or course links, check `levels/level-1.md` first since it is the source of truth for Level 1.
