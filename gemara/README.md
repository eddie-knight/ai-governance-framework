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
(`git clone --branch v1.1.0 https://github.com/ossf/gemara.git`):

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

See [CONVENTIONS.md](../CONVENTIONS.md) for the current Markdown content model.
