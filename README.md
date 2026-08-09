# Lehigh AI Tool Advisor (POC)

Proof of concept: a governed AI tool recommender for Lehigh University students, faculty, and staff. Select role, use case, and data classification; receive ranked tool matches with transparent factor-level scoring, availability status, and guardrails.

## Status

**DRAFT — pending Information Security validation.** The tool inventory, role mappings, data classification ceilings, and all capability scores are placeholder judgments, not approved guidance. Do not treat any recommendation as authoritative until validated.

## Design principles

- Data classification is a hard gate (Lehigh 4-class framework, Class I–IV); capability never overrides contract.
- Locked tools remain visible with full scores, surfacing licensing demand.
- Match scores decompose into weighted, named factors per use case for auditability.
- Community feedback (thumbs) is a separate signal and never adjusts governed scores.
- Feedback is in-memory only in this POC; production requires a collector endpoint.

## Maintenance

Single config block at the top of the script in `index.html`: ROLES, DATA_CLASSES,
USE_CASES, TOOLS, WEIGHTS, AA_SNAPSHOT. Quarterly review owned by Information
Security. Everything below the `Engine` divider is machinery and shouldn't need
editing to change policy.

### The two availability lists are different questions

Each tool carries both `roles` and `eligible`, and conflating them produces
confidently wrong advice in either direction:

- **`roles`** — who has the tool provisioned **today**, no request needed.
- **`eligible`** — who may **obtain** it, by purchase or grant. Omit it entirely
  and anyone may request; include it and everyone outside the list is told the
  tool is not available to them.

A tool can have nobody in the first and several in the second — Claude is the
worked example. Telling a graduate student a grant-funded tool is "not for
graduate students" and inviting an undergraduate to request something they
cannot get are the same bug pointed in opposite directions.

### `ceiling` is the hard gate — read the ranks carefully

`ceiling` is the highest `DATA_CLASSES.rank` permitted. Ranks run **1 = Class IV
(public) through 4 = Class I (critical)**, so `1` = Class IV only and `4` = Class I
permitted. Getting this off by one silently loosens or tightens the gate on every
tool.

On the ITS table, **"Under evaluation" means not approved for any confidential
data.** Classes I–III are all confidential, so it resolves to `ceiling: 1`, not 2.

### `provisioning-by-role.csv`

The reviewed source for who gets what. Regenerate it from the config, hand it to
Information Security to edit, then apply the result back — that round trip is how
the current values were set, and it keeps the provenance of a governance decision
outside a JavaScript object. Codes: `A` provisioned, `R` obtainable by request,
`N` not available to that role.

### Google Gemini AI Pro is a superset of Google Gemini

Anything Gemini can do, the paid add-on can do too. Raising a base Gemini factor
without raising Pro's produces a card where the free tier beats the paid upgrade.
Check Pro ≥ base on **every** factor across all nine use cases after touching
either.

## Known gaps

- **`AA_SNAPSHOT` is not wired up.** Eight of ten entries are `null` and `asOf`
  is a placeholder. The Artificial Analysis reference below describes the intended
  model, not a live feed. If it is populated, do it server-side — the API key must
  never ship in this file.
- **Inventory drift against the ITS table.** GitHub Copilot appears here but is
  not on the table, and the table says to treat unlisted services as not approved.
  DataCamp, LibreChat, and Sandbox are on the table but absent here — the first two
  are Class III + IV, so their absence understates what people may use.
- **Every capability score is a placeholder**, drafted for plausibility and
  internal consistency, not measured. They have not been validated by anyone.
- **Feedback is in-memory only.** Votes vanish on reload; production needs a
  collector endpoint.
- **All request links point at one service desk form.** The form, not this page,
  explains what the H.S. Lee Family Foundation gift will fund.

## Styling

Uses the official Lehigh brand system: Dopis for display type (self-hosted on
lehigh.edu, served with `Access-Control-Allow-Origin: *`, so it loads from any
origin), Inter for body, and only named Brand Hub palette values. Sora and Barlow
Condensed are the brand's own documented substitutes if those `~inis` font URLs
ever move — that path is legacy-looking, so treat a 404 there as a signal rather
than a mystery.

Every text/background pairing was checked against WCAG AA. Two are worth
remembering: **Golden Hour fails as text on white** (1.28:1) and is an accent only,
and focus rings need 3:1 against their own surface, so they are brown on light and
gold on the brown header.

## References

- Lehigh Classification of Data: https://data.lehigh.edu/classification-data
- ITS data classification table (source of truth for `ceiling`):
  https://its.lehigh.edu/information-security/training-awareness/data-classification
- Lehigh Brand Hub: https://live.standards.site/lehighbrandhub
- Capability data source (snapshot model, not currently live):
  https://artificialanalysis.ai
