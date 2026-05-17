# Scheme Patterns Reference

Reference for Stage 3 Step 7 (Scheme Pattern Check). Read this before running the pattern review.

**Important:** These are indicators, not proof. Soft signals (voice/accent, name patterns, regional Spanish) are advisory only. A Block decision always requires corroborating hard flags (failed validation, wrong-country payment routing, document mismatch). Never Block on soft signals alone.

---

## DPRK IT Worker Indicators

Per FBI/Treasury/Mandiant public advisories. These indicators are most concerning when multiple appear together.

### Hard indicators (flag immediately if present)
- Claimed residence in one country; payment routing to a different country (commonly China, Russia, Vietnam, or third-country intermediaries)
- Company hardware ship-to address in a US state known for "laptop farms" (historically Arizona, Tennessee, Florida — but this evolves)
- Profile photo is a confirmed reverse-image hit on another identity, or shows clear AI-generation artifacts (TPDNE-style facial symmetry, skin texture anomalies, impossible backgrounds)

### Soft indicators (flag only when combined with other signals)
- Video off during interviews, or heavy use of filters/avatars; reluctance to hold ID to camera live
- GitHub / LinkedIn profiles < 12 months old with curated but shallow content
- Resume reads like a template; employers listed cannot be reached through independent channels (no phone, no LinkedIn, no web presence)
- Interview answers that are over-rehearsed, word-for-word consistent with known script patterns
- Activity patterns (commit times, interview availability) clustered in a timezone that is NOT the claimed one
  - Example: "Claimed to be in Mexico but all commits are 1–8 AM UTC (midnight–7 AM Mexico City time), which is unusual; more consistent with East Asia timezone"

### What to look for in combination
A single soft indicator is not actionable. Three or more soft indicators, or one hard indicator + one soft indicator, warrant escalation to High tier.

---

## Identity-for-Hire Indicators

Real identity documents (purchased or stolen) being fronted by a different person. More sophisticated than pure document forgery because the IDs themselves may be authentic.

### Hard indicators
- Live video voice/face (from Stage 2 notes) does not convincingly match the person in the government ID photo
- Payment routes to an account name that differs from the ID name, with explanation like "my cousin handles banking" or "my accountant receives payments"
- Candidate cannot answer basic biographical questions about their own claimed life:
  - Neighborhood landmarks near their claimed address
  - Recent local news in their claimed city
  - Names of local elected officials
  - Common local prices (liter of milk, metro fare, etc.)

### Soft indicators
- All references are personal gmails or all from the same non-employer domain
- Resume employment history doesn't match the person's apparent age on video
- Hesitation or confusion when asked about details that should be automatic (own birthday, own address spelled out, parents' names)

---

## General Location Fraud Indicators

Person is not physically present in the jurisdiction they claim. May or may not involve identity fraud — sometimes it's the real person, just not where they say they are.

### Hard indicators
- IP geolocation (from application, interviews, or Stage 4 agreement signing) in a different country than claimed
- VPN/residential proxy exit patterns visible (datacenter ASNs from geolocation services)
- Bank statement transaction locations don't match claimed residence (expenses in a different city/country during the claimed period of residence)
- Undocumented address appears during onboarding that exists in no formal documentation (tax docs, bank statement, proof of address, ID)

### Soft indicators
- Regional Spanish mismatch:
  - Voseo (vos + conjugation) when claiming to be Mexican (Mexico uses tú)
  - Mexican Spanish patterns (güey, mande, órale) when claiming to be Argentine
  - Colombian Spanish patterns off for the claimed region
  - **Note:** This is inherently noisy. People move, pick up accents, have mixed backgrounds. Never use as sole basis for a flag.
- Unaware of national holidays, recent political events, local brands, food prices, weather patterns for claimed city (from Stage 2 interview notes)
- Refusal or heavy reluctance to do a surprise video call during claimed local business hours
- Requests payment via remittance service (Western Union, MoneyGram) or cryptocurrency instead of local bank transfer
- Only fintech accounts available — no traditional bank relationship in the claimed country

### What to look for in combination
Location fraud is the most common of the three categories. The strongest single signal is the **bank statement from a traditional bank** — if the candidate can produce one dated within 90 days, in their legal name, from a brick-and-mortar bank in the claimed country, it's very hard to fake. If they can't, that alone is a finding worth flagging.

Payment routing is the second strongest signal. If money goes to a different country than the claimed residence, that's a hard flag regardless of explanation.

IP geolocation + bank statement + proof of address + tax documentation all pointing to the same country = strong location confirmation. Any one of these pointing to a different country = escalation.

---

## Pattern Scoring Guide

Use this to calibrate your risk tier:

| Pattern | Alone | Combined with 1 other | Combined with 2+ others |
|---|---|---|---|
| Single soft indicator | Note it, don't flag | Medium tier consideration | High tier |
| Single hard indicator | High tier | High tier | Block tier |
| Multiple hard indicators | Block tier | Block tier | Block tier |
| Failed check-digit validation | High tier (always) | Block tier | Block tier |
| Wrong-country payment routing | High tier (always) | Block tier | Block tier |
| Fintech-only (no traditional bank) | Medium tier | High tier | High tier |
| Undocumented address mid-process | High tier | Block tier | Block tier |

---

## Bias Safeguards

Before finalizing any risk tier above Low, ask yourself:

1. **Am I flagging a hard signal (failed validation, wrong-country payment, name mismatch) or a soft signal (accent, name pattern, thin OSINT)?**
2. **Would I flag this same pattern for a candidate from any country, or am I applying different scrutiny based on nationality?**
3. **Is my Block recommendation supported by at least one hard flag, or only by soft signals?**

If the answer to #3 is "soft signals only," downgrade to High and request additional verification rather than blocking.
