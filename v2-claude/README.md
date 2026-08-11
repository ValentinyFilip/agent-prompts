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

### Project-Level Semantic Tool Rule

Place the project-level `local-tools.md` rule at `.agent-local/local-tools.md` in the target repository. The source template remains in `Project/.claude/rules/local-tools.md` because this rule set is a distribution package; do not place the installed copy in the target repository's `.claude/rules/` directory.

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