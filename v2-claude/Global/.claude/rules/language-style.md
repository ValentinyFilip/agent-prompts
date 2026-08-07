# Language Style Protocol: Caveman and ASD-STE100

This rule is the single source of truth for language style. Other rules must reference this rule instead of duplicating its requirements.

---

## 1. Select the Output Style

Use **Caveman style** only for these outputs:

- Caveman-compressed `.md` counterparts in `.steering/` and `.specs/`.
- local memory files in `.agent-local/`.
- prompts that explicitly require Caveman shorthand.

Use **ASD-STE100 Simplified Technical English (STE)** for these outputs:

- direct chat messages.
- human-readable `*.original.md` files.
- technical documentation and Markdown specifications.
- code comments and user-facing technical text.
- generated explanations in standard codebase files.

Do not apply either language style to source-code syntax, identifiers, commands, file paths, product names, configuration keys, API endpoints, quoted errors, or log messages.

If categories overlap, use Caveman style for compressed counterparts and local memory files.

---

## 2. Caveman Style

Write short fragments. If meaning stays clear, remove articles, auxiliary verbs, filler, and repeated context.

Use keywords, symbols, and logical arrows. Preserve every technical fact and normative constraint in compressed memory files.

Example:

```text
Tests fail -> fix imports -> build green.
```

Never use Caveman style in a human-readable original, standard codebase file, or code comment.

---

## 3. ASD-STE100 Style

Invoke `/simple-english` for generated technical prose. Use its **strict ASD-STE100 mode**.

The skill is the operational ASD-STE100 specification. Apply its rule catalog, vocabulary discipline, untouchables, and mandatory self-check.

For direct chat messages, use strict ASD-STE100 and keep the text concise.

1. Omit conversational pleasantries.
2. Do not repeat the user's instructions or restate the problem.
3. Do not give unsolicited explanations of code changes or file contents.
4. Give sufficient information for decisions, approvals, errors, and next actions.

Apply these minimum rules:

1. Classify text as procedural or descriptive before writing it.
2. Use one instruction in each procedural sentence.
3. Use no more than 20 words in a procedural sentence.
4. Use no more than 25 words in a descriptive sentence.
5. Put a condition before its instruction.
6. Use approved words, project terms, or necessary technical terms.
7. Use one word for one meaning. Do not change terms only for style.
8. When the actor is known, use active voice.
9. Use the imperative form for instructions.
10. Use simple present, simple past, or simple future tense for descriptions.
11. Do not use contractions, semicolons, slang, filler, or decorative language.
12. Use one topic in each paragraph. Use no more than six sentences in a descriptive paragraph.
13. Keep EARS keywords and syntax exact in requirements.
14. Keep diagrams, tables, and glossaries clear and consistent.

---

## 4. Examples

### Procedural text

Before:

```text
Increase the timeout if the network is slow, and then verify that the request completes successfully.
```

After:

```text
If the network is slow, increase the timeout.
Make sure that the request completes.
```

### Descriptive text

Before:

```text
The service is responsible for carrying out validation and it also provides useful information about any errors that may occur.
```

After:

```text
The service validates the input. It reports each error.
```

### EARS requirement

```text
WHEN the token expires, THE authentication service SHALL reject the request.
```

---

## 5. Self-Check

Before writing a generated file, make sure that the text complies with this rule.

1. Count the words in the three longest sentences.
2. Remove contractions, semicolons, filler, and unnecessary progressive verb forms.
3. Move each `if` or `when` condition before its instruction.
4. Use the same term for the same meaning throughout the document.
5. Make sure that Caveman style does not occur in STE-controlled files.
6. Make sure that STE expansion does not occur in Caveman-controlled files.