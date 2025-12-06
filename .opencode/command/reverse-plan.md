---
name: /reverse-plan
description: High-level reverse engineering planner — parallel multi-agent workflow
agent: build
usage: "/reverse-plan $ARGUMENTS=\"all\" $PREFIX=\"LLM_\" $CONCURRENCY=\"4\""
---

Coordinate a four-agent plan for large reverse-engineering efforts. Keep it project-agnostic; place any binary/game specifics in a separate context file.

### Inputs
- `$ARGUMENTS` (optional): scope to prioritize (e.g., `battle`, `map`, `gfx`, `script`).
- `$PREFIX` (optional, default `LLM_`): rename prefix shared across agents; use `$PREFIX<Subsystem>_<Name>` with subsystems from `01_workflow_and_style.md`.
- `$CONCURRENCY` (optional, default `4`): number of subagents to run concurrently.

### Phase 1 — Repository & Knowledge Audit
1) Summarize key project folders (docs, analysis notes, memory maps, headers, source).
2) Identify subsystems (Sys, Gfx, Snd, Map, Battle, Script, IO, Mem, etc.).
3) Build or refresh symbol inventory with `/reverse-autonames` → `out/candidates/candidates.csv` (keeps `out/candidates/candidates_raw.txt` for audit).
4) Write `out/reports/report-planner.md`: subsystem map, function counts, header/type coverage, missing SDK deps.

### Phase 2 — Define Four Parallel Agents
- **Agent 1 — Structural Analyst**
  - DFS from entry points (`_start`, `main`, loops, exports); record call chains.
  - Review Decompiler + ASM; add header comments (`Summary`, `Params`, `Return`, `original`, `[INFERRED]`).
  - Outputs: `out/logs/function_log.json`, `out/reports/report_structure.md`.
- **Agent 2 — Type & Interface Engineer**
  - Recover data structures and SDK-compatible types; fix signatures; rerun propagation.
  - Outputs: `out/types/types_recovered.cty`, `out/reports/report_types.md`.
- **Agent 3 — Naming & Semantics Specialist**
  - Apply `$PREFIX<Subsystem>_<Name>` to auto-named symbols; keep `// original: <oldname>`, `[INFERRED]` when unsure.
  - Outputs: `out/candidates/candidates_named.csv`, `out/reports/report_naming.md`.
- **Agent 4 — Documentation & Integration Orchestrator**
  - Reconcile outputs from Agents 1–3; resolve conflicts; enforce consistent format.
  - Outputs: `out/reports/report_final.md`, `out/reports/summary_index.md`.

### Phase 3 — Coordination & Parallelization
- Run Agents 1–4 in parallel on separate DB snapshots; avoid overlapping writes.
- Agent 4 performs periodic merges and consistency checks.

### Phase 4 — Deliverables
- Reports under `out/reports/` (structure, types, naming, final, planner, summary index).
- Logs under `out/logs/` (function traversal, rename log).
- Type definitions under `out/types/` (e.g., `types_recovered.cty`).
- Helper scripts under `tools/ghidra/` as needed.

### Non-Negotiable Guidelines
- Do not modify binary instructions/layout — metadata only.
- Mark assumptions with `[INFERRED]`; always verify in both Decompiler and ASM.
- Keep renames concise, subsystem-prefixed, and reversible; log every automated change for auditability.
