# Claude Code Rule Set (v3)

This directory contains the v3 rule set. It targets Claude Code only. It carries
forward the `v2-claude` rule set and adds optional delegation to the `itixo` plugin.

## What Changed From v2-claude

- New rule file `Global/.claude/rules/itixo-integration.md` defines detection of the
  itixo plugin, a delegation map to its agents, orchestrator hard boundaries while
  itixo is present, a per-action delegation prompt template for each agent, and a
  fallback for when the plugin is absent.
- `CLAUDE.md` (global and project), `sdd-tdd-engine.md`, `spec-mode-engine.md`,
  `vibe-mode.md`, and `governance-rules.md` each add a pointer to
  `itixo-integration.md` at the point where they would otherwise run a direct
  investigation, build, test, or review step.
- `.steering/` and `.specs/` writes delegate to `itixo-builder` through a two-phase
  process when itixo is present: it writes the `*.original.md` file, the orchestrator
  relays it and halts for the user's approval, and only after approval does the
  orchestrator delegate the Caveman-compression step. The orchestrator still decides
  when a write is needed and still owns every halt; only the file write moves to the
  subagent. See `itixo-integration.md` section 5.
- The `/steering-manager` skill is removed. It was known to fail on this machine. Its
  contract (six-field frontmatter, path mapping, compression, halt-for-approval) now
  lives directly in the new rule file `Global/.claude/rules/steering-contract.md`,
  applied by the orchestrator itself, or by a delegated `itixo-builder`. No skill is
  in the loop for `.steering/`/`.specs/` writes.
- `language-style.md`, `open-pets.md`, `local-tools.md`, and the `Prompts/` templates
  carry over unchanged. `memory-bank.md` and `governance-rules.md` are updated to
  point at `steering-contract.md` instead of the removed skill.

## Itixo Is Optional

The itixo plugin is never a requirement. Each rule file that mentions itixo states
its fallback: perform the action directly with `Read`, `Grep`, `Glob`, `Edit`,
`Write`, and `Bash`. Detection runs once per session; see
`Global/.claude/rules/itixo-integration.md` section 1.

## Installation Layout

### User-Level Rules and Skills

Copy the contents of `Global/.claude/` into the current user's Claude Code configuration directory:

- Windows: `%USERPROFILE%\.claude\`
- macOS and Linux: `~/.claude/`

This installs:

- the mandatory operating contract at `.claude/CLAUDE.md`;
- global rules under `.claude/rules/`, including `itixo-integration.md` and
  `steering-contract.md`.

This rule set ships no skills of its own. `steering-contract.md` replaced the
`/steering-manager` skill; `.steering/`/`.specs/` writes now apply that rule directly.

`CLAUDE.md` is the enforcement entry point. Claude Code loads it automatically at the start of each session. The file imports each rule file, declares the precedence order, and defines the boot sequence. Without this file, the rule files load as passive context and Claude can ignore them. Do not leave `.claude/CLAUDE.md` empty.

### Project-Level Files

Copy `Project/CLAUDE.md` to the root of the target repository. This file combines the general coding guidelines used across normal projects with the enforcement layer of this rule set. It imports the repository-local rule, defines the alignment sequence, and lists the non-negotiable behaviors.

Place the project-level `local-tools.md` rule at `.agent-local/local-tools.md` in the target repository. The source template is `Project/.agent-local/local-tools.md`. Do not place the installed copy in the target repository's `.claude/rules/` directory. When itixo is present and a task is delegated to one of its agents, the delegation prompt still points that agent at this same file, so the semantic-tool priority holds under delegation too.

### Prompt Template

`Prompts/chat-prompt-spec.md` and `Prompts/chat-prompt-vibe.md` are standalone prompt-authoring templates adapted to generate copy-pasteable Claude Code prompts.

## Required Integrations

- Configure the Rider and JetBrains Context MCP servers in Claude Code before relying on semantic code intelligence. Use `/mcp` to verify the actual server and tool names.
- Configure the optional OpenPets MCP server before using the companion integration. Engineering work must continue if OpenPets is unavailable.
- Install the itixo plugin (`/plugin marketplace add`, then `/plugin install`) before relying on itixo delegation. Engineering work must continue if itixo is unavailable.
- Install the `/simple-english` skill. This skill is mandatory. The global rules use it as the operational ASD-STE100 specification.

## Claude Code Controls

- Use `/model opus` for complex requirements and design work.
- Use `/model sonnet` for task planning and implementation.
- Use `/effort low`, `/effort medium`, `/effort high`, or `/effort xhigh` as directed by the governance rules and subject to organization policy.
- A delegated itixo agent keeps its own default model tier from `itixo-integration.md` section 2 unless the user explicitly overrides it.
