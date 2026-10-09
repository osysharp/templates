---
slug: add-sign-in-to-your-app
title: Add sign-in to your app
summary: Let people create an account and sign in with an email address and a password.
category: building-your-app
section: sign-in-and-security
audience: Everyone
position: 1
version: 1
---
An Osy# app signs people in with code you write in the app itself. You declare who your users are, write a `Login` and a `Signup` function, and tell the platform to run them. The platform hashes passwords and issues the session ticket.

## The pieces

1. **A user entity marked `[Principal]`.** It holds the login and the password hash. Make the email `[Unique]`, so two accounts can never share it.
2. **A role for the sign-in itself.** Sign-in runs as a dedicated role, here `Authenticator`. Only that role may read the password hash.
3. **`[AuthMethod]` functions.** `Login` checks the password and answers a ticket. `Signup` creates the account.
4. **`app.AuthBootstrap`.** It names the two functions and the role they run as.

## A complete example

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

A page calls them with `Session.SignIn(Login(email, password))`.

## Three things that matter

- **Keep the `when` on the hash rule.** `deny read PasswordHash when !IsAuthenticator` hides the hash from everyone except the sign-in. Without the condition, your own `Login` cannot read the hash and refuses every correct password.
- **Use `[AuthMethod]`, not `[AllowAnonymous]`.** `[AllowAnonymous]` only lets a signed-out visitor call a function. It does not run it as the sign-in role. (It is right on the login page itself.)
- **Spend the same time when the account does not exist.** The one-argument `Security.VerifyPassword(password)` does the full check against nothing, so nobody can tell from the timing which addresses have accounts.

## Test both ways

Test `Login` with a right password, which answers a ticket, and a wrong one, which answers an empty string. Test an address with no account too: it must fail exactly like a wrong password. `osy lint` asks for these tests until they exist.

## Accounts for testing

`osy user add <email> --role <role> --password <password>` adds an account to your app on the local platform. It goes through your app's own fields and rules.

## Learn more

`osy docs auth-bootstrap` shows the full flow, with a password reset and a login page. `osy docs password-auth` covers the generic binding the toolchain uses.

## Related

- [Security rules: who can read and write what](/help-centre/a/security-rules-who-can-read-and-write-what)
- [Your first compile error: how to read it and fix it](/help-centre/a/read-and-fix-a-compile-error)
- [Privacy requests: what your app gets from the Privacy kit](/help-centre/a/privacy-requests-and-the-privacy-kit)
