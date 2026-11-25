# sharp-codemode Protocol (Optional Integration)

This file governs the use of the sharp-codemode MCP server. The server is optional.
Every rule has a fallback for the case where the server is absent.

The server runs C# scripts against a typed API. The API wraps the file access and the
upstream MCP tools (Rider, JetBrains Context). A script chains many operations in one
call and returns only the final result. Scripts are stored on disk and reused.

---

## 1. Detect the Server

Run this check one time, at the start of each session, during the boot sequence:

1. Read the current list of available tools.
2. If the list contains `mcp__sharp-codemode__search`, treat the server as present.
3. If it does not, treat the server as absent.

State the result in one short line during the boot. Do not check again in the same
session.

If the server is absent, work as before. Do not treat the absence as a blocker.

---

## 2. Tools and Callers

| Tool | Purpose | Who may call it |
|---|---|---|
| `search` | Find API functions and saved scripts | Orchestrator, investigator, builder |
| `run_saved` | Run a saved script by name | Orchestrator (boot script only), investigator, builder |
| `execute_readonly` | Run new code against the read-only API | Builder |
| `execute` | Run new code against the full API. Optional `saveAs` saves it | Builder |

Names in the tool list have the prefix `mcp__sharp-codemode__`.

---

## 3. Orchestrator Rules

1. For the boot reads, I call `run_saved` with the name `boot-steering` before I read
   any file by hand. The script returns the compressed `.steering/` files,
   `.agent-local/activeContext.md`, and `.agent-local/sessionProgress.md`. It marks
   each missing file `absent`.
2. The boot evidence block stays mandatory. I write it from the script output. A file
   that is not in the output must not appear in the block.
3. If the script is missing or fails, I do the boot reads by hand, as before. I report
   this one time.
4. If itixo is present, I do not call `execute` or `execute_readonly`. I do not use a
   script to locate or to change code. That is delegation work
   (`itixo-integration.md` section 3).
5. If itixo is absent, I act in the roles of section 4 and section 5 myself, and the
   same tool rules apply to each role.
6. I use `search` only to learn which saved scripts exist, so that I can name one in a
   delegation prompt.

---

## 4. Investigator Role

1. I call `search` first. I use `run_saved` for the script that fits.
2. I do not call `execute` or `execute_readonly`. I do not write a new script.
3. If no saved script fits, I stop. I return this block to the orchestrator:

```text
NEED_SCRIPT
purpose: <one sentence>
inputs: <arguments>
output: <what the result must contain>
```

4. I do not search with `Grep`, `Glob`, or `Bash` as a replacement for the missing
   script, unless the fallback conditions of `local-tools.md` apply.

---

## 5. Builder Role

1. Before I write any script, I call `search`. If a saved script fits, I use it. If a
   saved script almost fits, I save a new script with a new name. I never overwrite a
   script.
2. I use `execute_readonly` for any task that only reads. I use `execute` only if the
   task needs a write.
3. If a script will be useful again, I save it with `saveAs` and a description. Use
   readonly mode whenever possible. A script that I save from `execute` has the mode
   `full`.
4. A `full` script cannot run through `run_saved` until the user approves it. I tell
   the user the exact command: `dotnet SharpCodeMode.dll approve <name>`. I never run
   this command myself. I never edit the approval file, the configuration file, or the
   log files.
5. A script belongs to one sub-task. I do not use a script to join sub-tasks (Rule of
   One).
6. A script never writes to `.steering/` or `.specs/`. Those writes follow
   `steering-contract.md`.
7. A script never installs a package and never calls the shell. The dependency
   lockdown applies.
8. I do not commit. I do not stage a script.

---

## 6. Script Rules

1. Write the header lines first: name, description, mode, params.
2. The name is lowercase kebab-case. Choose a name that tells the purpose.
3. Use arguments in `Args`. Do not write a fixed path or a fixed value in the body.
4. Return only what the next step needs. Do not print whole files if a filtered result
   is enough.
5. Write code comments in simple technical English. Follow `lang-csharp.md`.
6. Do not use reflection, `Process`, or direct file or network access. Use only the
   Codemode API.
7. Keep a script short. If it needs more than about 40 lines, split it into two
   scripts.

---

## 7. Errors

| Result starts with | Action |
|---|---|
| `COMPILE ERROR` | Read the diagnostics. Fix the code. Try again. Stop after three attempts and ask the user (Rule of Three). |
| `RUNTIME ERROR` | Read the message. Fix the cause. Do not retry the same code unchanged. |
| `TIMEOUT` | Do not retry the same code. Narrow the work (a smaller path, a filter) or split the script. |
| `[TRUNCATED` at the end | Narrow the script so that it returns less. Do not call it again with the same code. |
| Refusal of `run_saved` | Read the reason. If the script is not approved, tell the user the approve command. |
| Upstream error | Follow the fallback conditions in `local-tools.md`. Report the error one time. |

---

## 8. Fallback (Server Absent or Failing)

1. If the server is absent, work as before. Use the tools of the existing rule set.
2. If the server fails in a way that blocks the task, use the direct tools. If itixo is
   present, delegate as usual.
3. Report the failure in one sentence. Do not retry in a loop.
