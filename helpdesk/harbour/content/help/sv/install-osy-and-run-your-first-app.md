---
slug: install-osy-and-run-your-first-app
title: Installera Osy# och kör din första app
summary: Från en tom mapp till en app som körs i webbläsaren, utan konto och utan databas att sätta upp.
version: 1
---
Du kan bygga och köra en Osy#-app på din egen dator gratis. Du behöver inget konto, ingen Docker och ingen databasserver.

## Installera

På macOS eller Linux:

```
curl -fsSL https://raw.githubusercontent.com/osyrin-platform/cli/main/install.sh | sh
```

På Windows, i PowerShell:

```
irm https://raw.githubusercontent.com/osyrin-platform/cli/main/install.ps1 | iex
```

Installationsprogrammet kontrollerar nedladdningen mot dess publicerade SHA-256 och installerar i `~/.osy/bin`. Du får kompilatorn, körmiljön, en lokal PostgreSQL och två kommandon:

- `osy` bygger, kontrollerar och kör din app på din dator.
- `osyrin` driftsätter den och sköter den i produktion.

## Skapa ett projekt

1. Skapa en tom mapp och gå in i den:

   ```
   mkdir my-tasks && cd my-tasks
   ```

2. Skapa projektet:

   ```
   osy init --agent claude
   ```

   Det skriver en liten fungerande app (en sida, en funktion, ett tema och ett test) och en `CLAUDE.md` med instruktioner till din kodagent. Använd `--agent agent` för att få en `AGENTS.md` i stället.

Vill du börja från ett komplett exempel anger du dess namn, till exempel `osy init kanban --agent claude`. `osy docs sample` listar exemplen.

## Kör den

```
osy launch
```

`osy launch` startar den lokala plattformen om den inte redan kör, kompilerar din källkod och öppnar appen på `http://<your-app>.localhost:<port>/`. Ändra en fil och kör `osy launch` igen för att se ändringen.

## Kontrollera den

```
osy check
```

`osy check` validerar källkoden, kör produktionslinten och dina tester och ger sedan ett samlat besked. Den pekar ut det som är svagt och säger hur du rättar det.

## När du är klar

`osy stop` stoppar den lokala plattformen för projektet. Nästa `osy launch` startar den igen.

## Frågor som verktygen besvarar

- `osy docs <topic>` svarar på hur du skriver något, till exempel `osy docs "hash a password"`.
- `osy kit` listar de UI-kontroller du kan använda.
- `osy explain` visar vem som får läsa och skriva varje sorts data.

## Relaterat

- [Ditt första kompileringsfel: så läser och rättar du det](/sv/help-centre/a/read-and-fix-a-compile-error)
- [Lägg till ett kit i din app](/sv/help-centre/a/add-a-kit-to-your-app)
- [Driftsätt din app på Osyrin Cloud](/sv/help-centre/a/deploy-your-app-to-osyrin-cloud)
