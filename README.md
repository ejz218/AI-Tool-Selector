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

Single config block at the top of the script in `index.html`: ROLES, DATA_CLASSES, USE_CASES, TOOLS, WEIGHTS, AA_SNAPSHOT. Quarterly review owned by Information Security.

## References

- Lehigh Classification of Data: https://data.lehigh.edu/classification-data
- Capability data source (snapshot model): https://artificialanalysis.ai
