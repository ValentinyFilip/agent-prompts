# Local Semantic Code Intelligence Protocol

This file defines the strict semantic lookup workflow, tool-usage hierarchy, and code intelligence constraints that I must adhere to when working inside C#/.NET projects. It decouples the core orchestrator from IDE-specific implementations.

---

## 1. Active Tool Provider: JetBrains Rider MCP Server
The active semantic code intelligence provider on this machine is the **JetBrains Rider MCP Server** (providing tools prefixed with `rider__` or similar, such as `rider__search_symbol`, `rider__get_symbol_info`, `rider__find_usages`, `rider__find_implementations`).

---

## 2. Core Tool Restrictions
I am **strictly forbidden** from using standard filesystem search tools (such as `grep_search`, `search_files`, or manually reading entire files) to locate C# classes, methods, fields, interfaces, or DTO structures unless I have **first attempted** to use the active semantic tools defined below.

---

## 3. Mandatory Semantic Workflow
For every exploration or refactoring task in the C# solution, I must execute this exact sequence:

1. **To Locate a Class, Interface, or Method**:
   - I must run `rider__search_symbol` or `rider__get_symbol_info` first.
   - I will rely on Rider's symbol definitions instead of manually opening files to find class members or parameter types.
2. **To Find Where a Type is Used or Implemented**:
   - I must run `rider__find_implementations` to find concrete vertical slice handlers (e.g., discovering all classes implementing `AbstractProcessor` or `IGeneratePdf`).
   - I must run `rider__find_usages` to identify exactly which files, mappings, or controllers will be affected by a refactor or change before editing them.
3. **Fallback Exception**:
   - I am only permitted to fall back to standard file searches (`grep_search` or manual `read_file` lines) if:
     - The Rider MCP tool returns an explicit error or is offline.
     - The Rider MCP tool returns zero results after searching.
     - The file being inspected is a non-C# file (e.g., `.json`, `.css`, `.yml`) that the semantic engine cannot parse.

---

## 4. Build & Diagnostics First
When verifying C# code changes, I must check Rider's semantic diagnostics first before running terminal-level `dotnet build` commands. This allows the IDE to catch type safety, import errors, and dependency diagnostics instantly, preventing compile-loop overhead.
