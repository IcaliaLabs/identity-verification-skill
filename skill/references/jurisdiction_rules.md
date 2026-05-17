# Jurisdiction-Specific Validation Rules

Reference for Stage 3 document-level validation. Read this before running Step 3 (Document Validation) and Step 1 (Completeness Audit).

---

## Document Checklists by Jurisdiction

### Mexico (all employment types)
- [ ] INE (credencial para votar) — both sides, high-res
- [ ] CURP
- [ ] RFC with homoclave (visible on Constancia de Situación Fiscal)
- [ ] Constancia de Situación Fiscal
- [ ] Comprobante de domicilio < 90 days (CFE, Telmex, water, gas)
- [ ] CLABE for payment
- [ ] Bank statement from traditional bank < 90 days (Banamex, BBVA, Santander, Banorte — NOT Nubank, Mercado Pago, Ualá, Wise, Revolut)

### Colombia (all employment types)
- [ ] Cédula de ciudadanía — both sides (or Cédula de extranjería for foreign residents)
- [ ] RUT
- [ ] Factura de servicios públicos < 90 days
- [ ] Colombian bank account number
- [ ] Bank statement from traditional bank < 90 days (Bancolombia, Davivienda, BBVA Colombia — NOT Nequi, Daviplata as sole account)

### Argentina (all employment types)
- [ ] DNI — both sides
- [ ] CUIT or CUIL
- [ ] Factura de servicios < 90 days
- [ ] CBU for payment
- [ ] Bank statement from traditional bank < 90 days (Banco Nación, Santander Argentina, BBVA Argentina, Galicia — NOT Ualá, Mercado Pago as sole account)

### US Employees
- [ ] I-9 List A (passport, green card, EAD) OR List B + List C (photo ID + SSN card / birth cert / EAD)
- [ ] SSN
- [ ] W-4
- [ ] EOR handling I-9 verification confirmed (must be done within 3 business days of start)

### US Contractors
- [ ] W-9 (US tax residents) OR W-8BEN (non-US tax residents)
- [ ] Government-issued photo ID
- [ ] Proof of address < 90 days
- [ ] Payment account in their legal name

### Visa Holders (US)
- [ ] Passport
- [ ] Current visa document
- [ ] I-94
- [ ] EAD (if applicable)
- [ ] Visa type documented: H-1B, L-1, O-1, TN, F-1 OPT/STEM-OPT, E-2/E-3

---

## Tax ID Validation Algorithms

### Mexico — RFC (Persona Física)

**Format:** 13 characters total
- Positions 1–4: Letters derived from legal name per SAT rules
  - Position 1: First vowel of paternal surname
  - Position 2: First letter of paternal surname
  - Position 3: First letter of maternal surname
  - Position 4: First letter of given name
  - (Edge cases: vowel substitution for names beginning with compound syllables)
- Positions 5–10: YYMMDD (date of birth)
- Positions 11–13: Homoclave (3 alphanumeric characters assigned by SAT)

**Validation:**
1. Confirm 13 characters total
2. First 4 are letters (A–Z)
3. Positions 5–10 form a valid date (YYMMDD)
4. DOB portion must match the candidate's claimed DOB
5. Name-derivation letters must be plausibly derivable from the candidate's legal name
6. Homoclave is 3 alphanumeric characters

**Failed check = hard flag:** "RFC DOB portion YYMMDD does not match claimed DOB [date]" or "RFC name letters are not derivable from the claimed legal name [name]"

### Mexico — CURP

**Format:** 18 characters
- Positions 1–10: Same base as RFC (4 letters + YYMMDD)
- Position 11: H (male) or M (female)
- Positions 12–13: 2-letter state code (e.g., DF, NL, JC)
- Positions 14–16: 3 internal consonants from the name
- Position 17: Century digit (0 for 1900s, A for 2000s)
- Position 18: Mod-10 check digit

**Check digit algorithm (position 18):**
1. Assign values to first 17 characters: digits 0–9 = 0–9; letters A=10, B=11, ..., Z=35
2. Multiply each value by its positional weight (18 down to 2)
3. Sum all products
4. Result = sum mod 10
5. Check digit = (10 − result) mod 10

**Failed check = hard flag:** "CURP check digit does not validate"

### Mexico — INE

- Check "Vigencia" (expiration) on the front
- Expired INE = hard flag: "INE expired on [date]"
- Issuance date should be consistent with claimed age (e.g., INE issued in 2005 for someone claiming to be 25 is implausible)

### Colombia — Cédula de Ciudadanía

