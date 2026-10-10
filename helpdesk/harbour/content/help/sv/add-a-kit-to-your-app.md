---
slug: add-a-kit-to-your-app
title: Lägg till ett kit i din app
summary: Kit lägger färdiga funktioner till din app. Lägg till ett med en enda use-rad och lås det med osy lock.
version: 1
---
Ett kit är ett paket med Osy# som lägger till en funktion i din app: PDF:er, fillagring, arbetsflöden, UI-kontrollerna. Du lägger till det med en rad i ditt manifest. Dess entiteter, funktioner och sidor blir en del av din app, och dess regler använder de policyer som din app deklarerar.

## Lägg till ett kit

1. Öppna `app.osy` och lägg till en `use`-rad i blocket `app { }`:

   ```
   app MyTasks {
     model "model/**/*.osy";
     use Osysharp.Ui;
     use Osysharp.PdfViewer@1;
   }
   ```

   `@1` är den huvudversion du accepterar. Uppdateringar inom den huvudversionen tillåts. En ny huvudversion tas aldrig in utan att du ändrar raden.

2. Lås den:

   ```
   osy lock
   ```

   `osy lock` slår upp varje kit och skriver `osyrin.lock` med exakt version och en innehållshash, till exempel:

   ```
   ✓ Wrote osyrin.lock (2 pins)
     Osysharp.PdfViewer  1.0.0  github:osysharp/pdf@v1.0.0
     Osysharp.Ui  2.15.2  prewarmed:osysharp/ui
   ```

3. Checka in `osyrin.lock` tillsammans med källkoden, så att varje bygge använder samma versioner.

4. Lägg till `using Osysharp.PdfViewer;` överst i en modellfil som använder kitets typer.

## Vilka kit du kan lägga till

- **Kit som plattformen har med sig** behöver ingen nedladdning. `osy kits` listar dem; i dag finns bland andra `Osysharp.Ui`, `Osysharp.Workflow`, `Osysharp.Storage`, `Osysharp.Scheduling`, `Osysharp.Http`, `Osysharp.Memory` och `Osysharp.Markdown`.
- **Publicerade kit** hämtas av `osy lock`. `osy search` listar dem, var och en med den `use`-rad du klistrar in, till exempel `use Osysharp.PdfViewer@1;` eller `use Osysharp.BarcodeScanner@1;`.

Fler kit publiceras som paket. Ett kit som ännu inte har släppts kan inte hämtas: `osy lock` säger det med namn och skriver ingenting, så att din befintliga låsfil förblir som den var.

## Lär dig ett kit

```
osy kits Osysharp.PdfViewer
```

Det skriver ut vad kitet är till för och de kontrakt det deklarerar som andra kit kan implementera. Ett kit i din egen arbetsyta visar också sin minsta exempelapp och namnen på sina tester. För UI-kitet listar `osy kit` varje kontroll med sin signatur, och `osy kit Card` skriver ut källkoden för en kontroll.

## Håll kiten uppdaterade

`osy update` låser om varje kit till den senaste versionen inom dess deklarerade huvudversion och skriver om `osyrin.lock`. Kör dina tester efteråt.

## När inget kit täcker behovet

`osy kits new <Name>` startar ett eget kit bredvid din app, för ett API eller en tjänst som du vill hålla isär från resten av koden.

## Relaterat

- [Integritetsförfrågningar: vad din app får från Privacy-kitet](/sv/help-centre/a/privacy-requests-and-the-privacy-kit)
- [Installera Osy# och kör din första app](/sv/help-centre/a/install-osy-and-run-your-first-app)
- [Ditt första kompileringsfel: så läser och rättar du det](/sv/help-centre/a/read-and-fix-a-compile-error)
