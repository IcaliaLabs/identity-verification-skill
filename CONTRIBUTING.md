# Contributing to Identity Verification Skill

Thanks for your interest in contributing. This project is maintained by [Icalia Labs](https://icalialabs.com) and we welcome contributions that make remote hiring verification more rigorous for everyone.

---

## What We're Looking For

### High-Value Contributions

1. **New jurisdictions.** The most impactful contribution is adding a new country. See [Adding a Jurisdiction](#adding-a-jurisdiction) below.
2. **Validation algorithm corrections.** If you find a check-digit algorithm that's wrong or missing an edge case, we want to know.
3. **Updated scheme patterns.** Fraud evolves. If you have documented, sourced intelligence on new patterns (DPRK, identity-for-hire, location fraud), submit it.
4. **Bug fixes.** If the skill produces incorrect output for a specific input pattern, report it with the input data (anonymized) and expected output.

### What We'll Probably Decline

- Changes that make the skill less cautious (lowering scrutiny, removing checks)
- Additions without credible sources (government websites, official advisories, published research)
- Jurisdiction additions without check-digit algorithms or validation rules (we need the math, not just the document names)
- Changes that introduce bias (flagging based on name patterns, nationality heuristics, or accent alone)

---

## Adding a Jurisdiction

This is the most valuable contribution. To add a new country:

### 1. Add to `skill/references/jurisdiction_rules.md`

Follow the existing format. You need:

**Document checklist:**
```markdown
### {{Country Name}} (all employment types)
- [ ] Government ID — type, both sides
- [ ] Tax ID — type and format
- [ ] Proof of address < 90 days
- [ ] Bank statement from traditional bank < 90 days
- [ ] Payment account details
```

**Tax ID validation algorithm:**
```markdown
### {{Country}} — {{Tax ID Name}}

**Format:** [length] characters
- [Structure breakdown]

**Check digit algorithm:**
1. [Step-by-step math]
2. [Weights, modular arithmetic, special cases]

**Failed check = hard flag:** "[specific error message]"
```

**Traditional vs. fintech banks:**
- List 3–5 traditional banks for the country
- List the major fintechs that should NOT be accepted as sole proof of presence
- Explain why (e.g., "can be opened remotely without in-person verification")

**Right-to-work requirements:**
- What documents prove work authorization?
- Any visa classes to be aware of?

### 2. Source Everything

Every validation algorithm must include a source:
- Government website (SAT, DIAN, AFIP, IRS, etc.)
- Official documentation or published specification
- Credible secondary source if primary is unavailable

Include sources as comments in your PR description, not in the file itself.

### 3. Submit the PR

- Branch name: `add-{{country}}-jurisdiction`
- PR title: `Add {{Country}} jurisdiction support`
- Description: What you added, sources, any edge cases or limitations
- Tag with `jurisdiction` label

---

## Improving Scheme Patterns

To add or update fraud patterns in `skill/references/scheme_patterns.md`:

1. **Source required.** FBI advisories, Treasury sanctions guidance, Mandiant threat reports, CISA alerts, or equivalent credible sources.
2. **Distinguish hard vs. soft indicators.** Hard indicators warrant immediate escalation. Soft indicators are advisory only.
3. **Include bias safeguards.** If a new indicator could encode nationality, accent, or name-based bias, flag it explicitly as "advisory only — never use as sole basis for a flag."

---

## Process

1. **Fork** the repository
2. **Create a feature branch** with a descriptive name
3. **Make your changes** following the existing format and conventions
4. **Submit a PR** with:
   - Clear description of what changed and why
   - Sources for any factual claims
   - Any edge cases or limitations you're aware of
5. **Review:** We'll review within 1–2 weeks. We may request changes or ask for additional sources.
6. **Merge:** We control `main`. Once approved, we merge and tag a release.

---

## Code of Conduct

- Be respectful and constructive
- Source your claims
- Don't introduce bias
- Don't weaken security checks
- Remember this tool affects real people's livelihoods — accuracy and fairness matter

---

## Questions?

[Open an issue](https://github.com/icalialabs/identity-verification-skill/issues) with the `question` label.

---

*Maintained by [Icalia Labs](https://icalialabs.com)*
