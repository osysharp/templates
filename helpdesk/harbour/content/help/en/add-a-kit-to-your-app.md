---
slug: add-a-kit-to-your-app
title: Add a kit to your app
summary: Kits add ready-made features to your app. Add one with a single use line and pin it with osy lock.
category: building-your-app
section: kits-and-privacy
audience: Everyone
position: 1
version: 1
---
A kit is a package of Osy# that adds a feature to your app: PDFs, file storage, workflows, the UI controls. You add it with one line in your manifest. Its entities, functions and pages become part of your app, and its rules use the policies your app declares.

## Add a kit

1. Open `app.osy` and add a `use` line inside the `app { }` block:

   ```
   app MyTasks {
     model "model/**/*.osy";
     use Osysharp.Ui;
     use Osysharp.PdfViewer@1;
   }
   ```

   `@1` is the major version you accept. Updates within that major are allowed. A new major is never taken without you changing the line.

2. Pin it:

   ```
   osy lock
   ```

   `osy lock` resolves each kit and writes `osyrin.lock` with the exact version and a content hash, for example:

   ```
   ✓ Wrote osyrin.lock (2 pins)
     Osysharp.PdfViewer  1.0.0  github:osysharp/pdf@v1.0.0
     Osysharp.Ui  2.15.2  prewarmed:osysharp/ui
   ```

3. Commit `osyrin.lock` with your source, so every build uses the same versions.

4. In a model file that uses the kit's types, add `using Osysharp.PdfViewer;` at the top.

## Which kits you can add

- **Kits the platform carries** need no download. `osy kits` lists them; today they include `Osysharp.Ui`, `Osysharp.Workflow`, `Osysharp.Storage`, `Osysharp.Scheduling`, `Osysharp.Http`, `Osysharp.Memory` and `Osysharp.Markdown`.
- **Published kits** are fetched by `osy lock`. `osy search` lists them, each with the `use` line to paste, for example `use Osysharp.PdfViewer@1;` or `use Osysharp.BarcodeScanner@1;`.

More kits are being published as packages. A kit that has no release yet cannot be fetched: `osy lock` says so by name and writes nothing, so your existing lock stays as it was.

## Learn a kit

```
osy kits Osysharp.PdfViewer
```

This prints what the kit is for, and any contract it declares for other kits to implement. A kit in your own workspace also shows its smallest example app and the names of its tests. For the UI kit, `osy kit` lists every control with its signature, and `osy kit Card` prints one control's source.

## Keep kits up to date

`osy update` re-pins every kit to the newest version within its declared major and rewrites `osyrin.lock`. Run your tests after it.

## When no kit covers it

`osy kits new <Name>` starts a kit of your own beside your app, for an API or a service you want to keep separate from the rest of your code.

## Related

- [Privacy requests: what your app gets from the Privacy kit](/help-centre/a/privacy-requests-and-the-privacy-kit)
- [Install Osy# and run your first app](/help-centre/a/install-osy-and-run-your-first-app)
- [Your first compile error: how to read it and fix it](/help-centre/a/read-and-fix-a-compile-error)
