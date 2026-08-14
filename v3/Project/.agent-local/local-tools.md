---
paths:
  - "**/*.cs"
  - "**/*.csproj"
  - "**/*.sln"
  - "**/*.slnx"
---

# Local Semantic Code Intelligence Protocol for Claude Code

This file defines the strict semantic lookup workflow, tool-usage hierarchy, and code intelligence constraints that I must follow when working inside C#/.NET projects through Claude Code. It decouples the core orchestrator from IDE-specific implementations.

---

## 1. Active Tool Providers
This workspace leverages two complementary semantic tool providers through MCP:
- **JetBrains Rider MCP Server**: Provides precise symbol mapping, diagnostics, usage discovery, and implementation discovery.
- **JetBrains Context MCP Server**: Provides deep natural-language semantic codebase search.

Claude Code normally exposes MCP tools with server-qualified names such as `mcp__<server-name>__<tool-name>`. Server names and tool names depend on the installed MCP configuration. At the start of semantic work, I must inspect the available tool list and map the capabilities below to the exact exposed names. If the configured servers are unclear, I must ask the user to run `/mcp`. I must not fail merely because an example tool prefix differs from the configured prefix.

---

## 2. Core Tool Restrictions
I am **strictly forbidden** from using Claude Code's standard filesystem tools (such as `Grep`, `Glob`, or broad `Read` operations) to locate C# classes, methods, fields, interfaces, or DTO structures unless I have **first attempted** to use the active semantic tools defined below.

---

## 3. Mandatory Semantic Workflow
For every exploration or refactoring task in the C# solution, I must execute this exact sequence:

1. **Broad Conceptual and Semantic Discovery**:
   - When the exact file, class, or symbol name is unknown, or when doing exploratory research, I must first run the available JetBrains Context semantic-search tool using focused natural-language queries.
2. **Precise Symbol Identification**:
   - Once a relevant class, interface, or method has been identified, or if the exact symbol name is already known, I must run the available Rider symbol-search or symbol-information tool.
   - I will rely on Rider's symbol definitions instead of manually opening files to find class members or parameter types.
3. **Usage and Dependency Mapping**:
   - I must run the available Rider implementation-discovery tool to find concrete vertical slice handlers (e.g., discovering all classes implementing `AbstractProcessor` or `IGeneratePdf`).
   - I must run the available Rider usage-discovery tool to identify exactly which files, mappings, or controllers will be affected by a refactor or change before editing them.
4. **Fallback Exception**:
   - I am only permitted to fall back to Claude Code filesystem tools (`Grep`, `Glob`, or targeted `Read` operations) if:
     - The active MCP tools (both Rider and JetBrains Context) return an explicit error or are offline.
     - The active MCP tools return zero results after searching.
     - The file being inspected is a non-C# file (e.g., `.json`, `.css`, `.yml`) that the semantic engines cannot parse.

---

## 4. Build & Diagnostics First
When verifying C# code changes, I must check the Rider MCP server's semantic diagnostics first, when that capability is available, before running terminal-level `dotnet build` commands. This allows Rider to catch type safety, import errors, and dependency diagnostics quickly, preventing compile-loop overhead. If diagnostic tools are not exposed, I must proceed directly to the repository's documented build and test commands.
