---
name: /reverse
description: Reverse Engineer Expert — Ghidra DFS review with Decompiler+ASM, typed renames, and auditable logs
agent: build
usage: "/reverse $ARGUMENTS=\"all\" $PREFIX=\"LLM_\" $SUBSYSTEMS=\"Sys,Vid,Gpu,Gte,Spu,Pad,Cd,Mdec,Game,Ui,Snd,Fs,Save,Math,Mem,Bios\""
---

Run a depth-first reverse-engineering pass in Ghidra. Always read both Decompiler and ASM views, keep evidence-driven changes, and emit auditable artifacts.

### Inputs
- `$ARGUMENTS` (optional): scope (e.g., `all`, `network`, `crypto`, `ui`, comma-separated addresses).
- `$PREFIX` (optional, default `LLM_`): rename prefix; pattern is `$PREFIX<Subsystem>_<ShortName>` using subsystems from `01_workflow_and_style.md` (Sys, Vid, Gpu, Gte, Spu, Pad, Cd, Mdec, Game, Ui, Snd, Fs, Save, Math, Mem, Bios).
- `$SUBSYSTEMS` (optional): comma-separated subsystem tags to help pick names; defaults shown in `usage`.

### Runbook
1) Scope: If `$ARGUMENTS` empty → analyze `all`; otherwise limit traversal to provided scope/addresses.
2) Seeds: Enumerate Symbol Tree → Entry Points (add `_start`, CRT init, exports).
3) DFS traversal: From seeds, walk callees depth-first with a visited set; capture call stacks and leaf detection.
4) Per-function review (every visited function):
   - Open **Decompiler** and **ASM**. Document calling convention, stack usage, syscalls, vector/GPU ops, crypto, etc. inline.
   - Header comment template:
     ```
     // Subsystem: <${SUBSYSTEM or UNKNOWN}>
     // Summary: <one-line purpose>
     // Params: <type/name>...
     // Return: <type>
     // original: <oldname>
     // [INFERRED] if speculative
     ```
   - Types: Fix signatures, locals, and fields using PsyQ/SDK typedefs where possible; re-run propagation.
   - Renames: Only auto-named (`sub_`, `FUN_`, `loc_`, `dword_`, `byte_`, `word_`, `unk_`). Pattern `$PREFIX<Subsystem>_<ShortName>`; keep original in comment `// original: sub_XXXXXXXX`.
   - Naming scope: Globals/data keep prefix; locals get concise semantic names without `$PREFIX` unless cross-function static.
5) Evidence & logging:
   - Log renames to `out/candidates/candidates.csv` as `address,original_name,new_name,justification`.
   - Append traversal + notes to `out/logs/function_log.json`.
   - Maintain `out/logs/rename_log.md` with timestamp/operator and applied renames.
6) Reporting:
   - `out/reports/report.md` — scope, traversal summary, rename table, notable behaviors/IOCs, types imported, unknowns, next steps.
   - Optionally drop helper scripts under `tools/ghidra/`.

### Helper Command
- `/reverse-autonames` — export and filter auto-named symbols into `out/candidates/candidates.csv`.

### Non-Negotiable Constraints
- Do not modify binary instructions/layout — metadata only.
- Always review both Decompiler and ASM for every visited function.
- Mark assumptions with `[INFERRED]`; prefer propose → document → apply, avoid mass blind renames.
