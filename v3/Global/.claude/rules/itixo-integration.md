# Itixo Delegation Protocol (Optional Integration)

This file is one instantiation of the generic "agentic plugin integration"
contract that the rest of this rule set references by that name. It governs
interaction with the itixo plugin specifically. The itixo plugin is
optional, and this file itself is optional in the rule set: other rule
files call out to `rules/itixo-integration.md` only when it exists. Every
rule in this file has a defined fallback for the case where the plugin is
absent.

---

## 1. Detect Itixo

Run this check once, at the start of each session, before Phase 0 finishes:

1. Read the current list of available agent types.
2. Read the current list of available skills.
3. If the agent list contains `itixo-investigator`, `itixo-builder`,
   `itixo-tester`, `itixo-reviewer`, or the skill list contains
   `itixo:dirigent`, treat itixo as **present** for this session.
4. If none of these appear, treat itixo as **absent** for this session.

State the detected result one time, in one short line, during Phase 0.
Do not re-check itixo presence inside a single session.

---

## 2. Delegation Map (Itixo Present)

When itixo is present, delegate the listed action to the listed agent.
Do not perform the action directly.

| Action | Delegate to | Model tier |
|---|---|---|
| Locate code, map a codebase area, answer "where is X" | `itixo-investigator` | haiku |
| Implement one precisely specified change | `itixo-builder` | sonnet |
| Create a new `.steering/` or `.specs/` file pair | `itixo-builder` | sonnet |
| Update an existing `.steering/` file pair | `itixo-docs-updater` | sonnet (override) |
| Update an existing `.specs/` file pair (content change) | `itixo-builder` | sonnet |
| Mark a `.specs/` `tasks` sub-task checkbox complete after verification | `itixo-docs-updater` | haiku |
| Write or run tests for specified behavior | `itixo-tester` | sonnet |
| Review a diff, branch, or file for correctness/regression risk | `itixo-reviewer` | sonnet |
| Review a diff for security risk on explicit user request | `itixo-security-reviewer` | opus, max effort |
| Sync documentation with a code change | `itixo-docs-updater` | haiku |
| Create a GitHub issue from a request or bug report | `itixo-github-issues` | sonnet |

Use the canonical `itixo-*` agent ID. Apply the listed default model tier.
Apply a different model or effort only on an explicit, current user request.

Two rows carry a fixed override of the agent's own default tier. Apply the
override every time, without a new user request:

- `itixo-docs-updater` runs on sonnet for a `.steering/` update, not on its
  haiku default. Caveman compression under `steering-contract.md` section 4
  must keep every normative constraint and must keep `MUST`, `SHALL`,
  `SHOULD`, and `MAY` distinct.
- `itixo-security-reviewer` runs on opus at max effort.

**No delegated agent commits.** The itixo agent definitions end their workflow
with a commit step. That step is disabled in this rule set. Each template in
section 6 carries a do-not-commit constraint. The orchestrator runs
`git status` and proposes the commit message to the user, per
`governance-rules.md` section 2.4.

---

## 3. Orchestrator Hard Boundaries (Itixo Present)

When itixo is present, this rule set, acting as orchestrator, does not:

- run `Grep`, `Glob`, or `Bash` (`ls`, `find`, `grep`) to locate code;
- edit or write a repository code file;
- write or edit a `.steering/` or `.specs/` file directly;
- write or run a test suite;
- produce inline review findings for a non-trivial diff;
- hand-write a GitHub issue;
- rewrite documentation content.

Delegate each of these actions through the table in section 2. See section 5
for the two-phase delegation required on `.steering/` and `.specs/` writes.

The orchestrator still performs: `.agent-local/` memory updates, the
itixo detection check, relaying every halt and every subagent question to
the user, `git status`, and commit proposals. These stay with the
orchestrator because they are either trivial or because they are the
conversational thread the user is actually part of.

---

## 4. Subagent Prompt Contract Addition

When delegating to an itixo agent, add this instruction to the subagent
prompt, in addition to goal, exact paths, constraints, and expected output:

> Read `.agent-local/local-tools.md` first, if it exists. If a listed
> semantic tool covers this lookup or edit, use that tool before `Grep`,
> `Glob`, or `Bash`. Fall back to `Grep`, `Glob`, or `Bash` only under the
> fallback conditions stated in `local-tools.md`.

This keeps semantic-tool priority in effect even when the acting agent is
an itixo agent rather than the top-level orchestrator.

---

## 5. Steering and Spec Safety Under Delegation

