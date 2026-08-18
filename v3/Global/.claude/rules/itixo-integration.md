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

Delegation is automatic and permanent. It is the default execution path of this
rule set, not an option:

- Do not wait for the user to ask for delegation.
- Do not ask the user for permission to delegate.
- Do not offer a direct alternative to delegation.
- Do not treat a small task, a fast task, or a one-file task as an exception.
- Silence of the user is not permission to act directly.
- Only an explicit and current instruction of the user ("do it yourself", "no
  subagents") suspends delegation, and only for that one task.

If you start an action from the table directly, stop at once, report the
deviation in one sentence per the operating contract section 6, then delegate
the action.

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

These are negative rules. A softer statement in another rule never overrides
them. The first inline search is already a violation: delegate before you
search, not after the first result disappoints.

The orchestrator is not a worker. It thinks, it decomposes, it integrates, and
it talks to the user. It does not do the work of a role in the section 2 table.

Delegate each of these actions through the table in section 2. See section 5
for the two-phase delegation required on `.steering/` and `.specs/` writes.

**Permitted read.** To make a cross-step judgment, the orchestrator may read one
file that is already located, for example a file named by the user or a file
reported by `itixo-investigator`. Reading one known file is permitted.
Discovery of where a thing is, is not permitted.

The orchestrator still performs: the boot reads of `.steering/` and
`.agent-local/` (operating contract section 3), `.agent-local/` memory updates,
the itixo detection check, the pre-delegation contract in
`governance-rules.md` section 4, relaying every halt and every subagent
question to the user, `git status`, and commit proposals. These stay with the
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
Write-work discipline (all five steps, in order):
  1. Read each target file before you edit it.
  2. Make the smallest change that satisfies the goal.
  3. Inspect the dependencies of the changed symbols, inside the given scope only.
  4. Inspect your own diff before you report.
  5. Run verification proportionate to the change (build, lint, or the named test).
Expected output: diff summary + verification result.
Semantic-tool clause: see section 4.
```

### `itixo-builder` (new `.steering/` pair, or any `.specs/` write — Phase A, original only)
```
Goal: <create|update> <exact base path>.original.md, applying
      steering-contract.md section 5 Stage 1 directly (no skill).
Scope: <base path> only. Do not write <base path>.md in this call.
Constraints: six-field frontmatter contract (steering-contract.md §2),
             ASD-STE100 (language-style.md §3); do NOT commit; leave the file unstaged.
Expected output: the written original's full content or diff, for my relay to the user.
Semantic-tool clause: see section 4.
```

### `itixo-builder` (new `.steering/` pair, or any `.specs/` write — Phase B, compression, post-approval)
```
Goal: generate <base path>.md from the now-approved <base path>.original.md,
      applying steering-contract.md section 4 and section 5 Stage 2 directly.
Scope: <base path>.md only.
Constraints: six-field frontmatter identical to the original;
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
Scope: <target GitHub repo, owner, and known project or IssueType conventions>.
Constraints: one agent owns assessment and creation together; do not split the
             duplicate check from the creation;
             check for a duplicate issue first;
             determine whether native GitHub IssueTypes are available;
             with native IssueTypes: root `Feature`, direct children `Task` by
             default, a direct child `Feature` only when that child is split into
             executable children;
             max 2 parent-child edges (Feature -> Feature -> Task);
             use the lowercase `feature` and `task` label fallback only when
             native IssueTypes are unavailable in a personal repository; create
             only the missing fallback labels; use existing labels otherwise;
             return an unknown repository, outcome, scope, or success criterion
             to me as a user question; do NOT commit.
Expected output: decision and rationale; issue URL(s) or number(s); linked
             sub-issues and parallel waves; IssueType availability plus type
             readback evidence, or the personal-repository fallback
             justification plus label creation and assignment readback
             evidence; parent-child hierarchy and depth evidence; blockers.
```

---

## 7. Parallel Dispatch

`itixo-investigator` delegations may run in parallel batches of up to
three, because they change no files. `itixo-builder` and `itixo-tester`
delegations stay sequential, one sub-task at a time, per the Rule of One in
`governance-rules.md`. Concurrent file-changing delegations risk conflicting
edits and are not permitted.

---

## 8. Security-Review Lifecycle

This section applies when the user explicitly asks for a security review.
`itixo-security-reviewer` runs on opus at max effort, per section 2.

1. The security reviewer is read-only. It never edits a file.
2. On a pull request, publish each finding inline. If an inline comment is not
   possible, publish one general pull request comment.
3. If a Critical or High finding stays unresolved, submit `REQUEST_CHANGES`. If
   the platform does not permit it, submit `COMMENT` and identify the review as
   a self-review.
4. If no finding stays open, publish one neutral clean-review comment.
5. Outside a pull request, report the findings in chat in the same order of
   severity.

**Automatic remediation.** The orchestrator owns the decision to remediate. It
delegates the fix; it does not write the fix itself. Automatic remediation is
permitted only for a localized fix that keeps the behavior outside the
vulnerability unchanged, and that needs none of these:

- a dependency update or a version update;
- a data migration;
- a public API change;
- an authentication or authorization policy decision;
- a secret rotation;
- an architecture change.

For a permitted fix: publish the finding, delegate the fix to `itixo-builder`,
delegate the validation to `itixo-tester`, then reply to the finding and resolve
it. Keep every other finding open for the decision of the user.

---

## 9. Fallback (Itixo Absent)

When itixo is absent, perform every action in the section 2 table directly,
using `Read`, `Grep`, `Glob`, `Edit`, `Write`, and `Bash`, governed by the
tool hierarchy in `.agent-local/local-tools.md`. Section 3 boundaries do not
apply. Continue the task. Do not treat the absent plugin as a blocker.
