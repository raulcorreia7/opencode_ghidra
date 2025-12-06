# Workflow & Style (context)

## Core Principles
- Evidence-driven changes; mark uncertainty with `[INFERRED]`.
- Schema-validated data; contract-governed docs; tool-automated workflows.
- Agent lifecycle: Restate → Review → Plan → Build → Test → Verify → Self-Reflect.

## Build & Quality Gates
- Always run `make format` then `make verify` before handoff; keep checks fast. (Stubbed in this template—replace with real commands when available.)
- Do not merge changes that fail verification; log any skipped step and why.
- Never edit generated artifacts under `build/generated/`; rerun exporters instead.

## Naming & Renames
- Default prefix: `LLM_`; pattern: `$PREFIX<Subsystem>_<ShortName>`.
- Subsystems: Sys, Vid, Gpu, Gte, Spu, Pad, Cd, Mdec, Game, Ui, Snd, Fs, Save, Math, Mem, Bios (extend if the project has more).
- Auto-named symbols only (`sub_`, `FUN_`, `loc_`, `dword_`, `byte_`, `word_`, `unk_`); keep `// original: <oldname>`.
- Locals: lowerCamel (`buf`, `count`, `spriteId`, `ctx`); reserve `$PREFIX` for globals/statics.
- Constants: SCREAMING_SNAKE_CASE; Enums: PascalCase name, members `PREFIX_VALUE`.

## C/C Header Style
- 4-space indent; sorted includes; headers self-contained and guarded.
- Struct fields include offset comments when known.
- Generated stubs keep `/* AUTO-GENERATED */`; promoted code cites manifest evidence and removes artifacts.