`itixo-builder` and `itixo-docs-updater` may write `.steering/` and `.specs/`
files, but only through a two-phase delegation. The halt for user approval
still surfaces through the orchestrator; it never happens silently inside
the subagent.

**Select the acting agent first:**

- A new `.steering/` or `.specs/` pair does not exist yet. Authoring it needs
  full authoring judgment. Delegate to `itixo-builder`.
- An existing `.steering/` pair needs a change. The change is a sync of a
  durable document to a known new fact. Delegate to `itixo-docs-updater`.
- An existing `.specs/` pair needs a content change (new or revised
  requirement, design section, or task). This needs authoring judgment.
  Delegate to `itixo-builder`.
- An existing `.specs/` `tasks` pair needs only a sub-task checkbox flipped
  to `[x]` after that sub-task's own verification passed. This is evidenced
  sync, not authoring, and does not carry a `.steering/`-style approval
  halt. Delegate to `itixo-docs-updater`, per section 6's tick-update
  template. Do this immediately after each sub-task, per the Rule of One in
  `sdd-tdd-engine.md`.

Use the same acting agent for Phase A and Phase B of one write. Do not change
the agent between the two phases.

**Phase A — write the original:**

1. Delegate to the selected agent. The task instructs it to: apply
   `.claude/rules/steering-contract.md` section 5, Stage 1, for the
   requested `create`/`update` on the given `<base>` path; write only
   `<base>.original.md`, then stop without writing `<base>.md`.
2. The agent reports the written original (or its diff) back to the
   orchestrator.
3. The orchestrator relays this to the user and halts for explicit
   approval, exactly as it would if it had written the file itself.

**Phase B — compress, after approval:**

1. Once the user approves, the orchestrator delegates a second, small task
   to the same agent: apply `steering-contract.md` section 5, Stage 2, to
   generate `<base>.md` from the now-approved `<base>.original.md`.
2. The agent reports the written counterpart. The orchestrator
   confirms completion to the user.

Never let a subagent write `<base>.md` before the user has approved
`<base>.original.md`. Never let it skip the six-field frontmatter contract.

`itixo-docs-updater` refuses a documentation change with no source evidence.
Supply the evidence in the Phase A prompt: the code change already made, the
`file:line` findings of an investigation, or an explicit fact of the user.
If no such evidence exists, the update is an authoring task. Delegate it to
`itixo-builder` instead.

Spec Mode halts (requirements, design, task plan) stay decided by the
orchestrator; only the file write is delegated. Do not delegate the
authoring decision itself to `itixo-planner`; that agent does not carry
this rule set's dual-file, EARS, and steering-precedence rules.
`itixo-investigator` may still support Spec Mode by mapping existing code
before requirements or design are written.

---

## 6. Delegation Prompt Templates

Every delegation prompt uses one fixed base shape: goal, exact paths when
known, constraints, expected output, and a no-expansion boundary. This rule
set does not delegate to `itixo:dirigent`; the orchestrator role it would
play stays with this rule set's own phase gates and halts. This section adds
the exact fields for each row of
the section 2 table, so the orchestrator does not have to reassemble the
contract from memory each time. Fill every bracket; do not delegate with a
bracket left unfilled.

### `itixo-investigator`
```
Goal: <one question or one area to map>
Scope: <paths or package/module names already known, or "unknown, search from repo root">
Constraints: read-only. No fixes, no design, no opinions.
Expected output: grouped file:line-symbol-note evidence.
Semantic-tool clause: see section 4.
```

### `itixo-builder` (generic implementation)
```
Goal: <the one precise change>
Scope: <exact file path(s) to touch>
Constraints: <what NOT to touch> + no-expansion boundary + Dependency Lockdown (governance-rules.md §2.3)
             + do NOT commit; leave the change unstaged.
Expected output: diff summary + verification result.
Semantic-tool clause: see section 4.
```

### `itixo-builder` (new `.steering/` pair, or any `.specs/` write — Phase A, original only)
```
Goal: <create|update> <exact base path>.original.md, applying
      steering-contract.md section 5 Stage 1 directly (no skill).
Scope: <base path> only. Do not write <base path>.md in this call.
Constraints: six-field frontmatter contract (steering-contract.md §2, .steering/ only),
             ASD-STE100 (language-style.md §3); do NOT commit; leave the file unstaged.
Expected output: the written original's full content or diff, for my relay to the user.
Semantic-tool clause: see section 4.
```

