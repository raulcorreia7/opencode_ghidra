# Ghidra MCP Reference (context)

## Available Tools

### Program Analysis
- `ghidra-server_get_program_info` — Get basic program information
- `ghidra-server_list_programs` — List all open programs (multi-program)
- `ghidra-server_list_functions` — List all functions
- `ghidra-server_list_data` — List data definitions
- `ghidra-server_list_data_types` — List all available data types
- `ghidra-server_list_strings` — List string references
- `ghidra-server_list_imports` — List imported functions
- `ghidra-server_list_exports` — List exported functions
- `ghidra-server_list_segments` — List memory segments
- `ghidra-server_list_namespaces` — List namespaces
- `ghidra-server_list_classes` — List class definitions
- `ghidra-server_list_methods` — List method definitions

### Function Analysis
- `ghidra-server_get_function_info` — Detailed function info
- `ghidra-server_get_class_info` — Detailed class info
- `ghidra-server_get_function_by_address` — Function at specific address
- `ghidra-server_get_current_function` — Function at cursor
- `ghidra-server_decompile_function` — C-like decompilation
- `ghidra-server_disassemble_function` — Assembly disassembly
- `ghidra-server_search_functions` — Search functions by name
- `ghidra-server_search_classes` — Search classes by name
- `ghidra-server_function_xrefs` — Cross-references to/from function

### Location & Navigation
- `ghidra-server_get_current_address` — Current cursor address
- `ghidra-server_get_hexdump` — Hexdump at address
- `ghidra-server_xrefs_to` — References to an address
- `ghidra-server_xrefs_from` — References from an address

### Modification Tools
- `ghidra-server_rename_function` — Rename functions
- `ghidra-server_rename_function_by_address` — Rename function at address
- `ghidra-server_rename_variable` — Rename variables
- `ghidra-server_rename_data` — Rename data definitions
- `ghidra-server_set_function_prototype` — Set function signatures
- `ghidra-server_set_local_variable_type` — Set variable data types
- `ghidra-server_set_disassembly_comment` — Add disassembly comments
- `ghidra-server_set_decompiler_comment` — Add decompiler comments

### Structure & Data Type Management
- `ghidra-server_get_data_type` — Get data type/structure details
- `ghidra-server_create_struct` — Create user-defined structures
- `ghidra-server_modify_struct` — Modify structures with C definitions
- `ghidra-server_rename_structure_field` — Rename structure fields
- `ghidra-server_set_data_type` — Set data type at address
- `ghidra-server_auto_create_struct` — Auto-create structures from usage

## Workflow Guidelines
- Always use MCP commands, not Ghidra CLI.
- Keep manifest entries and Ghidra state aligned; record exact requests (address + command) as evidence.
- Update manifests immediately after MCP operations; regenerate derived artifacts via exporters.
- Use descriptive comments for functions and variables to aid analysis.
- Only make suggestions backed by evidence; if uncertain, annotate as `[INFERRED]` or hold back.
- For constants: check PSn00bSDK or project SDK defines/enums/constants; otherwise propose a new one.
