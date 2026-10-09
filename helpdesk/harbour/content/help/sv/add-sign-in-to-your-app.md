---
slug: add-sign-in-to-your-app
title: Lägg till inloggning i din app
summary: Låt människor skapa ett konto och logga in med e-postadress och lösenord.
version: 1
---
En Osy#-app loggar in människor med kod som du skriver i själva appen. Du deklarerar vilka dina användare är, skriver en `Login`- och en `Signup`-funktion och talar om för plattformen att köra dem. Plattformen hashar lösenorden och utfärdar sessionsbiljetten.

## Delarna

1. **En användarentitet märkt `[Principal]`.** Den håller inloggningen och lösenordshashen. Gör e-postadressen `[Unique]`, så att två konton aldrig kan dela på den.
2. **En roll för själva inloggningen.** Inloggningen körs som en egen roll, här `Authenticator`. Bara den rollen får läsa lösenordshashen.
3. **`[AuthMethod]`-funktioner.** `Login` kontrollerar lösenordet och svarar med en biljett. `Signup` skapar kontot.
4. **`app.AuthBootstrap`.** Den pekar ut de två funktionerna och rollen de körs som.

## Ett komplett exempel

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

En sida anropar dem med `Session.SignIn(Login(email, password))`.

## Tre saker som spelar roll

- **Behåll `when` i regeln för hashen.** `deny read PasswordHash when !IsAuthenticator` döljer hashen för alla utom inloggningen. Utan villkoret kan din egen `Login` inte läsa hashen och nekar varje korrekt lösenord.
- **Använd `[AuthMethod]`, inte `[AllowAnonymous]`.** `[AllowAnonymous]` låter bara en utloggad besökare anropa en funktion. Den körs då inte som inloggningsrollen. (På själva inloggningssidan är `[AllowAnonymous]` rätt.)
- **Lägg lika lång tid när kontot inte finns.** Varianten med ett argument, `Security.VerifyPassword(password)`, gör hela kontrollen mot ingenting, så ingen kan avgöra från tiden vilka adresser som har konton.

## Testa åt båda hållen

Testa `Login` med rätt lösenord, som ger en biljett, och med fel lösenord, som ger en tom sträng. Testa också en adress utan konto: den ska misslyckas precis som ett fel lösenord. `osy lint` ber om de här testerna tills de finns.

## Konton att testa med

`osy user add <email> --role <role> --password <password>` lägger till ett konto i din app på den lokala plattformen. Det går genom appens egna fält och regler.

## Läs mer

`osy docs auth-bootstrap` visar hela flödet, med lösenordsåterställning och en inloggningssida. `osy docs password-auth` beskriver den generiska bindning som verktygen använder.

## Relaterat

- [Säkerhetsregler: vem får läsa och skriva vad](/sv/help-centre/a/security-rules-who-can-read-and-write-what)
- [Ditt första kompileringsfel: så läser och rättar du det](/sv/help-centre/a/read-and-fix-a-compile-error)
- [Integritetsförfrågningar: vad din app får från Privacy-kitet](/sv/help-centre/a/privacy-requests-and-the-privacy-kit)
