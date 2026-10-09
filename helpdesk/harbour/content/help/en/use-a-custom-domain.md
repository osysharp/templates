---
slug: use-a-custom-domain
title: Use a custom domain
summary: Serve your app at an address you own, such as app.example.com, with a certificate issued for you.
category: deploying
section: domains
audience: Everyone
position: 1
version: 1
---
Every app on Osyrin Cloud has an address under `osyrin.app`. You can also serve it at a name you own, such as `app.example.com`.

## Before you start

- Deploy the app to an environment first. A custom domain serves exactly one environment of one app.
- You need to be an owner or admin of the organization, or of the app.
- Custom domains are part of your organization's plan. If your organization has none left, the page says how many it holds and what its limit is.

## Add the domain

1. In the Osyrin admin, open your app and choose the **Domains** tab.
2. Type the **Hostname**, for example `app.example.com`.
3. Choose the **Environment** it should serve.
4. Choose **Add domain**.

The page then shows two DNS records for you to publish.

## Publish the two records

At your DNS provider, add:

- **TXT** record at `_osyrin.app.example.com`, with the value `osyrin-domain-verification=…` shown on the page.
- **CNAME** record at `app.example.com`, pointing at the target shown on the page, `<app>--<environment>.osyrin.app`.

The TXT record proves the name is yours. The CNAME sends visitors to your app. Copy both values from the page: the verification value is your domain's own and never changes.

## Verify

Osyrin keeps checking the records on its own. You can also choose **Check now**. When both records are right, the domain shows **Verified** and the platform requests a TLS certificate for it. The certificate usually takes from a few minutes to tens of minutes. Until the domain is verified, it serves nothing.

If a record is wrong, the page says exactly what it found and what it expected, for example that no `_osyrin` record was found, or that the name points somewhere else.

## If DNS changes later

If a verified domain's records stop being right, it keeps serving for 7 days and your organization's admins get one email with the date it will stop. Fix the records within those 7 days and nothing changes for your visitors.

## Remove a domain

Choose **Remove** next to it. It stops being served, and the name is free to be added again.

## Good to know

- A hostname can point at one app on the platform. If it is already in use elsewhere, remove it there first.
- Names under `osyrin.app` cannot be added as custom domains.
- A subdomain such as `app.example.com` is the simplest choice, because it can carry a CNAME.

## Related

- [Deploy your app to Osyrin Cloud](/help-centre/a/deploy-your-app-to-osyrin-cloud)
- [Billing and plans on Osyrin Cloud](/help-centre/a/billing-and-plans)
- [Report a bug](/help-centre/a/found-a-bug)