### `itixo-builder` (new `.steering/` pair, or any `.specs/` write — Phase B, compression, post-approval)
```
Goal: generate <base path>.md from the now-approved <base path>.original.md,
      applying steering-contract.md section 4 and section 5 Stage 2 directly.
Scope: <base path>.md only.
Constraints: six-field frontmatter identical to the original (.steering/ only);
             Caveman style (language-style.md §2); every technical fact preserved;
             do NOT commit; leave the file unstaged.
Expected output: confirmation the counterpart was written and re-read.
```

### `itixo-docs-updater` (existing `.steering/` pair update — Phase A, original only)
```
Model: sonnet (fixed override of the haiku default; see section 2).
Goal: update <exact base path>.original.md, applying steering-contract.md
      section 5 Stage 1 directly (no skill).
Source evidence: <the code change already made, the file:line findings, or the
      explicit fact of the user that this update records>.
Scope: <base path>.original.md only. Do not write <base path>.md in this call.
       Do not touch any other .steering/ file.
Constraints: read the existing original and its counterpart first;
             edit only the affected sections, no wholesale rewrite;
             keep the six-field frontmatter contract (steering-contract.md §2) and
             set last_updated to the current local date;
             ASD-STE100 (language-style.md §3);
             do NOT commit; leave the file unstaged.
Expected output: diff of the original, for my relay to the user.
Semantic-tool clause: see section 4.
```

### `itixo-docs-updater` (existing `.steering/` pair update — Phase B, compression, post-approval)
```
Model: sonnet (fixed override of the haiku default; see section 2).
Goal: regenerate <base path>.md from the now-approved <base path>.original.md,
      applying steering-contract.md section 4 and section 5 Stage 2 directly.
Scope: <base path>.md only.
Constraints: six-field frontmatter identical to the original;
             Caveman style (language-style.md §2); every technical fact and every
             normative term (MUST/SHALL/SHOULD/MAY) preserved and distinct;
             do NOT commit; leave the file unstaged.
Expected output: confirmation the counterpart was written and re-read.
```

### `itixo-docs-updater` (`.specs/` `tasks` checkbox tick, single pass)
```
Goal: mark sub-task <wave/id> `[x]` in both tasks.original.md and tasks.md
      for the given feature.
Source evidence: <the verification result — test pass, build green, or
      completed review — that proves this sub-task is done>.
Scope: the one checkbox line in <feature path>/tasks.original.md and the
       matching line in <feature path>/tasks.md only. Do not touch any other
       line, task, or file.
Constraints: no approval halt needed for this write, it is a mechanical sync
             of a fact already verified; do NOT commit; leave the files
             unstaged.
Expected output: confirmation of both lines updated.
Semantic-tool clause: see section 4.
```

### `itixo-tester`
```
Goal: <behavior to cover, red/green/refactor stage>
Scope: <test file path(s)>; implementation file(s) to read, not edit.
Constraints: test-file-only edits; do NOT commit; leave the file unstaged.
Expected output: pass/fail result of the run, and the test file diff.
Semantic-tool clause: see section 4.
```

### `itixo-reviewer` / `itixo-security-reviewer`
```
Goal: review <diff/branch/file> for <correctness|security> risk.
Scope: <the changed paths>.
Constraints: read-only, no edits.
Expected output: path:line: blocker|warn|nit: problem. fix. (or "No findings.")
```

### `itixo-docs-updater`
```
Goal: sync <doc path(s)> with <the code change already made>.
Scope: <doc path(s)> only.
Constraints: edit only affected/supported sections; preserve existing style.
Expected output: diff of the doc change.
```

### `itixo-github-issues`
```
Goal: <the request or bug report to structure>.
Scope: <target GitHub repo>.
Constraints: max 2 parent-child edges (Feature -> Feature -> Task); use native
             IssueTypes when available.
Expected output: created issue URL(s) and their hierarchy.
```

---

## 7. Parallel Dispatch

`itixo-investigator` delegations may run in parallel batches of up to
three, because they change no files. `itixo-builder` and `itixo-tester`
delegations stay sequential, one sub-task at a time, per the Rule of One in
`governance-rules.md`. Concurrent file-changing delegations risk conflicting
edits and are not permitted.

---

## 8. Fallback (Itixo Absent)

When itixo is absent, perform every action in the section 2 table directly,
using `Read`, `Grep`, `Glob`, `Edit`, `Write`, and `Bash`, governed by the
tool hierarchy in `.agent-local/local-tools.md`. Section 3 boundaries do not
apply. Continue the task. Do not treat the absent plugin as a blocker.
