---
name: /reverse-autonames
description: Export and filter auto-named symbols into candidates.csv
agent: build
usage: "/reverse-autonames"
---

Use the Ghidra MCP to export auto-named symbols and filter them into `out/candidates/candidates.csv`.

Steps:
1) Export via MCP: call `ghidra-server_list_functions` and write auto-named symbols to `out/candidates/candidates_raw.txt` (log and continue if unavailable; if MCP lacks direct export, capture output manually).
2) Filter: `cat out/candidates/candidates_raw.txt | rg --no-line-number -E '(^sub_|FUN_|^loc_|dword_|byte_|word_|unk_)' > out/candidates/candidates.csv`.
