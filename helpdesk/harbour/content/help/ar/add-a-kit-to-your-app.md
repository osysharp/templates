---
slug: add-a-kit-to-your-app
title: أضف حزمة إلى تطبيقك
summary: تضيف الحزم إلى تطبيقك ميزات جاهزة. أضف واحدة بسطر use واحد وثبّت إصدارها بـ osy lock.
version: 1
---
الحزمة (kit) مجموعة من شيفرة Osy# تضيف ميزة إلى تطبيقك: ملفات PDF، وتخزين الملفات، وسير العمل، وعناصر الواجهة. تضيفها بسطر واحد في ملف التعريف. فتصبح كياناتها ودوالها وصفحاتها جزءًا من تطبيقك، وتستخدم قواعدها السياسات التي يصرّح بها تطبيقك.

## أضف حزمة

1. افتح `app.osy` وأضف سطر `use` داخل كتلة `app { }`:

   ```
   app MyTasks {
     model "model/**/*.osy";
     use Osysharp.Ui;
     use Osysharp.PdfViewer@1;
   }
   ```

   `@1` هو الإصدار الرئيسي الذي تقبله. التحديثات ضمن هذا الإصدار الرئيسي مسموحة. ولا يُعتمد إصدار رئيسي جديد أبدًا دون أن تغيّر السطر بنفسك.

2. ثبّت الإصدار:

   ```
   osy lock
   ```

   يحدد `osy lock` كل حزمة ويكتب `osyrin.lock` بالإصدار الدقيق وبصمة للمحتوى، مثل:

   ```
   ✓ Wrote osyrin.lock (2 pins)
     Osysharp.PdfViewer  1.0.0  github:osysharp/pdf@v1.0.0
     Osysharp.Ui  2.15.2  prewarmed:osysharp/ui
   ```

3. احفظ `osyrin.lock` في المستودع مع شيفرتك، حتى يستخدم كل بناء الإصدارات نفسها.

4. أضف `using Osysharp.PdfViewer;` في أعلى كل ملف نموذج يستخدم أنواع الحزمة.

## الحزم التي يمكنك إضافتها

- **الحزم التي تحملها المنصة** لا تحتاج إلى تنزيل. يعرضها `osy kits`؛ ومنها اليوم `Osysharp.Ui` و`Osysharp.Workflow` و`Osysharp.Storage` و`Osysharp.Scheduling` و`Osysharp.Http` و`Osysharp.Memory` و`Osysharp.Markdown`.
- **الحزم المنشورة** يجلبها `osy lock`. ويعرضها `osy search` مع سطر `use` الذي تلصقه، مثل `use Osysharp.PdfViewer@1;` أو `use Osysharp.BarcodeScanner@1;`.

تُنشر حزم أخرى تباعًا. والحزمة التي لم يصدر لها إصدار بعد لا يمكن جلبها: يذكر `osy lock` ذلك باسمها ولا يكتب شيئًا، فيبقى ملف التثبيت الحالي كما هو.

## تعرّف على حزمة

```
osy kits Osysharp.PdfViewer
```

يطبع هذا الأمر الغرض من الحزمة، وأي عقد تصرّح به لتنفذه حزم أخرى. والحزمة الموجودة في مساحة عملك تعرض أيضًا أصغر تطبيق مثال لها وأسماء اختباراتها. ولحزمة الواجهة، يعرض `osy kit` كل عنصر مع توقيعه، ويطبع `osy kit Card` شيفرة عنصر واحد.

## حدّث الحزم

يعيد `osy update` تثبيت كل حزمة على أحدث إصدار ضمن إصدارها الرئيسي المصرّح به، ويعيد كتابة `osyrin.lock`. شغّل اختباراتك بعده.

## عندما لا تغطي أي حزمة ما تحتاجه

يبدأ `osy kits new <Name>` حزمة خاصة بك بجانب تطبيقك، لواجهة API أو خدمة تريد إبقاءها منفصلة عن بقية شيفرتك.

## مقالات ذات صلة

- [طلبات الخصوصية: ما يحصل عليه تطبيقك من حزمة Privacy](/ar/help-centre/a/privacy-requests-and-the-privacy-kit)
- [ثبّت Osy# وشغّل تطبيقك الأول](/ar/help-centre/a/install-osy-and-run-your-first-app)
- [أول خطأ تصريف: كيف تقرؤه وتصلحه](/ar/help-centre/a/read-and-fix-a-compile-error)
