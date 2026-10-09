---
slug: deploy-your-app-to-osyrin-cloud
title: Deploy your app to Osyrin Cloud
summary: Sign in, create the app once, and deploy it to your organization's production environment.
category: deploying
section: osyrin-cloud
audience: Everyone
position: 1
version: 2
---
The app you run on your machine is the app you deploy. There is no container, database or pipeline to set up: Osyrin Cloud provides them.

## Before you start

Osyrin Cloud is in beta. Apply from the Plans page on osyrin.com. You sign in with a GitHub account that is at least 30 days old. When we approve your application, you get an organization with an environment called `production`.

## Deploy

1. **Sign in** from your project folder:

   ```
   osyrin login --server osyrin.app
   ```

2. **Create the app** the first time:

   ```
   osyrin app create "My Tasks"
   ```

   It prints the app's slug. On Osyrin Cloud the slug ends with your organization's slug.

   Use the name your `app.osy` declares: `osy deploy` finds the app by it. If you have an app of that name in more than one organization, add `--app <slug>` to `osy deploy` to choose.

3. **Deploy:**

   ```
   osy deploy --env production --server osyrin.app
   ```

   Your app is built on your machine into a bundle and uploaded. The environment rebuilds it, brings its database up to date and starts it. `osy deploy` follows each step and prints the reason if one fails.

4. **Open it:**

   ```
   osyrin app launch <slug>
   ```

## What happens on each deploy

- **Every deploy is a version.** Leave out `--version` and the platform names it. Add `--reason "…"` to record why you deployed.
- **The database follows your model.** The first deploy creates the environment's database. Each later deploy brings its tables up to the new version before it is served.
- **A failed deploy rolls back.** If the new version fails and an earlier one was running, the earlier one is put back and the deployment says so. Your team is emailed when a deploy fails.
- **Work in flight keeps running.** A workflow started on the previous version finishes on that version.

## Each environment has its own data and address

Your app is served at `https://<app>.osyrin.app`, where `<app>` is the slug `osyrin app create` printed. `https://<app>--<environment>.osyrin.app` reaches one environment directly. Rows written in one environment are never seen in another.

## Secrets and data

- `osyrin app secret set <NAME>` gives a declared secret its value in the cloud. `osyrin app secret list` shows which still have none.
- `osyrin app import` loads your data files into the deployed app, like `osy import` does locally.

## When something goes wrong

- `osyrin app versions` lists the versions and what is still running on each.
- `osyrin app logs` shows your app's log lines. Add `--errors` for failures only.

## Related

- [Use a custom domain](/help-centre/a/use-a-custom-domain)
- [Billing and plans on Osyrin Cloud](/help-centre/a/billing-and-plans)
- [Report a bug](/help-centre/a/found-a-bug)
