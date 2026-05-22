# Gemara representation of the AI Governance Framework

This directory holds the AIGF risks and mitigations expressed in the
[Gemara](https://gemara.openssf.org) machine-readable governance schema.

## ⚠️ Canonical source of truth

**The Markdown collections remain canonical.** Until the migration described
below is complete, the authoritative content lives in:

- `docs/_risks/ri-*.md`
- `docs/_mitigations/mi-*.md`
- `docs/_usecases/`

The YAML in this directory is a hand-authored representation of that content.
**If the two diverge, the Markdown wins.** Do not treat this YAML as canonical
until the cutover below has happened.

## Schema

These artifacts target **Gemara v1.1.0** (the latest stable release) and are
validated against it with `cue vet`. Every file declares
`gemara-version: "1.1.0"` and carries `draft: true` while unratified.

| File | `metadata.type` |
|------|-----------------|
| `guidance.yaml` | `GuidanceCatalog` — mitigations as guidelines |
| `vectors.yaml` | `VectorCatalog` — risks as vectors |
| `principles.yaml` | `PrincipleCatalog` |
| `aigf-to-nist-mapping.yaml` | `MappingDocument` → NIST SP 800-53r5 |

To re-validate against a checkout of the schema repo
(`git clone --branch v1.1.0 https://github.com/gemaraproj/gemara.git`):

```sh
cue vet /path/to/gemara/*.cue guidance.yaml            -d '#GuidanceCatalog'
cue vet /path/to/gemara/*.cue vectors.yaml             -d '#VectorCatalog'
cue vet /path/to/gemara/*.cue principles.yaml          -d '#PrincipleCatalog'
cue vet /path/to/gemara/*.cue aigf-to-nist-mapping.yaml -d '#MappingDocument'
```

## Migration intent

This is the first step toward Gemara becoming the source of truth, eventually
replacing the YAML front matter in the Markdown collections. The planned
end-state reverses today's flow: the Markdown (or its front matter) would be
**generated from** this Gemara content rather than maintained alongside it, so
the two never have to be kept in sync by hand.

"Cutover" is not done yet. Before it can be:

- [x] Pin the Gemara schema version (v1.1.0, validated with `cue vet`).
- [ ] Decide the file granularity (single catalog vs. per-entry, mirroring the
      one-file-per-risk/mitigation workflow the Markdown uses today).
- [ ] Define and build the generation flow (Markdown front matter ← Gemara).
- [ ] Record the decision (ADR / maintainer sign-off) — this commits the
      framework to Gemara as an interchange standard.

## Coverage

`guidance.yaml` represents **23 of 23** AIGF mitigations (controls). Every
control below has a guideline entry, its vectors (derived from the risks it
mitigates), and its NIST SP 800-53r5 mappings.

- [x] AIR-PREV-002 — Data Filtering From External Knowledge Bases (mi-2)
- [x] AIR-PREV-003 — User/App/Model Firewalling/Filtering (mi-3)
- [x] AIR-DET-001 — AI Data Leakage Prevention and Detection (mi-1)
- [x] AIR-DET-004 — AI System Observability (mi-4)
- [x] AIR-PREV-005 — System Acceptance Testing (mi-5)
- [x] AIR-PREV-006 — Data Quality, Classification & Sensitivity (mi-6)
- [x] AIR-PREV-007 — Legal & Contractual Frameworks for AI Systems (mi-7)
- [x] AIR-PREV-008 — Quality of Service (QoS) & DDoS Prevention (mi-8)
- [x] AIR-DET-009 — AI System Alerting & Denial-of-Wallet Spend Monitoring (mi-9)
- [x] AIR-PREV-010 — AI Model Version Pinning (mi-10)
- [x] AIR-DET-011 — Human Feedback Loop for AI Systems (mi-11)
- [x] AIR-PREV-012 — Role-Based Access Control for AI Data (mi-12)
- [x] AIR-DET-013 — Citations & Source Traceability (mi-13)
- [x] AIR-PREV-014 — Encryption of AI Data at Rest (mi-14)
- [x] AIR-DET-015 — LLM-as-a-Judge Automated Evaluation (mi-15)
- [x] AIR-DET-016 — Preserving Source Data Access Controls (mi-16)
- [x] AIR-PREV-017 — AI Firewall Implementation & Management (mi-17)
- [x] AIR-PREV-018 — Agent Authority Least-Privilege Framework (mi-18)
- [x] AIR-PREV-019 — Tool-Chain Validation & Sanitization (mi-19)
- [x] AIR-PREV-020 — MCP Server Security Governance (mi-20)
- [x] AIR-DET-021 — Agent Decision Audit & Explainability (mi-21)
- [x] AIR-PREV-022 — Multi-Agent Isolation & Segmentation (mi-22)
- [x] AIR-PREV-023 — Agentic System Credential Protection Framework (mi-23)

`vectors.yaml` represents all **23 of 23** AIGF risks (88 vectors). Vector IDs
follow `AIR-<type>-<risk-sequence>[-<NN>]`, where the type and sequence come
from the source risk's front matter and the optional `-NN` suffix enumerates
the distinct mechanisms described in that risk.

See [CONVENTIONS.md](../CONVENTIONS.md) for the current Markdown content model.
