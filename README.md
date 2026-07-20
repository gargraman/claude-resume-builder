# claude-resume-builder-skill

A [Claude Code](https://claude.com/claude-code) plugin that tailors your resume for a specific job description — with ATS keyword optimization, adjacent skill inference, gap analysis, and professional rewriting. Built to be shared publicly.

> **Hard limits:** This skill rewrites language, reorders sections, and infers adjacent skills — it does not fabricate job history, education, certifications, or numeric metrics. Review every `<!-- GENERATED -->` and `<!-- TODO -->` comment before submitting to any employer.

---

## What it does

**7-phase workflow:**

| Phase | What happens |
|---|---|
| 0 — Ingest JD | Accept JD as pasted text, local file, URL, or plain-language description |
| 1 — Load resume | Read `~/resume/base-resume.md` or a custom path (.md, .txt, .pdf, .docx) |
| 2 — Analyze JD | Extract hard/soft requirements, ATS keywords, seniority signals, culture flags |
| 3 — Match | Compare resume vs JD; classify each gap as direct or inference-eligible |
| 4 — Infer adjacent skills | Generate plausible additions for related skills; HIGH/MEDIUM auto-include; LOW requires confirmation |
| 5 — Enrich profile | Derive latent expertise from co-occurrences, domain context, and company stage |
| 6 — Generate resume | Produce full tailored Markdown with section reordering, bullet rewriting, and ATS compliance |
| 7 — Write output | Save to `./tailored/<company>-<role>-YYYY-MM-DD.md`; print ATS score + change log |

### What gets optimized
- ATS keyword coverage — every hard requirement must appear in the body
- Section ordering tuned to role type (IC technical, leadership, career changer)
- Bullets rewritten using XYZ / STAR / CAR formulas with strong action verbs
- Professional summary scoped to this specific company and role
- Skills section restructured to front-load JD-matching technologies
- Adjacent skills inferred from what you already know (marked transparently)

### What it will not do
- Create new job titles, employers, education entries, or certifications
- Fabricate numeric metrics (%, $, team sizes, user counts)
- Submit anything on your behalf
- Include LOW-confidence inferences without your explicit confirmation

---

## Install

### Global (available in all Claude Code projects)

```bash
git clone https://github.com/gargraman/claude-resume-builder-skill.git \
  ~/.claude/plugins/local/claude-resume-builder-skill
```

Then add to `~/.claude/settings.json`:

```json
{
  "plugins": [
    "~/.claude/plugins/local/claude-resume-builder-skill"
  ]
}
```

### Per-project (only available in one project)

From your project root:

```bash
mkdir -p .claude/plugins
git clone https://github.com/gargraman/claude-resume-builder-skill.git \
  .claude/plugins/claude-resume-builder-skill
```

Then add to `.claude/settings.json` in the project root:

```json
{
  "plugins": [
    ".claude/plugins/claude-resume-builder-skill"
  ]
}
```

### Verify

In Claude Code, type `/` — you should see `resume-builder` in the list. Or say *"tailor my resume for this job"* — Claude will invoke it automatically.

---

## Set up your base resume

```bash
mkdir -p ~/resume
cp resume/base-resume-template.md ~/resume/base-resume.md
# Edit ~/resume/base-resume.md with your own information
```

The skill reads `~/resume/base-resume.md` by default. You can override with any supported format:

| Format | How it's read |
|---|---|
| `.md` / `.txt` | Read directly |
| `.pdf` | Read natively — no extra tools needed |
| `.docx` | Extracted via `pandoc` (preferred) or `docx2txt` — install either one |

If neither pandoc nor docx2txt is installed and you have a `.docx` resume, the skill will tell you exactly what to install (`brew install pandoc` on macOS) or offer to accept pasted content instead.

You can also paste your resume content directly in the conversation — no file required.

---

## Usage

### Paste a job description directly

```
/resume-builder

Senior Software Engineer — Payments Infrastructure
Company: Stripe | Remote (US)

We're looking for a senior engineer to join our Payments Infrastructure team...
[paste the full JD here]
```

### Provide a local file path

```
/resume-builder ~/Downloads/stripe-senior-swe-jd.txt
```

### Provide a URL

```
/resume-builder https://www.linkedin.com/jobs/view/1234567890/
```

Note: LinkedIn often blocks automated fetches. If it fails, the skill will ask you to paste the JD directly.

### Describe the role in plain language

```
/resume-builder Staff engineer at Notion, infra team, 8+ years required, Go and Kubernetes, distributed systems, remote-first
```

---

## Output

After running, you get:

**1. Tailored resume saved to:**
```
./tailored/stripe-senior-software-engineer-2026-07-20.md
```

**2. In-conversation summary:**
```
RESUME TAILORING COMPLETE
═════════════════════════
Output: ./tailored/stripe-senior-software-engineer-2026-07-20.md

ATS Keyword Coverage
────────────────────
Hard requirements matched: 8/10 (80%)
Soft requirements matched: 4/5 (80%)

Original Changes (from base resume)
────────────────────────────────────
Summary: rewrote to lead with payments/infrastructure; injected Go, Kubernetes, high-throughput
Skills: front-loaded Go, PostgreSQL, Kubernetes, gRPC; moved JavaScript lower
[Stripe] billing bullet: reframed "worked on billing API" → "owned core billing API serving..."
Compressed [older role] to 2 bullets — not relevant to payments infra

Generated Additions (review before submitting)
───────────────────────────────────────────────
[HIGH] "Protocol Buffers" added to Skills — based on gRPC experience; verify you've used them
[MEDIUM] Expanded [Company] bullet to mention distributed tracing familiarity — based on Datadog usage
[LOW — not auto-included] "exposure to Kafka": confirm y/n before I add it

Gaps (address in cover letter or TODO comments)
────────────────────────────────────────────────
<!-- TODO --> PCI DSS compliance: no coverage in base resume — mention if relevant in cover letter

Next Steps
──────────
1. Open ./tailored/stripe-senior-software-engineer-2026-07-20.md
2. Grep for GENERATED and TODO — review every comment
3. Verify all metrics are accurate before submitting
4. Convert to .docx or clean PDF before uploading to ATS
```

---

## Directory layout

```
.claude-plugin/
  plugin.json                        Plugin manifest

skills/
  resume-builder/
    SKILL.md                         7-phase workflow — the core skill definition
    references/
      job-analysis.md                JD parsing: hard/soft reqs, level detection, ATS keywords, hidden reqs
      ats-optimization.md            ATS behavior, formatting rules, keyword density, anti-patterns, validation checklist
      skill-inference.md             Adjacent skill inference: adjacency taxonomy, confidence tiers, generation rules, ecosystem maps
      resume-writing.md              Bullet formulas (XYZ/STAR/CAR), action verbs, section ordering, summary structure

resume/
  base-resume-template.md            Generic Markdown resume template — copy to ~/resume/base-resume.md

README.md                            This file
```

---

## Contributing

PRs are welcome. Good areas to contribute:

- **New ecosystem adjacency maps** in `references/skill-inference.md` (mobile, security, product management, design, data science, DevRel, sales engineering)
- **Role-specific templates** in `resume/`: `base-resume-template-manager.md`, `base-resume-template-early-career.md`, `base-resume-template-data-scientist.md`
- **Reframing pattern examples** in `references/resume-writing.md` for more domain combinations
- **ATS system profiles** — behavior differences across Greenhouse, Workday, Lever, iCIMS, Taleo
- **Bug reports** — edge cases in JD parsing, inference misclassifications, formatting issues

Open an issue before changing the core 7-phase workflow or the hard generation limits.

---

## License

MIT

---

## Disclaimer

Not affiliated with Anthropic. This skill rewrites resume language and may infer adjacent skills — every claim in the final output remains your responsibility to verify. Review all `<!-- GENERATED -->` and `<!-- TODO -->` comments before submitting to any employer. The presence of a keyword in your resume does not guarantee an interview.
