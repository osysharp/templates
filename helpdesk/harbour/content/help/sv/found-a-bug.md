---
slug: found-a-bug
title: Rapportera en bugg
summary: Så berättar du för oss att något är fel, och vad du ska ta med så att vi kan rätta det snabbt.
version: 1
---
Har du hittat något som inte fungerar? Berätta för oss. En tydlig beskrivning ger en snabbare rättning, och en bugg i verktygen kommer oftast med detaljer som du kan kopiera direkt från terminalen.

## Här berättar du

- **Sidan Få hjälp.** Välj **Fråga teamet** i det här hjälpcentret. Skriv en **Rubrik** på en rad och beskriv **Vad som hände**. Medan du skriver rubriken föreslår sidan artiklar som kanske redan besvarar frågan. Välj **Skicka förfrågan** så får du ett referensnummer. Vi svarar via mejl.
- **Chatten.** Välj **Chatta med oss** i hörnet på valfri sida i hjälpcentret. Den söker först bland de här artiklarna, och ditt första meddelande når en person.
- **Dina förfrågningar.** Logga in för att följa allt du har skickat till oss, svara och bifoga filer som skärmbilder.
- **En bugg i själva `osy`-verktygen.** Kör `osy feedback` i ditt projekt. Det öppnar ett förifyllt ärende i verktygens publika ärendelista, github.com/osysharp/cli, med din version och det senaste felet redan ifyllda. Du granskar det innan du skickar. `osy feedback --full` lägger till ett diagnospaket som laddas upp privat och aldrig bifogas det publika ärendet. Din källkod följer bara med om du lägger till `--with-source`.

## Det här ska du ta med

1. **Vad du gjorde, vad du väntade dig och vad som hände.** Klistra in den exakta feltexten, inte en sammanfattning av den.
2. **Din version.** Kör `osy --version` och klistra in raden den skriver ut.
3. **Korrelations-id:t.** En handling i din app skriver sina rader från webbläsaren och servern under ett och samma id. `osy logs --errors` visar de senaste felen med sina id:n. `osy logs --corr <id>` visar allt för ett av dem.
4. **Spåret, för ett fel inne i din app.** `osy inspect` listar de körningar den har sparat, med de misslyckade markerade. `osy inspect <trace-id> --fault` hoppar till steget som misslyckades.
5. **Den minsta källkod som visar felet.** Ligger problemet i din Osy#-kod är några rader som fortfarande misslyckas värda mer än hela appen.

## Det här ska du aldrig skicka

Klistra aldrig in ett lösenord, en API-nyckel eller innehållet i din `.secrets`-fil. `osy feedback --full` utelämnar dem själv och listar allt i paketet innan något lämnar din dator.

## För en driftsatt app

`osyrin app logs` läser loggraderna från din driftsatta app. Lägg till `--errors` för att bara se fel, och ta med korrelations-id:t från raden du menar.

## Relaterat

- [Ditt första kompileringsfel: så läser och rättar du det](/sv/help-centre/a/read-and-fix-a-compile-error)
- [Installera Osy# och kör din första app](/sv/help-centre/a/install-osy-and-run-your-first-app)
- [Driftsätt din app på Osyrin Cloud](/sv/help-centre/a/deploy-your-app-to-osyrin-cloud)
