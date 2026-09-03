# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **Claude Code plugin** that packages a single skill (`resume-builder`) which tailors a resume to one job description. There is no application code, no build system, no tests, and no package manifest — the entire repo is Markdown skill definitions plus a JSON plugin manifest. "Editing this codebase" means editing prompt/instruction Markdown, not writing software.

There is nothing to build, lint, or run locally. The skill executes inside a Claude Code session that has loaded the plugin (see README.md "Install"). To exercise a change, install the plugin and invoke `/resume-builder` in a Claude Code session.

## Architecture

The skill uses **progressive disclosure**: a thin always-loaded entrypoint plus reference files pulled in only when their phase runs. This keeps the base context small — do not collapse the references back into `SKILL.md`.

- `skills/resume-builder/SKILL.md` — the entrypoint and control flow. Defines a **7-phase pipeline** (Phase 0 Intake → 1 Resume Load → 2 Intelligence → 3 Fit Scoring → 3.5 Gap Discovery → 4 Content Augmentation → 5 Assembly → 6 Delivery). Two intake/output extensions live inside these phases rather than as new phases: **Phase 1 Step 3** merges optional supplementary sources (LinkedIn URL, GitHub profile/repos, pasted notes) into the working profile with source provenance, and **Phase 6 Step 4.5** offers an opt-in short cover letter (ask each run, default No). A top-level **"Voice & review stance"** block sets a blunt, no-sugar-coating reviewer persona threaded through every printed output block. The YAML frontmatter's `description` is the trigger surface (natural-language phrases that auto-invoke the skill) and `allowed-tools` restricts it to `Read Write Bash WebFetch`.
- `skills/resume-builder/references/*.md` — domain logic loaded **exactly once, at the start of a specific phase** (the mapping is the table in `SKILL.md` "Reference files"):
  - `job-analysis.md` → Phase 2 (JD parsing: hard/soft reqs, level detection, ATS keywords, hidden reqs)
  - `ats-optimization.md` → Phase 3 (ATS matching, formatting rules, keyword density)
  - `skill-inference.md` → Phase 4 (adjacency taxonomy, confidence tiers, ecosystem maps)
  - `resume-writing.md` → Phase 5 (bullet formulas, action verbs, section ordering)
- `resume/base-resume-template.md` — user-facing template; copied to `~/resume/base-resume.md`, which is the skill's default input.
- `.claude-plugin/plugin.json` — plugin manifest (name, version, metadata).
- `ROADMAP.md` (repo root) — public, contributor-facing plan of future capabilities benchmarked against 2026 recruiting reality; categorizes what to build vs. what stays prompt-only, and marks items shipped so far. Update it when a roadmap item lands.

Runtime I/O (not in this repo): reads the base resume from `~/resume/base-resume.md` (or a user path); writes output to `./tailored/<company>-<role>-YYYY-MM-DD.md` plus a `-report.md` sidecar in the *user's* working directory. `tailored/` is gitignored.

## Invariants — do not break these when editing

- **Generation limits are the product's core promise.** The skill must never fabricate job titles, employers, education, certifications, or numeric metrics. Inferred/adjacent content must be marked with an inline `<!-- GENERATED: ... -->` comment; uncovered hard requirements get `<!-- TODO: ... -->`. These rules appear in `SKILL.md` ("Content rules"), the README, and the phase logic — keep all copies consistent if you change them.
- **Confidence tiers gate behavior.** HIGH/MEDIUM inferences auto-include; LOW inferences require explicit user confirmation before inclusion. The band definitions (DIRECT/TRANSFERABLE/ADJACENT/WEAK/GAP) in Phase 3 drive routing in Phases 4–5 — a change to one must be reflected downstream.
- **Reference-file phase mapping** in `SKILL.md` must stay in sync with the actual `Load references/...` instruction inside each phase.
- The output block formats (`JD ANALYSIS`, `FIT ASSESSMENT`, `AUGMENTATION RESULTS`, `TAILORING COMPLETE`, etc.) are contracts the README documents; keep them aligned across `SKILL.md`, the reference files, and `README.md`.

## Version sync

`plugin.json` and `SKILL.md` frontmatter must declare the same `version` (both are `2.1.0` as of the multi-source intake + cover-letter pass). A prior 1.0.0/2.0.0 drift was reconciled — keep them in lockstep on any future bump.

## Conventions

- Repo intent is public sharing (MIT, README written for external users). Keep example companies/roles fictional or clearly illustrative.
- Commit style in this repo is short imperative subjects (e.g. "Update plugin.json") — no Jira prefixes or Conventional Commits enforced. Match the existing log.
- Per README "Contributing", opening an issue is expected before changing the core 7-phase workflow or the hard generation limits.
