---
slug: add-sign-in-to-your-app
title: أضف تسجيل الدخول إلى تطبيقك
summary: اسمح للناس بإنشاء حساب وتسجيل الدخول بعنوان بريد إلكتروني وكلمة مرور.
version: 1
---
يسجّل تطبيق Osy# دخول الناس بشيفرة تكتبها داخل التطبيق نفسه. تصرّح بمن هم مستخدموك، وتكتب دالتي `Login` و`Signup`، وتطلب من المنصة تشغيلهما. والمنصة هي التي تُجزّئ كلمات المرور وتُصدر تذكرة الجلسة.

## الأجزاء

1. **كيان مستخدم موسوم بـ `[Principal]`.** يحمل بيانات الدخول وتجزئة كلمة المرور. اجعل البريد الإلكتروني `[Unique]` حتى لا يشترك حسابان فيه أبدًا.
2. **دور لعملية تسجيل الدخول نفسها.** يعمل تسجيل الدخول بدور مخصص، هنا `Authenticator`. وهذا الدور وحده يستطيع قراءة تجزئة كلمة المرور.
3. **دوال `[AuthMethod]`.** تتحقق `Login` من كلمة المرور وتعيد تذكرة. وتنشئ `Signup` الحساب.
4. **`app.AuthBootstrap`.** يسمّي الدالتين والدور الذي تعملان به.

## مثال كامل

```
[Role] enum AppRole { Authenticator, Member }
entity RoleGrant { User Grantee; [Required] AppRole Level; }
policy IsAuthenticator => RoleGrant.Any(g => g.Grantee == user && g.Level == AppRole.Authenticator);

[Principal] entity User {
  [Unique, MaxLength(200)] string Email;
  [MaxLength(200)] string PasswordHash;
  security {
    allow read when IsAuthenticated;
    allow read, create when IsAuthenticator;
    deny read PasswordHash when !IsAuthenticator;
  }
}

[AuthMethod]
string Login(string email, string password) {
  var u = User.Where(x => x.Email == email).FirstOrDefault();
  if (u == null) { Security.VerifyPassword(password); return ""; }
  if (Security.VerifyPassword(password, u.PasswordHash)) { return Security.IssueJwt(u.Id, u.Email); }
  return "";
}

[AuthMethod]
string Signup(string email, string password) {
  var u = new User { Email = email, PasswordHash = Security.HashPassword(password) };
  return Security.IssueJwt(u.Id, u.Email);
}

app.AuthBootstrap = new AuthBootstrap { Login = Login, Signup = Signup, Role = AppRole.Authenticator };
```

تستدعيهما الصفحة بـ `Session.SignIn(Login(email, password))`.

## ثلاثة أمور مهمة

- **أبقِ شرط `when` في قاعدة التجزئة.** تُخفي `deny read PasswordHash when !IsAuthenticator` التجزئة عن الجميع ما عدا تسجيل الدخول. ومن دون الشرط لا تستطيع دالة `Login` نفسها قراءة التجزئة، فترفض كل كلمة مرور صحيحة.
- **استخدم `[AuthMethod]` وليس `[AllowAnonymous]`.** تسمح `[AllowAnonymous]` فقط لزائر غير مسجّل باستدعاء دالة، لكنها لا تشغّلها بدور تسجيل الدخول. (وهي الصحيحة على صفحة تسجيل الدخول نفسها.)
- **استغرق الوقت نفسه عندما لا يوجد الحساب.** الصيغة ذات الوسيط الواحد `Security.VerifyPassword(password)` تجري الفحص كاملًا مقابل لا شيء، فلا يستطيع أحد أن يعرف من التوقيت أي العناوين لها حسابات.

## اختبر الحالتين

اختبر `Login` بكلمة مرور صحيحة تعيد تذكرة، وبكلمة خاطئة تعيد نصًا فارغًا. واختبر أيضًا عنوانًا لا حساب له: يجب أن يفشل تمامًا كما تفشل كلمة المرور الخاطئة. ويطلب `osy lint` هذه الاختبارات إلى أن تكتبها.

## حسابات للاختبار

يضيف `osy user add <email> --role <role> --password <password>` حسابًا إلى تطبيقك على المنصة المحلية، عبر حقول تطبيقك وقواعده.

## لمعرفة المزيد

يعرض `osy docs auth-bootstrap` العملية كاملة، مع إعادة تعيين كلمة المرور وصفحة تسجيل الدخول. ويشرح `osy docs password-auth` الربط العام الذي تستخدمه الأدوات.

## مقالات ذات صلة

- [قواعد الأمان: من يقرأ ماذا ومن يكتب ماذا](/ar/help-centre/a/security-rules-who-can-read-and-write-what)
- [أول خطأ تصريف: كيف تقرؤه وتصلحه](/ar/help-centre/a/read-and-fix-a-compile-error)
- [طلبات الخصوصية: ما يحصل عليه تطبيقك من حزمة Privacy](/ar/help-centre/a/privacy-requests-and-the-privacy-kit)
