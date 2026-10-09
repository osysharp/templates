---
slug: use-a-custom-domain
title: Använd en egen domän
summary: Visa din app på en adress du äger, till exempel app.example.com, med ett certifikat som utfärdas åt dig.
version: 1
---
Varje app på Osyrin Cloud har en adress under `osyrin.app`. Du kan också visa den på ett namn du äger, till exempel `app.example.com`.

## Innan du börjar

- Driftsätt appen i en miljö först. En egen domän visar exakt en miljö i en app.
- Du behöver vara ägare eller administratör för organisationen eller för appen.
- Egna domäner ingår i organisationens plan. Har organisationen inga kvar säger sidan hur många den har och vad gränsen är.

## Lägg till domänen

1. Öppna din app i Osyrins administration och välj fliken **Domains**.
2. Skriv in **Hostname**, till exempel `app.example.com`.
3. Välj vilken **Environment** den ska visa.
4. Välj **Add domain**.

Sidan visar sedan två DNS-poster som du ska publicera.

## Publicera de två posterna

Lägg till följande hos din DNS-leverantör:

- **TXT**-post på `_osyrin.app.example.com`, med värdet `osyrin-domain-verification=…` som visas på sidan.
- **CNAME**-post på `app.example.com`, som pekar på målet som visas på sidan, `<app>--<environment>.osyrin.app`.

TXT-posten visar att namnet är ditt. CNAME-posten skickar besökarna till din app. Kopiera båda värdena från sidan: verifieringsvärdet hör till just din domän och ändras aldrig.

## Verifiera

Osyrin kontrollerar posterna själv, löpande. Du kan också välja **Check now**. När båda posterna stämmer visar domänen **Verified** och plattformen begär ett TLS-certifikat för den. Certifikatet tar oftast från några minuter till några tiotals minuter. Tills domänen är verifierad visar den ingenting.

Är en post fel säger sidan exakt vad den hittade och vad den väntade sig, till exempel att ingen `_osyrin`-post hittades eller att namnet pekar någon annanstans.

## Om DNS ändras senare

Om en verifierad domäns poster slutar stämma fortsätter den att visas i 7 dagar, och organisationens administratörer får ett mejl med datumet då den slutar. Rätta posterna inom de 7 dagarna så märker dina besökare ingenting.

## Ta bort en domän

Välj **Remove** bredvid den. Den visas inte längre, och namnet kan läggas till igen.

## Bra att veta

- Ett värdnamn kan peka på en app på plattformen. Används det redan någon annanstans, ta bort det där först.
- Namn under `osyrin.app` kan inte läggas till som egna domäner.
- En underdomän som `app.example.com` är enklast, eftersom den kan ha en CNAME-post.

## Relaterat

- [Driftsätt din app på Osyrin Cloud](/sv/help-centre/a/deploy-your-app-to-osyrin-cloud)
- [Fakturering och planer på Osyrin Cloud](/sv/help-centre/a/billing-and-plans)
- [Rapportera en bugg](/sv/help-centre/a/found-a-bug)
