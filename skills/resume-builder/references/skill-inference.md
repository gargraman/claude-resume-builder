# Skill Inference Reference

Load this file at Phase 4. Defines the taxonomy, confidence tiers, generation rules, hard limits, and adjacency maps for inferring related skills from an existing profile.

---

## 1. Adjacency taxonomy — four relationship types

| Type | Definition | Default confidence |
|---|---|---|
| **Parent/child** | Profile has a general technology; JD requires a specific sub-component of it | HIGH |
| **Co-occurrence** | Tools that are almost always used together in practice; hard to use A without encountering B | HIGH |
| **Sibling** | Different tools solving the same problem in the same ecosystem or language | MEDIUM |
| **Domain transfer** | Real-world experience in a domain implies contextual awareness of related concepts | LOW |

---

## 2. Confidence tiers

### HIGH — auto-include, exact-match language

The inference is nearly certain: using the parent technology necessarily involved exposure to the child, or the two tools are genuinely inseparable in practice.

**Rules:**
- Add to Skills section using the exact JD capitalization of the inferred skill
- May expand an existing experience bullet to reference the inferred skill naturally
- Mark with `<!-- GENERATED: based on <basis> | confidence: HIGH | verify before submitting -->`
- Use direct language: "experience with X", "built with X", "used X in production"

