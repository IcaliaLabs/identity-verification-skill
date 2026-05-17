# Identity Verification Skill for Claude

**An open-source, Claude-powered identity verification system for remote hiring across US and Latin America.**

Built and maintained by [Icalia Labs](https://icalialabs.com). MIT Licensed.

---

## Why This Exists

Remote hiring is broken in ways most companies don't discover until it's too late.

The shift to distributed engineering teams — accelerated by the pandemic and now permanent — created a new attack surface that traditional HR processes were never designed to handle. When you hire someone you've never met in person, in a jurisdiction you may not fully understand, you're trusting a stack of digital documents and a video call to confirm that this person is who they claim to be, where they claim to be, and legally authorized to work there.

That trust is being exploited at scale.

### The problem is real and growing

**DPRK IT worker schemes** are the highest-profile example. North Korean operatives, working through intermediaries in China, Russia, and Southeast Asia, use stolen or purchased identities to get hired as remote software engineers at Western companies. The FBI, Treasury Department, and Mandiant have published extensive advisories documenting the pattern: curated but shallow LinkedIn profiles, template resumes with unverifiable employers, payment routing through third-country intermediaries, and "laptop farms" in US states where company equipment is received by confederates. The revenue funds weapons programs. The access creates espionage risk. And the companies often don't find out for months.

But DPRK is just the tip. **Identity-for-hire operations** — where real documents from one person are fronted by a different person on video calls — are a growing cottage industry targeting remote-first companies. **Location fraud** — where a contractor claims to be in Mexico but is actually in a different country with different tax, labor, and sanctions implications — is even more common and harder to detect.

**The numbers tell the story:**
- The FBI's Internet Crime Complaint Center (IC3) reported over $12.5 billion in losses from business email compromise and identity fraud in 2023, with remote hiring fraud as a growing subcategory.
- A 2024 report from the Identity Theft Resource Center found that business identity compromise incidents increased 41% year-over-year.
- Mandiant's 2024 threat research identified DPRK IT workers operating across dozens of Fortune 500 companies, with individual operatives earning $300K+/year in salary that was funneled back to Pyongyang.
- Remote hiring fraud disproportionately targets engineering roles because of the high salaries, full system access, and the normalization of "camera off" culture in technical teams.

### Why existing tools aren't enough

Enterprise IDV platforms (Persona, Veriff, Sumsub) handle document authentication, liveness checks, and biometric matching well. But they don't catch the **contextual** fraud signals that matter in hiring: timeline inconsistencies across a resume and LinkedIn profile, GitHub commit patterns that suggest a different timezone than claimed, payment routing to a country that doesn't match the stated residence, or a bank statement from a fintech that can be opened from anywhere.

Those signals require cross-referencing multiple data sources, understanding jurisdiction-specific document formats, and knowing what "normal" looks like for a software engineer in Mexico City vs. Bogotá vs. Buenos Aires vs. Austin.

That's what this skill does.

### What we built

A structured verification workflow that uses Claude as a senior analyst — not a replacement for human judgment, but a force multiplier that catches the signals humans miss when candidate packets get thrown together at speed.

The system runs at two stages of the hiring pipeline:
- **Stage 1 (Pre-Interview):** Before you invest interviewer time, a lightweight OSINT review of public information (resume, LinkedIn, GitHub, email, references) to catch obvious fakes and generate probe questions for the interview.
- **Stage 3 (Post-Offer Documentation):** After the offer is issued and documents are collected, a comprehensive review of IDs, tax documentation, bank statements, proof of address, and all prior stage outputs against jurisdiction-specific validation rules and known fraud patterns.

The cost is effectively zero (Claude conversation + your team's time). It's designed for small-to-mid teams hiring 5–50 engineers per year across US and LatAm, where enterprise compliance infrastructure is overkill but the risks are just as real.

---

## Supported Jurisdictions

| Country | IDs Validated | Tax ID Algorithms | Bank Statement Rules |
|---|---|---|---|
| 🇲🇽 **Mexico** | INE, CURP | RFC (13-char + DOB + name derivation), CURP (mod-10 check digit) | Traditional bank required (not Nubank, Mercado Pago, Ualá) |
| 🇨🇴 **Colombia** | Cédula, Cédula de Extranjería | NIT (weighted sum mod 11) | Traditional bank required (not Nequi, Daviplata as sole) |
| 🇦🇷 **Argentina** | DNI | CUIT/CUIL (weighted sum mod 11 + prefix validation) | Traditional bank required (not Ualá, Mercado Pago as sole) |
| 🇺🇸 **United States** | Passport, Driver's License, EAD | SSN (pattern validation), I-9 document classes | W-9 vs. W-8BEN validation |

**Want to add a jurisdiction?** See [Contributing](#contributing).

---

## Quick Start

### Install the Skill

1. Download the `skill/` folder from this repository.
2. Add it to your Claude Project or Claude workspace as a skill.
3. Start a new conversation and paste candidate data — the skill auto-detects which stage to run.

### Or Just Copy the Prompts

If you don't use Claude's skill system, you can copy the prompts directly:
- **Stage 1 prompt:** The pre-interview screen section in `skill/SKILL.md`
- **Stage 3 prompt:** The full documentation review section in `skill/SKILL.md`
- **Jurisdiction rules:** `skill/references/jurisdiction_rules.md`
- **Scheme patterns:** `skill/references/scheme_patterns.md`

Copy the relevant sections into any Claude conversation, replace the placeholders, paste your candidate data, and run.

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

### Stage 1: Pre-Interview Screen

**Input:** Resume, LinkedIn, GitHub, email, claimed location, references.
**Output:** Advance / Advance with extra probing / Reject at screen + probe questions for interviewer.

The skill checks:
- Resume plausibility (fake employers, impossible timelines, template writing)
- LinkedIn consistency (profile age, connection count, photo authenticity)
- GitHub authenticity (account age, commit cadence, real work vs. showcase)
- Email + reference verification (throwaway domains, employer email validation)
- Cross-source consistency (name, dates, locations across all sources)
- Jurisdiction plausibility (activity patterns vs. claimed location)

### Stage 3: Full Documentation Review

**Input:** Full candidate packet (IDs, tax docs, bank statements, proof of address, interview notes, all prior outputs).
**Output:** Risk tier (Low / Medium / High / Block) + detailed findings by category.

The skill runs:
1. **Completeness audit** — flags every missing document for the jurisdiction
2. **Internal consistency** — cross-references names, DOBs, addresses across all documents
3. **Document validation** — runs check-digit algorithms on tax IDs (RFC, CURP, CUIT/CUIL, NIT, SSN)
4. **Right-to-work** — validates I-9 docs, W-9/W-8BEN, visa status
5. **Payment routing** — confirms payment goes to the right country, right name, traditional bank
6. **Public footprint / OSINT** — LinkedIn age, GitHub patterns, general web presence
7. **Scheme pattern check** — DPRK indicators, identity-for-hire, location fraud signals
8. **Interviewer brief** — generates probe questions if Medium or High tier

### Risk Tiers

| Tier | Meaning | Action |
|---|---|---|
| **Low** | Complete, consistent, validated, plausible | Proceed |
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

- **An IDV tool** (Persona, Veriff, Sumsub). Claude cannot authenticate a document image or run a liveness check. Pair this with an IDV tool at Stage 3 for document forensics and biometric matching.
- **Form I-9 / E-Verify** for US employees. Must be done by the employer or EOR within 3 business days of start.
- **Live interviewer judgment.** This skill generates the briefing and probe questions — the human makes the call.
- **Sanctions / PEP screening.** Not covered. Add WorldCheck, ComplyAdvantage, or Sumsub screening if needed.

---

## Repository Structure

```
identity-verification-skill/
├── README.md                    # This file
├── LICENSE                      # MIT License
├── CONTRIBUTING.md              # How to contribute
├── CHANGELOG.md                 # Version history
│
└── skill/                       # The Claude skill
    ├── SKILL.md                 # Main skill (258 lines)
    └── references/
        ├── jurisdiction_rules.md   # Tax ID algorithms, doc checklists (234 lines)
        └── scheme_patterns.md      # Fraud patterns, scoring guide (105 lines)
```

---

## Known Limitations

1. **Document forgery.** Claude cannot authenticate an ID image. A well-forged, internally-consistent document will pass. Use an IDV tool.
2. **Biometric match.** Claude cannot confirm the face on video matches the ID. That's the interviewer's job or an IDV liveness check.
3. **Confabulation risk.** Claude can occasionally state a confident-but-wrong fact about a country's ID format. Cross-check hard flags against the reference rules or government sources for the first few runs.
4. **Bias risk.** Voice/accent mismatch, name-pattern heuristics, and regional Spanish judgments are noisy. These are advisory only. A Block decision requires corroborating hard flags, never soft signals alone.
5. **Jurisdiction coverage.** Currently US, Mexico, Colombia, Argentina only. See [Contributing](#contributing) to add more.

---

## Contributing

We welcome contributions. The most valuable additions are:

### Adding a New Jurisdiction

1. Create a new section in `skill/references/jurisdiction_rules.md` following the existing format:
   - Document checklist (what IDs, tax docs, bank statements are required)
   - Tax ID validation algorithm (format, check digits, edge cases)
   - Traditional vs. fintech bank guidance for that country
   - Right-to-work requirements
2. Submit a PR with the jurisdiction name in the title (e.g., "Add Brazil jurisdiction")
3. Include sources for any validation algorithms (government websites, official documentation)

### Improving Scheme Patterns

- New fraud patterns documented by FBI, Treasury, Mandiant, or other credible sources
- Updated DPRK indicators as the threat evolves
- New location fraud signals specific to emerging markets

### Improving Validation Rules

- Corrections to existing check-digit algorithms
- Edge cases we missed (compound names, special characters, etc.)
- Updated document requirements per jurisdiction changes

### Process

1. Fork the repository
2. Create a feature branch (`add-brazil-jurisdiction`, `fix-curp-edge-case`, etc.)
3. Submit a PR with a clear description of what changed and why
4. Include sources for any factual claims (government sites, official advisories)
5. We review and merge. We control `main` but are open to all well-sourced contributions.

---

## About Icalia Labs

[Icalia Labs](https://icalialabs.com) is a software consultancy specializing in staff augmentation and engineering services across US and Latin America. We built this system because we hire engineers across multiple jurisdictions and needed a lightweight, consistent way to verify identity, location, and right-to-work without enterprise-grade compliance infrastructure.

This skill was developed internally by our leadership team — including expertise from fintech KYC programs that onboarded 10,000+ consumers and sole proprietorships across Latin American jurisdictions with direct-to-bank relationships. We open-sourced it because the remote hiring fraud problem affects every company hiring distributed engineering teams, and the solution shouldn't be locked behind enterprise paywalls.

**Maintainers:**
- Icalia Labs Engineering & Talent Acquisition teams
- Questions or feedback: [Open an issue](https://github.com/icalialabs/identity-verification-skill/issues)

---

## License

MIT License. See [LICENSE](LICENSE) for details.

Use it, fork it, modify it, contribute back. The more companies running rigorous verification, the harder it gets for fraud schemes to operate.

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
