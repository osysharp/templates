---
slug: deploy-your-app-to-osyrin-cloud
title: Driftsätt din app på Osyrin Cloud
summary: Logga in, skapa appen en gång och driftsätt den i organisationens produktionsmiljö.
version: 2
---
Appen du kör på din dator är samma app som du driftsätter. Det finns ingen container, databas eller pipeline att sätta upp: Osyrin Cloud står för dem.

## Innan du börjar

Osyrin Cloud är i beta. Ansök från sidan Plans på osyrin.com. Du loggar in med ett GitHub-konto som är minst 30 dagar gammalt. När vi godkänner ansökan får du en organisation med en miljö som heter `production`.

## Driftsätt

1. **Logga in** från projektmappen:

   ```
   osyrin login --server osyrin.app
   ```

2. **Skapa appen** första gången:

   ```
   osyrin app create "My Tasks"
   ```

   Kommandot skriver ut appens slug. På Osyrin Cloud slutar den med organisationens slug.

   Använd namnet som din `app.osy` anger: `osy deploy` hittar appen med det. Har du en app med det namnet i flera organisationer, lägg till `--app <slug>` på `osy deploy` för att välja.

3. **Driftsätt:**

   ```
   osy deploy --env production --server osyrin.app
   ```

   Appen byggs på din dator till ett paket som laddas upp. Miljön bygger om det, uppdaterar sin databas och startar det. `osy deploy` följer varje steg och skriver ut orsaken om något misslyckas.

4. **Öppna den:**

   ```
   osyrin app launch <slug>
   ```

## Det här händer vid varje driftsättning

- **Varje driftsättning är en version.** Utelämnar du `--version` namnger plattformen den. Lägg till `--reason "…"` för att notera varför du driftsatte.
- **Databasen följer din modell.** Första driftsättningen skapar miljöns databas. Varje senare driftsättning uppdaterar tabellerna till den nya versionen innan den visas.
- **En misslyckad driftsättning rullas tillbaka.** Om den nya versionen misslyckas och en tidigare version körde, sätts den tidigare tillbaka och driftsättningen säger det. Ditt team får ett mejl när en driftsättning misslyckas.
- **Pågående arbete fortsätter.** Ett arbetsflöde som startade på den förra versionen avslutas på den versionen.

## Varje miljö har egen data och egen adress

Appen visas på `https://<app>.osyrin.app`, där `<app>` är den slug som `osyrin app create` skrev ut. `https://<app>--<environment>.osyrin.app` når en viss miljö direkt. Rader som skrivs i en miljö syns aldrig i en annan.

## Hemligheter och data

- `osyrin app secret set <NAME>` ger en deklarerad hemlighet sitt värde i molnet. `osyrin app secret list` visar vilka som fortfarande saknar värde.
- `osyrin app import` laddar dina datafiler i den driftsatta appen, på samma sätt som `osy import` gör lokalt.

## När något går fel

- `osyrin app versions` listar versionerna och vad som fortfarande körs på var och en.
- `osyrin app logs` visar appens loggrader. Lägg till `--errors` för att bara se fel.

## Relaterat

- [Använd en egen domän](/sv/help-centre/a/use-a-custom-domain)
- [Fakturering och planer på Osyrin Cloud](/sv/help-centre/a/billing-and-plans)
- [Rapportera en bugg](/sv/help-centre/a/found-a-bug)
