# Claude Code Rule Set

This directory contains the Claude Code adaptation of the original `v2` rule set. The original `v2` directory remains unchanged.

## Installation Layout

### User-Level Rules and Skills

Copy the contents of `Global/.claude/` into the current user's Claude Code configuration directory:

- Windows: `%USERPROFILE%\.claude\`
- macOS and Linux: `~/.claude/`

This installs:

- global rules under `.claude/rules/`;
- the `/steering-manager` skill under `.claude/skills/steering-manager/`.

### Project-Level Rules

Copy the contents of `Project/.claude/` into the target repository's `.claude/` directory. The `local-tools.md` rule is project-level because it describes repository-specific Rider and JetBrains Context MCP usage.

### Prompt Template

`Prompts/chat-prompt.md` is a standalone prompt-authoring template adapted to generate copy-pasteable Claude Code prompts.

## Required Integrations

- Configure the Rider and JetBrains Context MCP servers in Claude Code before relying on semantic code intelligence. Use `/mcp` to verify the actual server and tool names.
- Configure the optional OpenPets MCP server before using the companion integration. Engineering work must continue if OpenPets is unavailable.
- The steering manager delegates compression to a separately installed `/caveman-compress` skill.

## Claude Code Controls

- Use `/model opus` for complex requirements and design work.
- Use `/model sonnet` for task planning and implementation.
- Use `/effort low`, `/effort medium`, `/effort high`, or `/effort xhigh` as directed by the governance rules and subject to organization policy.