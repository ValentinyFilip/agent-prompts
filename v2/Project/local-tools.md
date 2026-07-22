# Local Semantic Code Intelligence Protocol

This file defines the strict semantic lookup workflow, tool-usage hierarchy, and code intelligence constraints that I must adhere to when working inside C#/.NET projects. It decouples the core orchestrator from IDE-specific implementations.

---

## 1. Active Tool Providers
This workspace leverages two complementary semantic tool providers:
- **JetBrains Rider MCP Server**: Provides precise symbol mapping and reference intelligence (providing tools prefixed with `rider__` or similar, such as `rider__search_symbol`, `rider__get_symbol_info`, `rider__find_usages`, `rider__find_implementations`).
- **JetBrains Context MCP Server**: Provides deep natural-language semantic codebase search (providing tools prefixed with `jbcontext__` or similar, such as `jbcontext__search`).

---

## 2. Core Tool Restrictions
I am **strictly forbidden** from using standard filesystem search tools (such as `grep_search`, `search_files`, or manually reading entire files) to locate C# classes, methods, fields, interfaces, or DTO structures unless I have **first attempted** to use the active semantic tools defined below.

---

## 3. Mandatory Semantic Workflow
For every exploration or refactoring task in the C# solution, I must execute this exact sequence:

1. **Broad Conceptual and Semantic Discovery**:
   - When the exact file, class, or symbol name is unknown, or when doing exploratory research, I must run `jbcontext__search` first using focused natural-language queries.
2. **Precise Symbol Identification**:
   - Once a relevant class, interface, or method has been identified, or if the exact symbol name is already known, I must run `rider__search_symbol` or `rider__get_symbol_info`.
   - I will rely on Rider's symbol definitions instead of manually opening files to find class members or parameter types.
3. **Usage and Dependency Mapping**:
   - I must run `rider__find_implementations` to find concrete vertical slice handlers (e.g., discovering all classes implementing `AbstractProcessor` or `IGeneratePdf`).
   - I must run `rider__find_usages` to identify exactly which files, mappings, or controllers will be affected by a refactor or change before editing them.
4. **Fallback Exception**:
   - I am only permitted to fall back to standard file searches (`grep_search` or manual `read_file` lines) if:
     - The active MCP tools (both Rider and JetBrains Context) return an explicit error or are offline.
     - The active MCP tools return zero results after searching.
     - The file being inspected is a non-C# file (e.g., `.json`, `.css`, `.yml`) that the semantic engines cannot parse.

---

## 4. Build & Diagnostics First
When verifying C# code changes, I must check Rider's semantic diagnostics first before running terminal-level `dotnet build` commands. This allows the IDE to catch type safety, import errors, and dependency diagnostics instantly, preventing compile-loop overhead.
