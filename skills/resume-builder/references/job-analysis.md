# Job Description Analysis — Extraction Guide

Load this file at Phase 2. Defines the methodology for parsing a job description into structured data used for matching and generation.

---

## 1. Signal taxonomy

Every requirement in a JD belongs to one of four categories. Classify each before proceeding.

| Category | Markers | Treatment |
|---|---|---|
| **Hard requirement `[HARD]`** | "required", "must have", "you must", "X+ years of", listed in "Qualifications" or "Requirements" without a preferred qualifier, repeated throughout the JD | Disqualifying if absent — must appear in resume body |
| **Soft requirement `[SOFT]`** | "preferred", "nice to have", "bonus", "plus if you have", "ideally", "a plus", "we'd love if" | Strengthens match if present; not fatal if absent |
| **Responsibility signal** | "you will", "you'll", "the role involves", "day-to-day", "key responsibilities" | Informs bullet emphasis and section ordering; not a direct requirement |
| **Culture / values signal** | "we move fast", "high autonomy", "customer-obsessed", "data-driven", "wear many hats", "ambiguous environment" | Informs tone, summary framing, and which soft skills to surface |

### Parsing order
1. Scan title block → opening overview paragraph → responsibilities → required qualifications → preferred qualifications → about the company / culture section
2. Items in "Required Qualifications" are `[HARD]` unless explicitly prefixed with "preferred" language
3. Items in "Preferred Qualifications" are `[SOFT]`
4. Items mentioned only in responsibilities (not qualifications) are responsibility signals — use to tune bullet emphasis; do not mark as hard requirements
5. When the same requirement appears in both sections: treat as `[HARD]`

---

## 2. Seniority level detection

### Explicit signals (read from title first)
- Title words: Junior / Associate → early career (0–3 yrs)
- Mid-level (unlabeled) → 2–5 yrs
- Senior → 5–8 yrs
- Staff / Principal → 8–12 yrs, org-level impact
- Distinguished / Fellow → 12+ yrs, industry-level impact
- VP / Director / Head of → management track
- Years-of-experience ranges: "2+ years" (mid), "5+ years" (senior), "8+ years" (staff+)

### Implicit signals — ownership language

| Language in JD | Implied level |
|---|---|
| "Support", "assist", "contribute to", "help the team" | Junior / Mid IC |
| "Own", "lead", "drive", "responsible for", "accountable for" | Senior IC |
| "Define", "set direction", "shape strategy", "partner with leadership" | Staff / Principal IC |
| "Manage team", "hire", "grow engineers", "performance reviews" | Engineering Manager |
| "Org-wide", "cross-org", "company-wide", "executive stakeholders", "C-suite" | Director+ or Staff+ IC |

### Cross-functional and mentoring signals
- Explicit mentoring / coaching mentions → Senior or above
- "Work cross-functionally with product, design, data, legal" → Senior+ IC
- "Influence without authority" → Principal / Staff IC or Director

---

## 3. ATS keyword extraction

Extract every keyword into a flat list. Do not skip "obvious" ones — ATS systems tokenize everything.

### Five keyword categories

1. **Technologies:** languages (Python, Go, Java, TypeScript, Rust), frameworks (React, Django, FastAPI, Spring Boot), platforms (AWS, GCP, Azure, Kubernetes), databases (PostgreSQL, DynamoDB, Redis, Snowflake), tools (Terraform, GitHub Actions, Datadog, PagerDuty)
2. **Methodologies:** Agile, Scrum, Kanban, CI/CD, TDD, BDD, SRE, DevOps, MLOps, GitFlow
3. **Domain terms:** distributed systems, microservices, event-driven architecture, real-time data, ML inference, recommendation systems, payments, fraud detection, fintech, healthtech, developer tools
4. **Certifications and credentials:** AWS Certified Solutions Architect, CPA, PMP, CISSP, GCP Professional Cloud Architect
5. **Soft-skill keywords ATS systems filter on:** "collaboration", "cross-functional", "data-driven", "customer focus", "ownership", "accountability"

### Capitalization rule
Preserve the exact capitalization from the JD. "kubernetes" and "Kubernetes" are different tokens to case-sensitive parsers. When the JD uses inconsistent capitalization, use the most common form.

### Frequency weighting
Count how many times each keyword appears across the entire JD:
- **3+ appearances** → high-frequency keyword; inject at least twice in the output resume (once in Skills, once in a bullet or summary)
- **1–2 appearances** → standard keyword; ensure it appears at least once

---

## 4. Hidden requirement detection

Hidden requirements are not in the qualifications section but are implied by context. Check for these patterns:

| JD pattern | Hidden requirement implied |
|---|---|
| Startup / Series A–C, small team, "fast-paced environment" | Comfort with ambiguity, breadth over depth, self-directed without process, 0-to-1 capability |
| FAANG / Big Tech, "technical excellence", "bar-raiser" | Rigor in code review, strong systems design, mentoring culture, scalability awareness |
| "Customer-facing", "partner with customers", "enterprise accounts" | External communication skills, executive presence, professional writing |
| "High-growth", "hypergrowth", "scaling the team" | Experience scaling systems or teams through rapid growth |
| Regulated industry (finance, healthcare, legal, defense) | Compliance awareness, data privacy, auditability, security mindset |
| "On-call rotation", "incident response", "5 nines", "SLO/SLA" | Operational ownership — not just feature development |
| "Wear many hats", "generalist", "full-stack mindset" | Breadth expected; narrow specialization is a negative signal |
| "Greenfield", "0-to-1", "build from scratch", "founding engineer" | Experience bootstrapping; making foundational architecture decisions |
| "Data-driven", "experimentation", "A/B testing", "metrics-informed" | Comfort defining and measuring success quantitatively |
| "Distributed team", "async communication", "remote-first" | Strong written communication, self-management, documentation culture |

---

## 5. Company and industry context

Extract from the JD and (if URL was provided) the company overview:

- **Industry:** fintech, healthtech, enterprise SaaS, consumer social, marketplace, developer tools, defense, climate tech, etc.
- **Company stage:** seed / Series A / B / C, growth stage, pre-IPO, public company
- **Engineering size signals:** "small engineering team" / "hundreds of engineers" / "thousands of engineers"
- **Product type:** B2B / B2C / B2B2C, API-first platform, marketplace, mobile-first

These affect how the professional summary frames the candidate's experience. A candidate from a 500-person growth startup should be framed differently for a seed company vs. a FAANG role.

---

## 6. Output format for Phase 2

Produce this block verbatim at the end of Phase 2:

```
JD ANALYSIS
───────────
Role: [Exact title] at [Company] ([Inferred level, e.g., "Senior IC"])
Industry/Stage: [e.g., "B2B SaaS, Series C, ~200 engineers"]

Hard requirements [HARD]:
  - [requirement 1]
  - [requirement 2]

Soft requirements [SOFT]:
  - [requirement 1]
  - [requirement 2]

ATS keywords (N total):
  [comma-separated, exact capitalization from JD]

High-frequency keywords (3+ appearances — inject ≥2× in resume):
  [list]

Seniority signals: [1–2 sentences citing specific ownership language]
Culture signals: [1–2 sentences]

Hidden requirements:
  - [hidden req 1]
  - [hidden req 2, or "none identified"]
```