**Format:** 8–10 digits, numeric
- No check digit algorithm
- Rough cohort plausibility: lower numbers correlate to older cohorts (cédulas under ~10 million tend to be 1960s or earlier)
- Flag if the number seems implausible for the candidate's apparent age

### Colombia — NIT

**Format:** 9 base digits + 1 check digit = 10 digits total

**Check digit algorithm:**
1. Apply weights [41, 37, 29, 23, 19, 17, 13, 7, 3] to the 9 base digits, right-to-left
2. Sum all products
3. Result = sum mod 11
4. If result is 0 or 1: check digit = result
5. Otherwise: check digit = 11 − result

**Failed check = hard flag:** "NIT check digit does not validate"

### Colombia — Cédula de Extranjería

- For foreign residents
- Typically 6–7 digits
- Confirm with RUT for contracting purposes

### Argentina — DNI

**Format:** 7–8 digits, numeric
- No check digit algorithm
- Number should be plausible for the candidate's apparent age (higher numbers = younger)

### Argentina — CUIT/CUIL

**Format:** 11 digits total
- Positions 1–2: Type prefix
  - 20, 23, 24 = male individual
  - 27 = female individual
  - Other prefixes = entities (not individuals)
- Positions 3–10: The person's DNI (8 digits)
- Position 11: Check digit

**Check digit algorithm:**
1. Apply weights [5, 4, 3, 2, 7, 6, 5, 4, 3, 2] to the first 10 digits
2. Sum all products
3. Result = 11 − (sum mod 11)
4. If result = 11: check digit = 0
5. If result = 10: reclassify with a different type prefix (e.g., 20 → 23 or 23 → 27)
6. Otherwise: check digit = result

**Failed check = hard flag:** "CUIT/CUIL check digit does not validate for the given DNI"

### Argentina — CBU

**Format:** 22 digits
- Two check digits at positions 8 and 22
- Used for bank transfers; confirm the CBU is from a traditional Argentine bank

### United States — SSN

**Format:** 9 digits (XXX-XX-XXXX)

**Invalid patterns (hard flags):**
- Area code (first 3 digits): cannot be 000, 666, or 900–999
- Group (middle 2 digits): cannot be 00
- Serial (last 4 digits): cannot be 0000

No mathematical check digit exists for SSN.

**Failed check = hard flag:** "SSN area/group/serial is invalid per SSA rules"

### United States — I-9 Document Classes

**List A (establishes both identity and work authorization):**
- US passport or passport card
- Permanent resident card (green card)
- Employment authorization document (EAD)
- Foreign passport with I-94 and work endorsement

**List B (identity only) — must be paired with List C:**
- Driver's license
- State-issued ID
- School ID with photo
- Voter registration card
- Military ID

**List C (work authorization only) — must be paired with List B:**
- Social Security card (unrestricted)
- Birth certificate
- Employment authorization document (EAD)

### United States — W-9 vs. W-8BEN

- **W-9:** For US tax residents (citizens, permanent residents, resident aliens)
- **W-8BEN:** For non-US tax residents (foreign individuals)
- **Hard flag:** Claiming to be in Mexico but providing a W-9 (US tax resident form)
- **Hard flag:** Claiming US tax residency but providing a W-8BEN

### Visa Classes to Know

| Visa | Notes |
|---|---|
| H-1B | Specialty occupation, employer-sponsored, 3+3 years |
| L-1 | Intracompany transferee |
| O-1 | Extraordinary ability |
| TN | USMCA professional (Mexico/Canada citizens) |
| F-1 OPT | Post-graduation work, 12 months (STEM: 36 months) |
| E-2/E-3 | Treaty investor / Australian specialty worker |

**Do not self-interpret visa rules.** Route through EOR or immigration counsel.

---

## General ID Checks (All Jurisdictions)

For every government ID:
1. **Is it expired?** Expired ID = hard flag
2. **Issuance date plausible?** An ID issued 20 years ago for someone claiming to be 25 is implausible
3. **Address on ID matches claimed current address?** Flag discrepancies
4. **Address plausibly exists?** City/state/postal code combination valid for the country?

For every bank statement:
1. **Traditional bank?** Not fintech (Nubank, Wise, Revolut, Mercado Pago, Ualá, Nequi, Daviplata)
2. **< 90 days old?**
3. **Account holder name matches legal name on ID?**
4. **Account country matches claimed jurisdiction?**
5. **Transaction patterns consistent with claimed location?** (expenses, merchants, locations)

For every proof of address:
1. **< 90 days old?**
2. **Account holder name matches legal name on ID?**
3. **Address matches claimed current address?**
4. **Utility type is legitimate?** (CFE, Telmex, water, gas — not a mobile phone bill from a fintech)
