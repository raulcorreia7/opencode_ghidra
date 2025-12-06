# opencode_ghidra

Lean guide for working with this repo using Opencode agents.

## Quickstart
- Load context: `.opencode/instructions/01_workflow_and_style.md`, `02_manifests_and_docs.md`, `03_ghidra_mcp_reference.md`.  
  (Optional: `.opencode/instructions/07_binary_context_bof3.md` when working on Breath of Fire III.)
- Commands:
  - `/reverse` — DFS reverse-engineering pass with Decompiler+ASM, typed renames, audit logs.
  - `/reverse-autonames` — export and filter auto-named symbols to `out/candidates/candidates.csv`.
  - `/reverse-plan` — multi-agent plan for large scopes.
- Deliverables/live paths:
  - Reports: `out/reports/`
  - Logs: `out/logs/`
  - Types: `out/types/`
  - Candidates: `out/candidates/`
  - Reversed staging: `out/reversed/`
  - Source (promoted code): `src/`
  - Generated outputs: never edit `build/generated/`; rerun exporters.

## Quality Gates
- Run `make format` then `make verify` before handoff; log any skipped step and why. (Current Makefile stubs these; replace with real commands when available.)
- Apply evidence-driven changes; mark uncertainty with `[INFERRED]`.
- Renames: `$PREFIX<Subsystem>_<ShortName>` (default prefix `LLM_`); keep `// original: <oldname>`.
