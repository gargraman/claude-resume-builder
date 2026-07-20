# Resume Writing Reference

Load this file at Phase 6 (generation). Contains section ordering logic, bullet formulas, action verb lists, summary writing rules, and quantification guidelines.

---

## 1. Section ordering

Section order signals to both ATS systems and human reviewers what the candidate considers most relevant to this role.

### For individual contributor technical roles (SWE, DE, ML, DevOps, SRE, Security, Data)
1. Header (name, contact, LinkedIn, GitHub)
2. **Skills** — front-loads ATS keywords; human reviewer sees competencies immediately
3. Work Experience (reverse chronological)
4. Projects (only if directly relevant to the JD; omit if experience section is strong)
5. Education
6. Certifications (only if JD-relevant)

### For leadership and management roles (EM, Director, VP, Head of, Staff/Principal IC with org-wide scope)
1. Header
2. **Professional Summary** — establishes leadership scope and domain immediately
3. Work Experience
4. Skills
5. Education
6. Certifications

### For career changers or early career candidates
1. Header
2. **Professional Summary** — bridges from previous background to target role
3. Projects or Relevant Coursework (if strongest signal)
4. Work Experience
5. Skills
6. Education

**Rule of thumb:** lead with the section providing the strongest match signal for this specific JD.

---

## 2. Bullet point formulas

Every experience bullet must follow one formula. Never write bullets as prose paragraphs. Never use "Responsible for" or "Helped with" or "Worked on".

### 2a. XYZ formula (preferred — Google-style)

> **Accomplished [X] as measured by [Y], by doing [Z].**

- "Reduced API p99 latency from 800ms to 120ms by implementing a Redis caching layer and query index optimization, improving checkout conversion rate by 4%."
- "Led migration of a 2M-user monolith to microservices, cutting deployment frequency from monthly to daily and reducing incident MTTR by 60%."
- "Automated infrastructure provisioning with Terraform, reducing new environment setup time from 2 days to under 30 minutes."

The metric `[Y]` is the anchor. If no metric exists in the base resume, use a qualitative outcome: "enabling the team to ship 2× faster" or "adopted as the org-wide standard for new services".

### 2b. STAR formula (for complex narratives that need brief context)

Compress into a single sentence:

> **[Context/scale] — [what was needed] — [what you did] — [outcome].**

- "Faced with a 3× traffic spike during a product launch (10M users over 48 hrs), re-architected the job queue system using Celery and Redis to handle burst load, eliminating 99.9% of timeout errors with zero downtime."

### 2c. CAR formula (concise — for older roles or less complex contributions)

> **[Brief challenge], [action taken], achieving [result].**

- "Inherited a Python 2.7 codebase with no tests; introduced pytest, achieved 72% coverage, and unblocked the Python 3.11 migration within 6 weeks."
- "Detected memory leak causing weekly outages; profiled with py-spy, patched the connection pool logic, eliminating the issue in production."

### Formula selection guide

| Situation | Formula |
|---|---|
| Clear, specific metric available | XYZ |
| Complex incident or turnaround with context | STAR |
| Short and punchy; older role; simple fix | CAR |
| No metric but clear outcome | CAR or XYZ with qualitative [Y] |

---

## 3. Action verb list — open every bullet with one

Never repeat the same verb twice in the same role section. Never use: "Responsible for", "Helped with", "Worked on", "Assisted in", "Participated in".

**Engineering & Architecture**
Architected, Designed, Built, Implemented, Developed, Engineered, Refactored, Migrated, Deployed, Scaled, Optimized, Automated, Integrated, Containerized, Shipped, Rewrote, Instrumented, Profiled

**Leadership & Influence**
Led, Managed, Directed, Mentored, Coached, Championed, Established, Drove, Spearheaded, Pioneered, Initiated, Advocated, Established, Defined, Partnered

**Analysis & Data**
Analyzed, Modeled, Instrumented, Benchmarked, Profiled, Diagnosed, Investigated, Audited, Evaluated, Designed, Queried

**Process & Quality**
Standardized, Streamlined, Consolidated, Simplified, Enforced, Introduced, Established, Improved, Reduced

**Outcome-oriented (for summary bullets)**
Reduced, Improved, Increased, Accelerated, Eliminated, Saved, Delivered, Launched, Grew, Achieved, Exceeded, Enabled