**Examples:**
- Profile: `AWS` → JD: `AWS EC2, S3, Lambda` — can enumerate the specific services as HIGH (using AWS implies touching these)
- Profile: `Kubernetes` → JD: `Docker` — can add Docker at HIGH (Kubernetes requires container images; can't use K8s without Docker or an equivalent)
- Profile: `React` → JD: `JavaScript, HTML, CSS` — HIGH (can't write React without these)
- Profile: `NumPy, pandas` → JD: `scikit-learn` — HIGH (the numerical computing stack directly enables scikit-learn; commonly used together)
- Profile: `Terraform` → JD: `Infrastructure as Code (IaC)` — HIGH (Terraform is IaC)
- Profile: `PostgreSQL` → JD: `SQL` — HIGH (PostgreSQL is a SQL database; using it requires SQL)
- Profile: `GitHub Actions` → JD: `CI/CD` — HIGH (GitHub Actions is a CI/CD platform)
- Profile: `Kafka` → JD: `event-driven architecture` — HIGH (Kafka is the primary tool for event-driven systems)

---

### MEDIUM — auto-include, qualified language

The tools are in the same ecosystem or solve the same problem differently. Genuine exposure is plausible but not guaranteed.

**Rules:**
- Add to Skills with qualified phrasing: "familiar with", "exposure to", "experience with [sibling] workflows"
- Do NOT write a dedicated experience bullet as if the candidate has shipped code with this tool specifically
- Mark with `<!-- GENERATED: based on <basis> | confidence: MEDIUM | verify before submitting -->`
- Use hedged language: "familiar with X patterns", "exposure to X", "background in [parent ecosystem] including X"

**Examples:**
- Profile: `Flask` → JD: `FastAPI` — MEDIUM (same Python async/sync web framework space; different paradigm but same ecosystem)
- Profile: `Docker` → JD: `Kubernetes` — MEDIUM (Docker alone does not imply K8s; orchestration is a separate skill)
- Profile: `REST APIs` → JD: `API design, OpenAPI` — MEDIUM (REST experience implies familiarity with API design principles)
- Profile: `Jenkins` → JD: `GitHub Actions` — MEDIUM (both are CI/CD tools; pipeline concepts transfer)
- Profile: `MySQL` → JD: `PostgreSQL` — MEDIUM (both relational; SQL is transferable, but specific features differ)
- Profile: `Vue.js` → JD: `React` — MEDIUM (both component-based JS frameworks; concepts overlap but syntax differs)
- Profile: `AWS` → JD: `GCP` — MEDIUM (cloud concepts transfer; service names differ)
- Profile: `TensorFlow` → JD: `PyTorch` — MEDIUM (both deep learning frameworks; concepts overlap, APIs differ significantly)
- Profile: `led engineering team` → JD: `mentoring engineers` — MEDIUM (team lead role implies some mentoring)
- Profile: `Celery` → JD: `distributed task queues, async processing` — MEDIUM (Celery is a distributed task queue)

---

### LOW — do not auto-include, present as suggestion

The inference is a stretch or involves domain-level reasoning rather than direct technical adjacency. The candidate must confirm before it goes in the resume.

**Rules:**
- Do NOT write anything into the generated resume
- Present the suggestion to the user with its basis and ask for explicit confirmation
- Format: `[LOW — needs confirmation] <skill>: basis is <adjacent skill>. Should I add "exposure to X" to your Skills section? Confirm y/n.`
- Only after the user says yes: add at MEDIUM confidence language and mark accordingly

**Examples:**
- Profile: `REST APIs` → JD: `GraphQL` — LOW (different query paradigm; exposure is possible but not implied)
- Profile: `Python data analysis` → JD: `Spark, distributed data processing` — LOW (single-machine pandas to distributed is a meaningful gap)
- Profile: `fintech company, 3 years` → JD: `PCI DSS compliance` — LOW (working in fintech implies awareness but not necessarily direct compliance responsibility)
- Profile: `startup, small team` → JD: `hiring, interviewing, performance reviews` — LOW (possible but not guaranteed without explicit mention)
- Profile: `backend engineering` → JD: `mobile development` — LOW (completely different skill; do not infer)
- Profile: `junior data analyst` → JD: `ML model deployment, MLOps` — LOW (data analysis does not imply MLOps without evidence)

---

## 3. What can be generated

### Skills section entries (✓ allowed at HIGH/MEDIUM)
- Add the inferred technology to the appropriate category in Skills
- Use exact JD capitalization
- For MEDIUM: qualify with "familiar with" or "exposure to" in a parenthetical or separate line
- Mark with inline HTML comment

### Bullet point expansions (✓ allowed at HIGH/MEDIUM, with restrictions)
- Can expand an EXISTING bullet that already references the base skill to mention the inferred skill naturally
- The expansion must anchor to a real role, project, or company in the base resume
- Cannot create a standalone new bullet that is entirely generated with no base
- Example: bullet says "built microservices with Docker" → can expand to "built microservices with Docker; familiar with Kubernetes orchestration for container scheduling" at MEDIUM

### Summary mentions (✓ allowed at HIGH for major ecosystem skills)
- A single brief mention in the professional summary if the inferred skill is a high-frequency JD keyword
- Keep to one phrase; do not make it the headline of the summary

---

## 4. What must never be generated

These are absolute prohibitions regardless of confidence:

| Prohibited | Why |
|---|---|
| New job title, employer, or company | Creates a false work history |
| New education entry or degree | Cannot be verified; is fraud on a resume |
| Certifications not held | Employers verify; creates immediate disqualification |
| Numeric metrics (%, $, user counts, team sizes) | Fabricated numbers are the most common red flag; employers ask about them in interviews |
| Leadership claims without explicit base | "Managed a team of 8" with no team-lead mention in base resume |
| Domain expertise claims without real experience | "HIPAA compliance expert" inferred from one healthcare-adjacent project |
| Skills marked as "proficient" or "expert" when basis is MEDIUM | MEDIUM = "familiar with", not "proficient" |

---

## 5. Adjacency maps by ecosystem

Use these tables to quickly classify relationship type. Items in the same row are HIGH co-occurrences or parent/child. Items in the same section are MEDIUM siblings.

### Python ecosystem
| Known in profile | JD requires | Type |
|---|---|---|
| NumPy, pandas | scikit-learn | HIGH |
| NumPy, pandas | matplotlib, seaborn | HIGH |
| Python | pip, virtual environments, pyproject.toml | HIGH |
| FastAPI / Flask | Pydantic, Uvicorn, ASGI | HIGH |
| Celery | Redis, RabbitMQ (as broker) | HIGH |
| Flask | FastAPI | MEDIUM |
| Django | Flask, FastAPI | MEDIUM |
| scikit-learn | XGBoost, LightGBM | MEDIUM |
| TensorFlow | Keras | HIGH |
| TensorFlow | PyTorch | MEDIUM |

### JavaScript / TypeScript ecosystem
| Known in profile | JD requires | Type |
|---|---|---|
| React | JavaScript, HTML, CSS | HIGH |
| React | TypeScript | MEDIUM |
| React | Webpack, Vite | HIGH |
| Next.js | React, Node.js | HIGH |
| Node.js | npm/yarn, Express | HIGH |
| Vue.js | React, Angular | MEDIUM |
| Jest | React Testing Library | HIGH (for React projects) |

### Cloud and infrastructure
| Known in profile | JD requires | Type |
|---|---|---|
| AWS | EC2, S3, Lambda, RDS, VPC | HIGH (enumerate sub-services used) |
| Terraform | Infrastructure as Code | HIGH |
| Kubernetes | Docker, container orchestration | HIGH |
| Docker | containerization, OCI images | HIGH |
| Docker | Kubernetes | MEDIUM |
| AWS | GCP, Azure | MEDIUM |
| GitHub Actions | CI/CD | HIGH |
| Jenkins | GitHub Actions, CircleCI | MEDIUM |

### Data and ML
| Known in profile | JD requires | Type |
|---|---|---|
| Kafka | event-driven architecture, message queues | HIGH |
| Spark | distributed data processing | HIGH |
| Airflow | workflow orchestration, DAGs | HIGH |
| dbt | data transformation, analytics engineering | HIGH |
| PostgreSQL | SQL, relational databases | HIGH |
| PostgreSQL | MySQL | MEDIUM |
| Snowflake | cloud data warehouse | HIGH |
| MLflow | experiment tracking, model registry | HIGH |

### Go ecosystem
| Known in profile | JD requires | Type |
|---|---|---|
| Go | gRPC, Protocol Buffers | HIGH |
| Go | goroutines, channels, concurrency | HIGH |
| Go | net/http | HIGH |
| Go microservices | distributed systems, service mesh | MEDIUM |

### Java / JVM ecosystem
| Known in profile | JD requires | Type |
|---|---|---|
| Java | Maven/Gradle | HIGH |
| Java | JVM, JDK | HIGH |
| Spring Boot | Spring Framework | HIGH |
| Kotlin | Java | HIGH |
| Java | Scala | MEDIUM |

### Observability and reliability
| Known in profile | JD requires | Type |
|---|---|---|
| Datadog | APM, distributed tracing, metrics | HIGH |
| Prometheus | Grafana | HIGH |
| PagerDuty, on-call | incident response, SRE practices | MEDIUM |
| structured logging | observability, log aggregation | MEDIUM |

### Domain transfers (LOW by default — confirm with user)
| Profile domain | JD domain signal | Inferred awareness |
|---|---|---|
| Fintech / payments | PCI DSS, SOC 2, fraud detection | Financial compliance awareness |
| Healthcare / health data | HIPAA, data privacy | Healthcare data regulation awareness |
| Enterprise B2B SaaS | enterprise sales cycles, customer success | Enterprise customer empathy |
| Consumer social app | growth metrics, DAU/MAU, A/B testing | Consumer product analytics |
| Security company | threat modeling, CVE, pentesting | Security mindset |
| Startup (seed–Series B) | 0-to-1 ownership, breadth, ambiguity | Generalist/autonomous work style |
