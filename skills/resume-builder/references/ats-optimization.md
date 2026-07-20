# ATS Optimization Reference

Load this file at Phase 3 (match analysis). Keep it active through Phase 6 (generation). Defines how ATS systems parse resumes and the rules Claude follows to maximize parse success.

---

## 1. How ATS systems work

Applicant Tracking Systems (Workday, Greenhouse, Lever, iCIMS, Taleo, Jobvite, SmartRecruiters, Ashby, Rippling) do three things:

1. **Parse:** extract text from the submitted file. Parsing accuracy degrades with complex formatting.
2. **Score:** compare extracted text against the JD using keyword matching, sometimes weighted by section (Skills > Summary > Experience > Education) or by term frequency.
3. **Rank:** order candidates by score; surface the top N to a human reviewer.

A resume scoring below the cutoff is never seen by a human — format or keyword gaps cause rejection before any human reads it.

---

## 2. Formatting rules (apply to every generated resume)

### 2a. Layout constraints

- **No multi-column layouts.** ATS parsers read left-to-right and linearize columns, merging a skills column mid-sentence into a job title column.
- **No tables for content.** Tables for decorative alignment sometimes parse; tables containing experience entries or skills frequently break.
- **No text boxes or drawing shapes.** Content in drawing objects is invisible to most ATS parsers.
- **No header/footer regions.** Contact info placed in a Word/Google Docs "header" region is often stripped entirely. Place all contact information in the body.
- **No inline images, icons, profile photos, or logos.** Unreadable to parsers.
- **No special characters as bullet symbols.** Use standard hyphens (`-`) or asterisks (`*`). Unicode bullets (•, ►, ▸, ✓) parse inconsistently across older ATS systems.

### 2b. Section headers

Use standard English phrases that ATS systems are trained to recognize. Non-standard headers cause misclassification of section content.

| Use this | Not this |
|---|---|
| Work Experience | Employment History, Professional Background, Where I've Been, Experience |
| Education | Academic Background, Schooling, Learning |
| Skills | Technical Skills, Core Competencies, Expertise, Proficiencies |
| Projects | Selected Projects, Side Projects, Notable Work |
| Certifications | Credentials, Licenses & Certifications, Accreditations |
| Summary | Professional Profile, About Me, Career Overview, Executive Summary |

In Markdown output: use `##` for section headers. Keep them plain text — no bold, italics, or inline code formatting in section header lines.

### 2c. Date formats

Use consistent formats throughout: `Month YYYY – Month YYYY` or `YYYY – YYYY`. "Present" is safe. Three-letter abbreviations (Jan, Feb) parse correctly. Avoid ambiguous formats:

- `01/2022` — some parsers read as January 2022, others as the 1st of 2022
- `2022–23` — compact ranges parse inconsistently
- `January 1, 2022` — overly verbose; use `January 2022`

### 2d. File format note for user

Markdown is the internal working format. Before submitting to an ATS:
- Convert to `.docx` (Word) for systems that prefer editable files (most enterprise ATS)
- Use clean `.pdf` from a single-column Markdown → PDF export for systems that accept PDF
- Do not upload multi-column HTML-generated PDFs — column layouts break ATS text extraction

---

## 3. Keyword density and placement

### 3a. Density targets

| Keyword type | Minimum appearances | Maximum appearances |
|---|---|---|
| Hard requirement keyword | 1× in body (not just headers) | 4× (one-page) / 6× (two-page) |
| High-frequency JD keyword (3+) | 2× — once in Skills/Summary, once in a bullet | 4× |
| Soft requirement keyword | 1× if genuinely applicable | 3× |

Exceeding the maximum triggers "keyword stuffing" detection in modern ATS systems using TF-IDF or embedding-based scoring.

### 3b. Placement priority

ATS systems typically weight sections in this order:

