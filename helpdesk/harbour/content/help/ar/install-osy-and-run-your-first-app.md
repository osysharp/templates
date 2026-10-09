---
slug: install-osy-and-run-your-first-app
title: ثبّت Osy# وشغّل تطبيقك الأول
summary: من مجلد فارغ إلى تطبيق يعمل في متصفحك، دون حساب ودون قاعدة بيانات تحتاج إلى إعداد.
version: 1
---
يمكنك بناء تطبيق Osy# وتشغيله على جهازك مجانًا. لا تحتاج إلى حساب ولا إلى Docker ولا إلى خادم قاعدة بيانات.

## التثبيت

على macOS أو Linux:

```
curl -fsSL https://raw.githubusercontent.com/osyrin-platform/cli/main/install.sh | sh
```

على Windows، في PowerShell:

```
irm https://raw.githubusercontent.com/osyrin-platform/cli/main/install.ps1 | iex
```

يتحقق برنامج التثبيت من الملف المُنزَّل مقابل بصمة SHA-256 المنشورة، ثم يثبّته في `~/.osy/bin`. تحصل على المُصرّف وبيئة التشغيل وقاعدة PostgreSQL محلية وأمرين:

- `osy` يبني تطبيقك ويفحصه ويشغّله على جهازك.
- `osyrin` ينشره ويديره في بيئة الإنتاج.

## أنشئ مشروعًا

1. أنشئ مجلدًا فارغًا وادخل إليه:

   ```
   mkdir my-tasks && cd my-tasks
   ```

2. أنشئ المشروع:

   ```
   osy init --agent claude
   ```

   يكتب هذا الأمر تطبيقًا صغيرًا يعمل (صفحة ودالة وسمة واختبار) وملف `CLAUDE.md` يرشد وكيل البرمجة الذي تستخدمه. استخدم `--agent agent` لتحصل على `AGENTS.md` بدلًا منه.

للبدء من نموذج كامل، اكتب اسمه، مثل `osy init kanban --agent claude`. ويعرض `osy docs sample` قائمة النماذج.

## شغّله

```
osy launch
```

يشغّل `osy launch` المنصة المحلية إن لم تكن تعمل، ويُصرّف شيفرتك، ويفتح التطبيق على `http://<your-app>.localhost:<port>/`. عدّل ملفًا وشغّل `osy launch` مرة أخرى لترى التغيير.

## افحصه

```
osy check
```

يتحقق `osy check` من الشيفرة، ويشغّل فحص جودة الإنتاج واختباراتك، ثم يعطي نتيجة واحدة. ويسمّي كل ما هو ضعيف ويقول كيف تصلحه.

## عند الانتهاء

يوقف `osy stop` المنصة المحلية لهذا المشروع. والأمر `osy launch` التالي يشغّلها من جديد.

## أسئلة تجيب عنها الأدوات

- يجيب `osy docs <topic>` عن طريقة كتابة شيء ما، مثل `osy docs "hash a password"`.
- يعرض `osy kit` عناصر الواجهة التي يمكنك استخدامها.
- يوضح `osy explain` من يستطيع قراءة كل نوع من البيانات وكتابته.

## مقالات ذات صلة

- [أول خطأ تصريف: كيف تقرؤه وتصلحه](/ar/help-centre/a/read-and-fix-a-compile-error)
- [أضف حزمة إلى تطبيقك](/ar/help-centre/a/add-a-kit-to-your-app)
- [انشر تطبيقك على Osyrin Cloud](/ar/help-centre/a/deploy-your-app-to-osyrin-cloud)
