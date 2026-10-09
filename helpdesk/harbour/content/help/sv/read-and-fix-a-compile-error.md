---
slug: read-and-fix-a-compile-error
title: "Ditt första kompileringsfel: så läser och rättar du det"
summary: Varje fel anger filen, raden, vad som är fel och oftast hur du rättar det. Så här läser du ett.
version: 1
---
Osy# kontrollerar hela appen innan den körs, så de flesta misstag syns som ett kompileringsfel på din egen dator, inom några sekunder, i stället för som en trasig sida senare.

## Kontrollera källkoden

```
osy validate
```

`osy validate` tolkar källkoden, slår upp varje namn och kör samma kontroller som en kompilering. Den behöver ingen server, ingen databas och inget konto. `osy check` kör samma validering, sedan linten och dina tester, och ger ett samlat besked.

## Läs felet

Här är ett verkligt fel. Koden frågar efter `n.Titel`, men fältet heter `Title`:

```
model/count.osy:2:28  ERROR  RESOLVE_ERROR  entity 'Note' has no property 'Titel'. Did you mean 'Title'?
  → Title
  → for more, RUN: osy docs query-index …

✗ 1 error
```

Läs det från vänster till höger:

1. **Var:** `model/count.osy:2:28` är filen, raden och kolumnen. De flesta editorer öppnar stället när du klickar på det.
2. **Hur allvarligt:** `ERROR` stoppar bygget. En varning gör det inte.
3. **Vilken kontroll:** `RESOLVE_ERROR` betyder att ett namn inte matchade något. Ett tolkningsfel betyder att själva texten inte är giltig Osy#.
4. **Vad som är fel,** i en mening, ofta med ett förslag: `Did you mean 'Title'?`
5. **Rättningen,** efter pilen. Här är det rätt namn.
6. **Var du läser mer:** det `osy docs`-ämne som förklarar funktionen.

## Rätta det

1. Rätta det **första** felet först. Ett misstag kan orsaka flera fel längre ner.
2. Kör `osy validate` igen.
3. Upprepa tills det står `✓ Validated successfully`.

Nya fel kan dyka upp när du har rättat de första. Det är normalt: vissa kontroller körs först när källkoden går att slå upp utan fel, och utskriften säger det.

## När meddelandet inte räcker

- **Slå upp funktionen:** `osy docs <topic>`, till exempel `osy docs security` eller `osy docs "hash a password"`.
- **Kontrollera en UI-kontrolls exakta signatur:** `osy kit <Control>`, till exempel `osy kit Card`.
- **Se vad kompilatorn förstod:** `osy model` visar dina entiteter, relationer och funktioner så som den har slagit upp dem.

## Låt din kodagent göra det

Bygger du med en agent läser den samma utskrift. Ett projekt som skapats med `osy init` säger redan åt agenten att köra `osy validate` och `osy check`, så den kan hitta och rätta de flesta fel själv.

Tror du att kompilatorn har fel, kör `osy feedback`. Det öppnar ett förifyllt ärende med din version och det senaste felet.

## Relaterat

- [Installera Osy# och kör din första app](/sv/help-centre/a/install-osy-and-run-your-first-app)
- [Säkerhetsregler: vem får läsa och skriva vad](/sv/help-centre/a/security-rules-who-can-read-and-write-what)
- [Rapportera en bugg](/sv/help-centre/a/found-a-bug)