---

## 4. Professional summary — sentence-by-sentence structure

The summary is 2–4 sentences. It is the only section written in prose. Must be role-specific — rewrite from scratch for each JD, not copied from the base resume.

**Sentence 1 — Who you are:**
Seniority + domain + years of experience. No personal pronouns.
- "Senior software engineer with 8 years building high-throughput distributed systems at growth-stage fintech companies."
- "Staff infrastructure engineer with 11 years owning platform reliability and developer productivity at B2B SaaS companies."

**Sentence 2 — Most relevant context for this JD:**
Most recent or most relevant company, product type, or technical scope.
- "Most recently at Stripe, led the core billing infrastructure team owning the payment processing critical path for $50B+ in annual volume."
- "Previously at Datadog, built the metrics ingestion pipeline serving 400B data points per day at 99.99% availability."

**Sentence 3 — Top ATS keywords injected naturally:**
2–3 high-frequency JD keywords + what the candidate brings to this specific role.
- "Deep expertise in Go, Kubernetes, and event-driven architecture; brings a track record of reducing system latency while scaling to 10× load."

**Sentence 4 (optional) — Direct value-add for this employer:**
Close with the specific thing this employer gets.
- "Excited to apply that foundation to Brex's real-time transaction processing infrastructure."

### Forbidden phrases — never use in summaries

results-driven, motivated, passionate, dynamic, synergy, detail-oriented, hard-working, go-getter, team player (as a standalone claim), seasoned professional, proven track record (as an opener), seeking a challenging role, dedicated to excellence.

These are ATS and recruiter red flags indicating an unedited template resume.

---

## 5. Quantification rules

Metrics make bullets credible and memorable. They also stand out in ATS keyword scoring.

**Use metrics from the base resume:** if numbers exist, use them exactly.

**Allowable qualitative metrics (when hard numbers are unavailable):**
- Scale: "serving 3 enterprise clients", "used by the entire 200-person engineering org"
- Time: "delivered in 6 weeks", "completed within a single sprint"
- Comparative: "2× faster than the previous solution", "reduced from 3 manual steps to 1"
- Adoption: "adopted company-wide", "now the default library for all new services"
- Scope: "production system", "customer-facing API", "mission-critical path"

**Never fabricate:**
- Percentage improvements you cannot recall or verify
- Revenue or ARR figures not stated in the base resume
- Team sizes you are uncertain of
- User counts or traffic figures not mentioned in the base resume

For generated bullets (from Phase 4 inference): use qualitative framing only. Mark with `<!-- GENERATED -->`. Do not assign any numeric metrics.

---

## 6. Reframing patterns — common transformations

Use these patterns to inject JD keywords naturally without fabricating experience.

| Base resume language | JD requirement | Reframe example |
|---|---|---|
| "Built internal tooling" | "Developer productivity" | "Built internal developer productivity tooling adopted by the 12-person backend team, reducing deployment prep time by 40%" |
| "Worked with microservices" | "Microservices architecture" | "Designed and maintained microservices architecture for the payments domain, owning 4 services in production" |
| "Used AWS" | "AWS (EC2, S3, Lambda, RDS)" | "Deployed and operated infrastructure on AWS (EC2, S3, Lambda, RDS), reducing hosting costs 30% by rightsizing instances" |
| "Wrote documentation" | "Technical communication" | "Authored architecture decision records and runbooks; reduced new engineer onboarding time from 3 weeks to 1 week" |
| "Worked on improving performance" | "Latency optimization, SLOs" | "Profiled and optimized hot paths against a 150ms P99 SLO, achieving 80ms P99 across the customer-facing API" |
| "Helped migrate the database" | "Zero-downtime migration" | "Led zero-downtime migration from self-hosted PostgreSQL to AWS RDS Aurora, serving 1.2M daily active users without service interruption" |
| "Reviewed pull requests" | "Code review, technical mentorship" | "Established code review standards for the team; mentored 2 junior engineers through their first production contributions" |
| "Used CI/CD" | "CI/CD pipelines, GitHub Actions" | "Designed and maintained GitHub Actions CI/CD pipelines, cutting average build time from 18 minutes to 6 minutes" |

The reframe must be accurate. Only apply if the base resume actually contains the experience being reframed. Never add detail that is not grounded in the original.
