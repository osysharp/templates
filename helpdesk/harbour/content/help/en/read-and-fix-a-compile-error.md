---
slug: read-and-fix-a-compile-error
title: "Your first compile error: how to read it and fix it"
summary: Every error names the file, the line, what is wrong and usually the fix. Here is how to read one.
category: building-your-app
section: the-compiler
audience: Everyone
position: 1
version: 1
---
Osy# checks your whole app before it runs, so most mistakes show up as a compile error on your own machine, in seconds, rather than as a broken page later.

## Check your source

```
osy validate
```

`osy validate` parses your source, resolves every name and runs the same checks a compile does. It needs no server, no database and no account. `osy check` runs the same validation, then the lint and your tests, and gives one verdict.

## Read the error

Here is a real one. The code asks for `n.Titel`, but the field is called `Title`:

```
model/count.osy:2:28  ERROR  RESOLVE_ERROR  entity 'Note' has no property 'Titel'. Did you mean 'Title'?
  → Title
  → for more, RUN: osy docs query-index …

✗ 1 error
```

Read it left to right:

1. **Where:** `model/count.osy:2:28` is the file, the line and the column. Most editors open the spot when you click it.
2. **How bad:** `ERROR` stops the build. A warning does not.
3. **Which check:** `RESOLVE_ERROR` means a name did not match anything. A parse error means the text itself is not valid Osy#.
4. **What is wrong,** in a sentence, often with a suggestion: `Did you mean 'Title'?`
5. **The fix,** after the arrow. Here it is the right name.
6. **Where to read more:** the `osy docs` topic that explains the feature.

## Fix it

1. Fix the **first** error first. One mistake can cause several more below it.
2. Run `osy validate` again.
3. Repeat until it prints `✓ Validated successfully`.

New errors can appear after you fix the first ones. That is normal: some checks only run once the source resolves cleanly, and the output says so.

## When the message is not enough

- **Look up the feature:** `osy docs <topic>`, for example `osy docs security` or `osy docs "hash a password"`.
- **Check a UI control's exact signature:** `osy kit <Control>`, for example `osy kit Card`.
- **See what the compiler understood:** `osy model` shows your entities, relations and functions as it resolved them.

## Let your coding agent do it

If you build with an agent, it reads the same output. A project made with `osy init` already tells the agent to run `osy validate` and `osy check`, so it can find and fix most errors on its own.

If you believe the compiler is wrong, run `osy feedback`. It opens a prefilled issue with your version and the last error.

## Related

- [Install Osy# and run your first app](/help-centre/a/install-osy-and-run-your-first-app)
- [Security rules: who can read and write what](/help-centre/a/security-rules-who-can-read-and-write-what)
- [Report a bug](/help-centre/a/found-a-bug)
