---
slug: security-rules-who-can-read-and-write-what
title: "قواعد الأمان: من يقرأ ماذا ومن يكتب ماذا"
summary: صرّح بالصلاحيات مرة واحدة على كل كيان. كل استعلام وكل عملية كتابة تلتزم بها، ولا يوجد مسار في الشيفرة يتجاوزها.
version: 1
---
في Osy# تحدد في مكان واحد من يمكنه قراءة البيانات وكتابتها: كتلة `security { }` على الكيان. هذه ليست فحصًا تستدعيه بنفسك، بل تُصرَّف داخل كل استعلام وكل عملية كتابة، فلا تستطيع أي صفحة أو دالة أو استدعاء API تجاوزها.

## كل شيء يبدأ مغلقًا

الكيان الذي لا يحتوي على كتلة `security` مرفوض لكل طلب من المستخدمين. لا تكتب أبدًا "ارفض كل شيء"، بل تكتب فقط ما هو مسموح.

## نوعان من القواعد

- **`where`** تُرشّح حسب **الصف**: "كل شخص يرى ملاحظاته".
- **`when`** تسمح حسب **من يسأل**: "المسؤولون يرون كل الملاحظات".

الأفعال هي `read` و`create` و`update` و`delete`. ويمكن أن تشترك عدة أفعال في قاعدة واحدة.

## مثال

```
[Role] enum AppRole { Authenticator, Member, Admin }
entity RoleGrant { User Grantee; [Required] AppRole Level; }
policy IsAdmin => RoleGrant.Any(g => g.Grantee == user && g.Level == AppRole.Admin);

entity Note {
  [Required] User Owner;
  [MaxLength(200)] string Title;
  [MaxLength(2000)] string? PrivateComment;
  security {
    allow read, create, update, delete where Owner == user;
    allow read when IsAdmin;
    deny read PrivateComment when IsAdmin;
  }
}
```

يقرأ كل شخص ملاحظاته ويعدّلها. ويقرأ المسؤول كل الملاحظات، لكن ليس التعليق الخاص على ملاحظات غيره.

`policy` اختبار مُسمّى لمن يسأل. و`user` هو الشخص الذي سجّل الدخول.

## كيف تعمل القواعد

- **الصف الذي لا يحق لك قراءته لا يُجلب أبدًا.** تعطي `Note.Count()` كل شخص عدد ملاحظاته هو. لا يُخفى شيء بعد جلبه.
- **قاعدة الحقل تخفي عمودًا واحدًا.** تعيد `deny read PrivateComment` الصف دون ذلك الحقل.
- **يُفحص التعديل مرتين:** على الصف كما هو وكما سيصبح. ويجب أن ينجح الفحصان، لذلك لا يستطيع أحد في المثال أعلاه أن يعطي ملاحظته لشخص آخر بتغيير `Owner`.
- **اكتب الرسالة التي يراها من يُرفض طلبه.** سطر `message "…";` في أعلى الكتلة هو ما يقرؤه الشخص المرفوض.

## اطّلع على ما صرّحت به

```
osy explain
```

يطبع `osy explain` صلاحيات تطبيقك بلغة إنجليزية واضحة، لكل كيان ولكل صفحة، بما في ذلك الحقول المخفية. لا يحتاج إلى قاعدة بيانات ولا إلى منصة قيد التشغيل.

## أثبت ذلك في اختبار

القاعدة التي لم تختبرها هي قاعدة تظن أنك كتبتها. في الاختبار يعمل `[runas(Person)]` بصفة ذلك الشخص، فتستطيع التحقق من أن صفوف غيره غير موجودة:

```
[Test(Seed)]
[runas(Bob)]
void Bob_cannot_see_Alices_note() {
  Assert.Equal(0, Note.Count());
}
```

جسم الاختبار الذي لا يحتوي على `runas` يعمل بصفة شخص لم يسجّل الدخول.

يشرح `osy docs security` كل أشكال القواعد.

## مقالات ذات صلة

- [أضف تسجيل الدخول إلى تطبيقك](/ar/help-centre/a/add-sign-in-to-your-app)
- [طلبات الخصوصية: ما يحصل عليه تطبيقك من حزمة Privacy](/ar/help-centre/a/privacy-requests-and-the-privacy-kit)
- [أول خطأ تصريف: كيف تقرؤه وتصلحه](/ar/help-centre/a/read-and-fix-a-compile-error)
