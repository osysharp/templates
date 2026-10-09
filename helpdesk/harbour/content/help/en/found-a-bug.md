---
slug: found-a-bug
title: Report a bug
summary: How to tell us something is wrong, and what to include so we can fix it fast.
category: getting-started
section: getting-help
audience: Everyone
position: 1
version: 1
---
Found something that does not work? Tell us. A clear description gets a fix sooner, and a bug in the toolchain usually comes with details you can copy straight from your terminal.

## Where to tell us

- **The Get help page.** Choose **Ask the team** in this help centre. Write a one-line **Subject** and say **What happened**. As you type the subject, the page offers articles that may already answer it. Choose **Send request** and you get a reference number. We answer by mail.
- **The chat.** Choose **Chat with us** in the corner of any help-centre page. It searches these articles first, and your first message reaches a person.
- **Your requests.** Sign in to follow everything you have sent us, reply, and attach files such as screenshots.
- **A bug in the `osy` toolchain itself.** Run `osy feedback` in your project. It opens a prefilled issue on the toolchain's public tracker, github.com/osysharp/cli, with your version and the last error already filled in. You review it before you submit. `osy feedback --full` adds a diagnostic bundle, which is uploaded privately and never attached to the public issue. Your source goes in only if you add `--with-source`.

## What to include

1. **What you did, what you expected, and what happened.** Paste the exact error text, not a summary of it.
2. **Your version.** Run `osy --version` and paste the line it prints.
3. **The correlation id.** One action in your app writes its browser and server lines under one id. `osy logs --errors` shows recent failures with their ids. `osy logs --corr <id>` shows everything for one of them.
4. **The trace, for a failure inside your app.** `osy inspect` lists the runs it has kept, with failed ones marked. `osy inspect <trace-id> --fault` jumps to the step that failed.
5. **The smallest source that shows it.** If the problem is in your Osy# code, a few lines that still fail are worth more than the whole app.

## What never to send

Never paste a password, an API key or the contents of your `.secrets` file. `osy feedback --full` leaves them out on its own, and lists everything in the bundle before anything leaves your machine.

## For a deployed app

`osyrin app logs` reads your deployed app's log lines. Add `--errors` to see only failures, and include the correlation id from the line you mean.

## Related

- [Your first compile error: how to read it and fix it](/help-centre/a/read-and-fix-a-compile-error)
- [Install Osy# and run your first app](/help-centre/a/install-osy-and-run-your-first-app)
- [Deploy your app to Osyrin Cloud](/help-centre/a/deploy-your-app-to-osyrin-cloud)
