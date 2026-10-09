---
slug: security-rules-who-can-read-and-write-what
title: "Säkerhetsregler: vem får läsa och skriva vad"
summary: Deklarera åtkomst en gång per entitet. Varje fråga och varje skrivning följer den, och ingen kodväg går runt den.
version: 1
---
I Osy# bestämmer du på ett enda ställe vem som får läsa och skriva data: ett `security { }`-block på entiteten. Det är ingen kontroll som du anropar. Det kompileras in i varje fråga och varje skrivning, så ingen sida, funktion eller API-anrop kan ta sig runt det.

## Allt börjar stängt

En entitet utan `security`-block nekas för varje användarförfrågan. Du skriver aldrig "neka allt". Du skriver bara vad som är tillåtet.

## Två sorters regler

- **`where`** filtrerar på **raden**: "var och en ser sina egna anteckningar".
- **`when`** spärrar på **vem som frågar**: "administratörer ser alla anteckningar".

Verben är `read`, `create`, `update` och `delete`. Flera kan dela på en regel.

## Ett exempel

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

Var och en läser och ändrar sina egna anteckningar. En administratör läser alla anteckningar, men inte den privata kommentaren på någon annans.

En `policy` är ett namngivet test av den som frågar. `user` är den som är inloggad.

## Så beter sig reglerna

- **En rad du inte får läsa hämtas aldrig.** `Note.Count()` ger var och en sitt eget antal. Ingenting döljs i efterhand.
- **En fältregel döljer en kolumn.** `deny read PrivateComment` returnerar raden utan just det fältet.
- **En uppdatering kontrolleras två gånger:** mot raden som den är och som den blir. Båda måste godkännas, så i exemplet ovan kan ingen lämna över sin anteckning till någon annan genom att ändra `Owner`.
- **Skriv det meddelande som nekade personer ser.** En rad `message "…";` överst i ett block är det en nekad person får läsa.

## Se vad du har deklarerat

```
osy explain
```

`osy explain` skriver ut appens åtkomst på vanlig engelska, per entitet och per sida, inklusive dolda fält. Det behöver ingen databas och ingen plattform som kör.

## Bevisa det i ett test

En regel du inte har testat är en regel du tror att du har skrivit. I ett test agerar `[runas(Person)]` som den personen, så du kan kontrollera att någon annans rader inte finns där:

```
[Test(Seed)]
[runas(Bob)]
void Bob_cannot_see_Alices_note() {
  Assert.Equal(0, Note.Count());
}
```

Ett testblock utan `runas` körs som någon som inte är inloggad.

`osy docs security` går igenom alla sorters regler.

## Relaterat

- [Lägg till inloggning i din app](/sv/help-centre/a/add-sign-in-to-your-app)
- [Integritetsförfrågningar: vad din app får från Privacy-kitet](/sv/help-centre/a/privacy-requests-and-the-privacy-kit)
- [Ditt första kompileringsfel: så läser och rättar du det](/sv/help-centre/a/read-and-fix-a-compile-error)
