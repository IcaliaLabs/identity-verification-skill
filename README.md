# Identity Verification Skill for Claude

**An open-source, Claude-powered identity verification system for remote hiring across US and Latin America.**

Built and maintained by [Icalia Labs](https://icalialabs.com). MIT Licensed.

---

## Quick Start

### Option A: Install as a Claude Skill

1. Download the `skill/` folder from this repository
2. In your Claude Project, go to **Project Knowledge** → **Add Content** → upload the three files:
   - `skill/SKILL.md`
   - `skill/references/jurisdiction_rules.md`
   - `skill/references/scheme_patterns.md`
3. Start a new conversation and paste candidate data — the skill auto-detects which stage to run

### Option B: Copy-Paste into Any Claude Conversation

1. Open `skill/SKILL.md` in this repo
2. Copy the **entire file** into a new Claude conversation
3. Then paste your candidate data after it
4. Claude will detect the stage, confirm with you, and run the review

> **Rule:** One candidate per conversation. Start a new chat for the next candidate.

---

## Try It Now

### Stage 1 Example: Pre-Interview Screen (Clean Candidate → Advance)

**What you paste into Claude:**

```
I need to run a Stage 1 pre-interview screen on this candidate.

Candidate name: Ana Lucía Ramírez Vega
Role: Senior Backend Engineer
Claimed location: Guadalajara, Mexico
Email: analucia.ramirez@gmail.com

Resume summary:
- 2024–present: Senior Engineer at Kueski (Guadalajara) — Python/Django, microservices, AWS
- 2021–2024: Backend Developer at Wizeline (Guadalajara) — Node.js, PostgreSQL, Docker
- 2018–2021: Junior Developer at Tec de Monterrey IT Dept (Monterrey) — PHP, MySQL
- B.S. Computer Science, Tec de Monterrey, 2018

LinkedIn: https://linkedin.com/in/analucia-ramirez-vega
- Profile created 2017, 820+ connections
- Current role at Kueski since Jan 2024
- Previous roles match resume
- Professional photo, no AI artifacts

GitHub: https://github.com/analucia-rv
- Account created 2016
- Mix of personal projects (Django REST APIs, a FastAPI template) and older university coursework
- Commit history variable — heavier on weekdays, lighter on weekends
- Languages: Python (primary), JavaScript, some Go

References:
- jorge.martinez@kueski.com (current engineering manager)
- patricia.leon@wizeline.com (previous team lead)
```

**What Claude produces:**

```
Based on the data you've shared (resume, LinkedIn, GitHub, email, references —
no IDs or tax docs), this looks like a Stage 1: Pre-Interview Screen.
I'll review public information for consistency and authenticity signals. Correct?
```

*You confirm, and Claude returns:*

```
Screen decision: Advance

Summary: Strong, consistent candidate. Public footprint across LinkedIn (2017,
820+ connections), GitHub (2016, real commit patterns), and resume all align on
timeline, employers, and location. Both references are direct work emails at
verifiable employer domains.

Positive signals:
1. LinkedIn profile established since 2017 with 820+ connections — consistent
   with 7+ years in industry
2. GitHub account since 2016 with variable commit cadence and mix of real
   projects + older coursework — authentic pattern
3. Both references are direct work emails at real employer domains
   (kueski.com, wizeline.com) — independently verifiable
4. Employers (Kueski, Wizeline, Tec de Monterrey) are well-known, findable,
   Guadalajara-based — consistent with claimed location
5. Career progression (junior → developer → senior) is plausible for the timeline

Concerns:
1. No concerns flagged at this stage.

Probes for the interviewer:
1. "You moved from Monterrey to Guadalajara between your Tec de Monterrey role
    and Wizeline. What drove that move, and when exactly did you relocate?"
2. "Tell me about a production incident you handled at Kueski. Walk me through
    what happened and how you responded."
```

---

### Stage 3 Example: Full Documentation Review (Flagged Candidate → Medium)

**What you paste into Claude:**

```
I need to run a Stage 3 full documentation review on this candidate.

Candidate name: Diego Alejandro Torres Medina
Role: Full Stack Engineer
Employment type: Contractor
Claimed jurisdiction: Bogotá, Colombia

Stage 1 output: Advance with extra probing — LinkedIn profile only 14 months old,
but resume claims 4 years experience. GitHub account created 8 months ago.

Stage 2 interview notes: Video worked fine. Face matched LinkedIn. Answered
technical questions well. Slight hesitation on local context question about
Bogotá neighborhood (Chapinero) — said he "just moved there recently." Timezone
appeared consistent with Colombia.

Government ID: Colombian Cédula de Ciudadanía #1015487623, issued 2019,
address: Cra 7 #45-12, Bogotá

Tax ID: NIT 1015487623-7

Bank statement: Nequi (digital bank), dated April 28, 2026, account holder
"Diego Alejandro Torres Medina", account country Colombia

Proof of address: Claro mobile phone bill dated April 15, 2026, name
"Diego A. Torres", address: Cra 7 #45-12, Apt 301, Bogotá

LinkedIn: https://linkedin.com/in/diego-torres-medina
- Profile created March 2025 (14 months ago)
- 127 connections
- Lists Rappi 2021–2023, Appify SaaS 2023–present
- Professional photo, no obvious AI artifacts

GitHub: https://github.com/diegotorresm
- Account created September 2025 (8 months ago)
- 12 repos, all polished showcase projects
- Commit messages are clean, regular cadence (every 2–3 days)
- Languages: React, Node.js, Python

Payment destination: Nequi Colombia, account name "Diego Alejandro Torres Medina"

References:
- carlos.mejia@gmail.com (claims former Rappi colleague)
- andrea.vargas@gmail.com (claims former university classmate)
```

**What Claude produces:**

```
Based on the data you've shared (cédula, NIT, bank statement, proof of address,
Stage 1+2 notes, payment destination), this looks like a Stage 3: Full
Documentation Review. I'll run the complete verification. Correct?
```

*You confirm, and Claude returns:*

```
Overall risk tier: Medium

Summary: Identity documents are internally consistent and NIT validates, but
several soft signals warrant attention. The candidate has no traditional bank
account (Nequi only), a LinkedIn profile that's only 14 months old despite
claiming 4 years of experience, a GitHub account that's 8 months old with
curated showcase-only repos, and both references are personal Gmail addresses
with no verifiable employer email. No hard flags, but the combination of signals
requires additional documentation before proceeding.

Findings by category:

Completeness:
- Missing: Bank statement from a traditional bank (Bancolombia, Davivienda,
  BBVA Colombia). Nequi is a fintech — insufficient physical-presence signal.
- Missing: Utility bill for proof of address. A Claro mobile phone bill is
  weaker than a fixed-line utility (water, gas, electric).
- RUT not provided — needed for contractor tax compliance in Colombia.

Internal consistency:
- Name on cédula: "Diego Alejandro Torres Medina"
- Name on proof of address (Claro bill): "Diego A. Torres" — abbreviated.
  Minor, but flag for clarification.
- Address on cédula: "Cra 7 #45-12, Bogotá" — no apartment number.
- Address on proof of address: "Cra 7 #45-12, Apt 301, Bogotá" — includes
  apartment. Could be the same building, but the cédula omits Apt 301.
  Request clarification.

Document validation:
- Cédula: 10-digit number (1015487623), plausible for a younger cohort.
  Format valid.
- NIT: 1015487623-7. Applying weights [41,37,29,23,19,17,13,7,3] right-to-left
  to base digits 101548762, sum = 478, 478 mod 11 = 5, check digit = 11-5 = 6.
  However, stated check digit is 7. CHECK DIGIT DOES NOT VALIDATE.
  NOTE: Verify the NIT digits were pasted correctly. If confirmed as provided,
  this is a hard flag.

Right-to-work:
- Colombian cédula + claimed Bogotá residence = consistent for contractor
  engagement.
- RUT not provided — must be requested for tax compliance.

Payment routing:
- Payment to Nequi Colombia, in candidate's legal name, in Colombia. Country
  matches. Name matches.
- However: Nequi is a fintech. Insufficient traditional-bank physical-presence
  signal. Request traditional bank statement.

Public footprint:
- LinkedIn created March 2025 (14 months) but claims 4 years of experience.
  Profile is thin relative to claimed tenure. 127 connections is low for
  someone with 4 years in Bogotá's tech scene.
- GitHub created September 2025 (8 months). All 12 repos are polished showcase
  projects with regular, clean commits — pattern is consistent with a curated
  portfolio, not organic work history.
- Rappi (claimed 2021–2023) is a real, verifiable employer. However, no
  reference from Rappi is provided with an @rappi.com email.
- "Appify SaaS" — not independently verifiable. No web presence found.

Scheme patterns:
- No hard scheme indicators present.
- Soft indicators: LinkedIn < 18 months + GitHub < 12 months + curated repos +
  all-Gmail references + fintech-only banking. Individually these are
  explainable. Together they form a pattern worth probing.

Green flags:
1. NIT base number matches cédula number (same person)
2. Payment destination is in Colombia, in candidate's name
3. Stage 2 interview: video worked, face matched, timezone consistent
4. Cédula format is valid and cohort-plausible

Red flags:
1. NIT check digit may not validate (verify digits — potential hard flag)
2. No traditional bank statement (Nequi only — fintech)
3. LinkedIn profile 14 months old vs. 4 years claimed experience
4. GitHub account 8 months old, all showcase repos
5. Both references are personal Gmail — no employer domain reference
6. Current employer "Appify SaaS" not independently verifiable
7. Proof of address is a mobile phone bill (weaker signal than fixed utility)
8. Name abbreviated on Claro bill vs. full name on cédula

Missing information needed to raise confidence:
1. Traditional bank statement (Bancolombia, Davivienda, or BBVA Colombia)
   < 90 days
2. RUT document
3. Fixed-line utility bill (water, gas, or electric) < 90 days
4. Reference with @rappi.com email to verify claimed employment
5. Clarification on NIT check digit (re-verify the number)
6. Clarification on apartment number discrepancy (cédula vs. Claro bill)

Questions for follow-up interview:
1. "Your LinkedIn profile was created about a year ago, but you've been working
    in tech for four years. Did you have a previous LinkedIn account, or is this
    your first one?"
2. "Can you provide a reference from your time at Rappi with a @rappi.com email
    address?"
3. "You mentioned you recently moved to Chapinero. Where were you living before
    that, and how long have you been at your current address?"
4. "What's the nearest TransMilenio station to your apartment?"
5. "Do you have a bank account at Bancolombia, Davivienda, or another
    traditional bank? We need a statement from a traditional institution."
6. "Can you walk me through what Appify SaaS does and who the founders are?"

Recommendation: Proceed with additional checks

Reasoning: The NIT check digit discrepancy needs immediate clarification — if
the digits were pasted incorrectly, this may resolve to Low tier. If confirmed,
it escalates to High. Independent of that, the combination of fintech-only
banking, thin public footprint (young LinkedIn + young GitHub + curated repos),
all-Gmail references, and an unverifiable current employer creates enough soft
signal density to warrant requesting traditional bank statement, RUT, a Rappi
reference, and a utility bill before proceeding. No hard scheme pattern is
present, but the profile doesn't yet have enough anchor points to clear at Low.
```

---

## Data Format Templates

### Stage 1: Pre-Interview Screen

Copy this, fill in the blanks, paste into Claude:

```
I need to run a Stage 1 pre-interview screen on this candidate.

Candidate name: {{full legal name}}
Role: {{role title}}
Claimed location: {{city, country}}
Email: {{email address}}

Resume summary:
{{paste resume text, or a structured summary of roles, dates, companies}}

LinkedIn: {{URL}}
- Profile created: {{year or "unknown"}}
- Connections: {{approximate count}}
- Key details: {{anything notable}}

GitHub: {{URL or "N/A"}}
- Account created: {{year or "unknown"}}
- Notable repos: {{brief description}}
- Languages: {{primary languages}}

References:
- {{name}} — {{email}} ({{relationship: manager, colleague, etc.}})
- {{name}} — {{email}} ({{relationship}})
```

### Stage 3: Full Documentation Review

Copy this, fill in the blanks, paste into Claude:

```
I need to run a Stage 3 full documentation review on this candidate.

Candidate name: {{full legal name}}
Role: {{role title}}
Employment type: {{employee | contractor}}
Claimed jurisdiction: {{city, country}}

Stage 1 output: {{paste or summarize the Stage 1 results}}

Stage 2 interview notes: {{video on? face match? probe answers? timezone?
hesitations? local context question results?}}

Government ID: {{type (INE/Cédula/DNI/Passport)}}, number {{ID number}},
issued {{year}}, expiration {{year}}, address {{address on ID}}

Tax ID: {{type (RFC/CURP/NIT/CUIT/SSN)}} {{full number with check digit}}

Bank statement: {{bank name}}, dated {{date}}, account holder "{{name on
account}}", account country {{country}}

Proof of address: {{type (utility bill, lease, etc.)}}, dated {{date}},
name "{{name on document}}", address {{address on document}}

LinkedIn: {{URL}}
- {{brief summary: profile age, connections, key observations}}

GitHub: {{URL or "N/A"}}
- {{brief summary: account age, repo types, commit patterns}}

IDV tool output: {{paste results if available, or "not yet run"}}

Payment destination: {{bank name}}, {{account type (CLABE/CBU/routing)}},
account name "{{name}}", country {{country}}

References:
- {{name}} — {{email}} ({{relationship}})
- {{name}} — {{email}} ({{relationship}})
```

---

## Document Checklist by Country

### 🇲🇽 Mexico
- INE (both sides, high-res)
- CURP
- RFC (on Constancia de Situación Fiscal)
- Comprobante de domicilio < 90 days (CFE, Telmex, water, gas)
- CLABE for payment
- Bank statement from traditional bank < 90 days (Banamex, BBVA, Santander, Banorte)

### 🇨🇴 Colombia
- Cédula de ciudadanía (both sides)
- RUT
- Factura de servicios públicos < 90 days
- Colombian bank account number
- Bank statement from traditional bank < 90 days (Bancolombia, Davivienda, BBVA Colombia)

### 🇦🇷 Argentina
- DNI (both sides)
- CUIT or CUIL
- Factura de servicios < 90 days
- CBU for payment
- Bank statement from traditional bank < 90 days (Banco Nación, Santander, BBVA, Galicia)

### 🇺🇸 United States — Employees
- I-9 List A (passport, green card, EAD) OR List B + List C
- SSN
- W-4
- EOR handling I-9 confirmed

### 🇺🇸 United States — Contractors
- W-9 (US tax residents) or W-8BEN (non-US)
- Government photo ID
- Proof of address < 90 days

> **Key rule:** Bank statements must be from a **traditional bank** — not fintechs (Nubank, Wise, Revolut, Mercado Pago, Ualá, Nequi). Traditional banks require in-person onboarding; fintechs can be opened from anywhere.

---

## How It Works

### The Four-Stage Flow

This skill handles Stages 1 and 3 (the Claude-driven reviews). Stages 2 and 4 are human-driven.

```
Stage 1: Pre-Interview Screen (Claude)
  ↓ Advance / Advance with probing / Reject
Stage 2: Interview (Human — video on, face match, probe questions)
  ↓ Advance / Hold / Reject  
Stage 3: Full Documentation Review (Claude)
  ↓ Low / Medium / High / Block
Stage 4: Pre-Start Verification (Human — equipment delivery, IP check, first-week video)
  ↓ Confirm start / Escalate
```

### Risk Tiers (Stage 3)

| Tier | Meaning | Action |
|---|---|---|
| **Low** | Complete, consistent, validated, plausible | Proceed to Stage 4 |
| **Medium** | Minor gaps or soft flags | Request missing items, re-run |
| **High** | Hard flags present | Hold. Escalate. |
| **Block** | Multiple hard flags or clear scheme pattern | Withdraw. Escalate. |

---

## What This Catches

| Check | What It Detects |
|---|---|
| RFC DOB validation | Forged Mexican tax ID |
| CURP check digit | Tampered Mexican identity document |
| CUIT/CUIL check digit | Stolen or fabricated Argentine ID |
| NIT check digit | Invalid Colombian tax ID |
| SSN pattern validation | Invalid US Social Security Number |
| Traditional bank requirement | Remote account opening / no physical presence |
| Account name vs. ID name | Account doesn't belong to the candidate |
| Payment destination country | Location fraud / money laundering |
| LinkedIn profile age | AI-generated or recently purchased profile |
| GitHub commit timezone | DPRK / laptop-farm timezone patterns |
| Interview notes vs. packet | Bait-and-switch or imposter |

---

## What This Does NOT Replace

- **An IDV tool** (Persona, Veriff, Sumsub) for document forensics and biometric matching
- **Form I-9 / E-Verify** for US employees (must be done within 3 days of start)
- **Live interviewer judgment** — the skill generates the briefing, the human decides
- **Sanctions / PEP screening** — add WorldCheck or ComplyAdvantage if needed

---

## Supported Jurisdictions

| Country | IDs Validated | Tax ID Algorithms |
|---|---|---|
| 🇲🇽 **Mexico** | INE, CURP | RFC (13-char + DOB + name derivation), CURP (mod-10 check digit) |
| 🇨🇴 **Colombia** | Cédula, Cédula de Extranjería | NIT (weighted sum mod 11) |
| 🇦🇷 **Argentina** | DNI | CUIT/CUIL (weighted sum mod 11 + prefix validation) |
| 🇺🇸 **United States** | Passport, Driver's License, EAD | SSN (pattern validation), I-9 document classes |

**Want to add a jurisdiction?** See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Known Limitations

1. **Document forgery.** Claude cannot authenticate an ID image. A well-forged document will pass. Use an IDV tool.
2. **Biometric match.** Claude cannot confirm the face on video matches the ID.
3. **Confabulation risk.** Claude can occasionally state a confident-but-wrong fact about a country's ID format. Cross-check hard flags against government sources.
4. **Bias risk.** Voice/accent, name patterns, and regional Spanish judgments are advisory only. A Block decision requires hard flags, never soft signals alone.
5. **Jurisdiction coverage.** Currently US, Mexico, Colombia, Argentina. See [CONTRIBUTING.md](CONTRIBUTING.md) to add more.

---

## Repository Structure

```
identity-verification-skill/
├── README.md                           # This file
├── LICENSE                             # MIT License
├── CONTRIBUTING.md                     # How to add jurisdictions & patterns
├── CHANGELOG.md                        # Version history
│
└── skill/                              # The Claude skill
    ├── SKILL.md                        # Main skill file
    └── references/
        ├── jurisdiction_rules.md       # Tax ID algorithms, doc checklists
        └── scheme_patterns.md          # Fraud patterns, scoring guide
```

---

## Why This Exists

Remote hiring created a new attack surface that traditional HR processes weren't designed to handle. DPRK IT worker schemes, identity-for-hire operations, and location fraud are real, growing, and disproportionately target remote engineering roles because of the high salaries, full system access, and normalized "camera off" culture.

Enterprise IDV tools handle document authentication and biometrics well, but they miss the contextual signals that matter in hiring — timeline inconsistencies, commit timezone patterns, payment routing anomalies, and jurisdiction-specific document validation. This skill fills that gap: a structured verification workflow that uses Claude as a senior analyst to catch what humans miss at speed.

The cost is effectively zero. It's designed for teams hiring 5–50 engineers/year across US and LatAm, where enterprise compliance infrastructure is overkill but the risks are just as real.

*Full backstory and data on remote hiring fraud rates coming soon — blog post in progress.*

---

## Contributing

We welcome contributions — especially new jurisdictions, updated scheme patterns, and validation algorithm corrections. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide.

**Process:** Fork → branch → PR → we review and merge. We control `main` but are open to all well-sourced contributions.

---

## About Icalia Labs

[Icalia Labs](https://icalialabs.com) is a software consultancy specializing in staff augmentation and engineering services across US and Latin America. We built this system internally, drawing on expertise from fintech KYC programs that onboarded 10,000+ consumers across Latin American jurisdictions with direct-to-bank relationships. We open-sourced it because the remote hiring fraud problem affects every company hiring distributed engineering teams.

**Maintainers:** Icalia Labs Engineering & Talent Acquisition teams
**Questions:** [Open an issue](https://github.com/icalialabs/identity-verification-skill/issues)

---

## License

MIT License. See [LICENSE](LICENSE).

---

## Changelog

### v1.0.0 — May 2026
- Initial public release
- Stage 1 (pre-interview screen) + Stage 3 (full documentation review)
- Jurisdictions: Mexico, Colombia, Argentina, United States
- Scheme patterns: DPRK IT workers, identity-for-hire, location fraud
- Tax ID validation: RFC, CURP, CUIT/CUIL, NIT, SSN

---

*Built by [Icalia Labs](https://icalialabs.com). If this helps your team hire safer, [star the repo](https://github.com/icalialabs/identity-verification-skill) and tell others.*
