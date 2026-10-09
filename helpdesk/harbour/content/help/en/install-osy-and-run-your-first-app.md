---
slug: install-osy-and-run-your-first-app
title: Install Osy# and run your first app
summary: From an empty folder to an app running in your browser, with no account and no database to set up.
category: getting-started
section: install-and-run
audience: Everyone
position: 1
version: 1
---
You can build and run an Osy# app on your own machine for free. You need no account, no Docker and no database server.

## Install

On macOS or Linux:

```
curl -fsSL https://raw.githubusercontent.com/osyrin-platform/cli/main/install.sh | sh
```

On Windows, in PowerShell:

```
irm https://raw.githubusercontent.com/osyrin-platform/cli/main/install.ps1 | iex
```

The installer checks the download against its published SHA-256 and installs to `~/.osy/bin`. You get the compiler, the runtime, a local PostgreSQL and two commands:

- `osy` builds, checks and runs your app on your machine.
- `osyrin` deploys it and operates it in production.

## Create a project

1. Make an empty folder and go into it:

   ```
   mkdir my-tasks && cd my-tasks
   ```

2. Create the project:

   ```
   osy init --agent claude
   ```

   This writes a small working app (a page, a function, a theme and a test) and a `CLAUDE.md` brief for your coding agent. Use `--agent agent` to get an `AGENTS.md` instead.

To start from a complete sample instead, give its name, for example `osy init kanban --agent claude`. `osy docs sample` lists the samples.

## Run it

```
osy launch
```

`osy launch` starts the local platform if it is not running, compiles your source and opens the app at `http://<your-app>.localhost:<port>/`. Change a file and run `osy launch` again to see the change.

## Check it

```
osy check
```

`osy check` validates the source, runs the production lint and runs your tests, then gives one verdict. It names anything weak and says how to fix it.

## When you are done

`osy stop` stops the local platform for this project. The next `osy launch` starts it again.

## Questions the toolchain answers

- `osy docs <topic>` answers how to write something, for example `osy docs "hash a password"`.
- `osy kit` lists the UI controls you can use.
- `osy explain` says who can read and write each kind of data.

## Related

- [Your first compile error: how to read it and fix it](/help-centre/a/read-and-fix-a-compile-error)
- [Add a kit to your app](/help-centre/a/add-a-kit-to-your-app)
- [Deploy your app to Osyrin Cloud](/help-centre/a/deploy-your-app-to-osyrin-cloud)
