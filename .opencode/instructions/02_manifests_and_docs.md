# Manifests & Docs (context)

- Source of truth: `src/docs/data/*.json`; follow schemas in `src/docs/schemas/*.schema.json` when present.
- Update manifests first, then runtime code/Markdown; include evidence (addresses, references).
- If schema validators or generators exist, run them; otherwise ensure JSON is well-formed and consistent with schemas by review.
- Avoid manual edits to generated docs; rerun the project’s generator if/when it exists.
