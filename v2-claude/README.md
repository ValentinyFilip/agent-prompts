# Claude Code Rule Set

This directory contains the Claude Code adaptation of the original `v2` rule set. The original `v2` directory remains unchanged.

## Installation Layout

### User-Level Rules and Skills

Copy the contents of `Global/.claude/` into the current user's Claude Code configuration directory:

- Windows: `%USERPROFILE%\.claude\`
- macOS and Linux: `~/.claude/`

This installs:

- the mandatory operating contract at `.claude/CLAUDE.md`;
- global rules under `.claude/rules/`;
- the `/steering-manager` skill under `.claude/skills/steering-manager/`.

`CLAUDE.md` is the enforcement entry point. Claude Code loads it automatically at the start of each session. The file imports each rule file, declares the precedence order, and defines the boot sequence. Without this file, the rule files load as passive context and Claude can ignore them. Do not leave `.claude/CLAUDE.md` empty.

### Project-Level Files

Copy `Project/CLAUDE.md` to the root of the target repository. This file reinforces the user-level contract and imports the repository-local rule.

Place the project-level `local-tools.md` rule at `.agent-local/local-tools.md` in the target repository. The source template is `Project/.agent-local/local-tools.md`. Do not place the installed copy in the target repository's `.claude/rules/` directory.

### Prompt Template

`Prompts/chat-prompt.md` is a standalone prompt-authoring template adapted to generate copy-pasteable Claude Code prompts.

## Required Integrations

- Configure the Rider and JetBrains Context MCP servers in Claude Code before relying on semantic code intelligence. Use `/mcp` to verify the actual server and tool names.
- Configure the optional OpenPets MCP server before using the companion integration. Engineering work must continue if OpenPets is unavailable.
- Install the `/simple-english` skill. The global rules use it as the operational ASD-STE100 specification.
- The steering manager performs Caveman compression itself. No external compression skill is required.

## Claude Code Controls

- Use `/model opus` for complex requirements and design work.
- Use `/model sonnet` for task planning and implementation.
- Use `/effort low`, `/effort medium`, `/effort high`, or `/effort xhigh` as directed by the governance rules and subject to organization policy.