1. **Skills section** — highest weight; direct keyword match
2. **Job title lines** — company and title are indexed prominently
3. **Summary / Professional Summary** — second keyword anchor
4. **Bullet content in Work Experience** — primary body content
5. **Education / Certifications** — lowest weight for most technical roles

Ensure the most critical hard-requirement keywords appear in Skills and Summary first, then reinforced in bullets.

### 3c. Exact-match vs. synonym tolerance

Most ATS systems do not stem or synonym-match by default:
- `"ML"` and `"machine learning"` score as different tokens
- When the JD uses a full form, use the full form in the resume
- Where space allows, use both: `"Machine Learning (ML)"` on first mention in Skills

**Common pairs — always include both where relevant:**

| Abbreviated | Full form |
|---|---|
| AWS | Amazon Web Services |
| K8s | Kubernetes |
| CI/CD | Continuous Integration / Continuous Deployment |
| NLP | Natural Language Processing |
| ML | Machine Learning |
| AI | Artificial Intelligence |
| IaC | Infrastructure as Code |
| OOP | Object-Oriented Programming |

Preferred pattern: use full form in Skills section, abbreviation in bullets.

---

## 4. Skills section optimization

Structure as a flat list grouped by category. Flat lists parse significantly better than tables or comma-separated paragraphs.

**Recommended grouping order for technical IC roles:**
1. Programming Languages
2. Frameworks & Libraries
3. Cloud & Infrastructure
4. Databases & Storage
5. Tools & Platforms
6. Methodologies

**Within each group:** list JD-matching skills first. This is where ATS keyword scoring is heaviest.

**Capitalization — common mistakes to fix:**

| Correct | Common mistakes |
|---|---|
| JavaScript | Javascript, javascript |
| TypeScript | Typescript, typescript |
| PostgreSQL | Postgresql, postgres, postgreSQL |
| Kubernetes | kubernetes, k8s (unless JD uses k8s) |
| GraphQL | Graphql, graphQL |
| GitHub | Github |
| macOS | MacOS, macos |
| Redis | redis |
| TensorFlow | Tensorflow, tensorflow |
| PyTorch | pytorch |
| scikit-learn | Scikit-Learn, sklearn |
| Next.js | NextJS, Next JS |

---

## 5. ATS anti-patterns — scan base resume before generating

| Anti-pattern | Fix |
|---|---|
| Skills listed as comma-separated prose paragraph | Break into categorized flat list |
| Job title buried in a bullet instead of its own header line | Surface as `### [Title] · [Company]` on its own line |
| Experience years only in the intro paragraph | Reinforce in relevant bullets |
| Skills section shows only broad categories ("Machine Learning", "Cloud") | Break into specific tools ("scikit-learn, XGBoost", "AWS EC2/S3/Lambda") |
| Objective statement instead of Summary | Replace with a role-targeted 2–4 sentence summary |
| Dates formatted inconsistently | Standardize throughout |
| "References available upon request" | Remove — wastes space, ATS ignores it |
| GPA for candidates with 5+ years experience | Remove unless JD explicitly requests it |
| First-person pronouns ("I led", "My projects") | Remove all pronouns from bullets and summary |

---

## 6. ATS validation checklist (run before writing output file)

Before calling `Write`, check every item:

- [ ] All `[HARD]` keywords from Phase 2 appear at least once in the body text (not only in headers)
- [ ] High-frequency keywords (3+ in JD) appear at least twice in the resume
- [ ] No keyword appears more than 4× (one-page) or 6× (two-page)
- [ ] No multi-column layout in the Markdown structure
- [ ] Section headers are plain, standard English
- [ ] No `<!-- TODO -->` left without a corresponding gap flag in the Phase 7 summary
- [ ] No fabricated metrics, titles, employers, or certifications
- [ ] All `<!-- GENERATED -->` items are marked with basis and confidence
- [ ] Dates are consistently formatted throughout
- [ ] Skills section lists specific tools, not just categories
- [ ] No first-person pronouns anywhere in bullet points or summary
