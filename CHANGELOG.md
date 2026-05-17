# Changelog

All notable changes to this project will be documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] — 2026-05-17

### Added
- Initial public release
- **Stage 1:** Pre-interview screen (public data review + OSINT plausibility check)
- **Stage 3:** Full documentation review (IDs, tax docs, bank statements, proof of address, payment routing, scheme patterns)
- **Jurisdictions:** Mexico (INE, RFC, CURP), Colombia (Cédula, NIT), Argentina (DNI, CUIT/CUIL), United States (SSN, I-9, W-9/W-8BEN)
- **Scheme patterns:** DPRK IT worker indicators, identity-for-hire patterns, general location fraud signals
- **Tax ID validation algorithms:** RFC (name derivation + DOB), CURP (mod-10 check digit), CUIT/CUIL (weighted sum mod 11), NIT (weighted sum mod 11), SSN (pattern validation)
- **Risk tier system:** Low / Medium / High / Block with defined actions per tier
- **Bias safeguards:** Hard vs. soft signal distinction, bias self-check before Block decisions
- **Auto-stage detection:** Skill detects Stage 1 vs. Stage 3 based on input data and confirms with user

### Notes
- This is the first public release. We expect to iterate after community feedback and additional jurisdiction contributions.
- Built and maintained by [Icalia Labs](https://icalialabs.com).
