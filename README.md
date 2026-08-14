# Promps

This repository holds a Claude Code rule set for spec-driven and rapid ("Vibe")
development. The rule set defines a boot sequence, a dual-file memory architecture
(`.steering/`, `.specs/`, `.agent-local/`), and two operating modes. The repository
also holds the iterations that led to the current rule set.

## Directories

| Directory | Status | Content |
|---|---|---|
| `v1` | superseded | first draft of the rule files, ungrouped, no install layout |
| `v2` | superseded | grouped `Global`/`Project`/`Prompts` layout, supported Cline |
| `v2-claude` | superseded | `v2` adapted for Claude Code only |
| `v3` | **current** | `v2-claude` plus optional delegation to the itixo plugin |

Use `v3`. Earlier directories stay in the repository as history; do not install them.

## v3 in one paragraph

`v3` targets Claude Code only. It keeps the dual-file steering/spec memory, the
Spec Mode and Vibe Mode engines, and the ASD-STE100/Caveman language split from
`v2-claude`. It adds two new rules: `itixo-integration.md`, which detects the
optional itixo plugin and delegates investigation, implementation, testing, review,
and `.steering/`/`.specs/` writes to itixo's agents when the plugin is installed; and
`steering-contract.md`, which replaces the `v2-claude` `/steering-manager` skill with
a rule the orchestrator (or a delegated `itixo-builder`) applies directly. Every
itixo-touching rule states a fallback for direct action when the plugin is absent.

See `v3/README.md` for installation steps and required integrations.
