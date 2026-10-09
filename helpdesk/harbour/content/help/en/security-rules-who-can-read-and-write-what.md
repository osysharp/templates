---
slug: security-rules-who-can-read-and-write-what
title: "Security rules: who can read and write what"
summary: Declare access once on each entity. Every query and every write obeys it, with no code path around it.
category: building-your-app
section: sign-in-and-security
audience: Everyone
position: 2
version: 1
---
In Osy# you decide who can read and write data in one place: a `security { }` block on the entity. It is not a check you call. It is compiled into every query and every write, so no page, function or API call can get around it.

## Everything starts closed

An entity with no `security` block is denied to every user request. You never write "deny everything". You only write what is allowed.

## Two kinds of rule

- **`where`** filters by the **row**: "people see their own notes".
- **`when`** gates by **who is asking**: "admins see every note".

The verbs are `read`, `create`, `update` and `delete`. Several can share one rule.

## An example

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

Each person reads and changes their own notes. An admin reads every note, but not the private comment on someone else's.

A `policy` is a named test of the person asking. `user` is the person signed in.

## How the rules behave

- **A row you may not read is never fetched.** `Note.Count()` gives each person their own count. Nothing is hidden after the fact.
- **A field rule hides one column.** `deny read PrivateComment` returns the row without that field.
- **An update is checked twice:** against the row as it is and as it will be. Both must pass, so in the example above nobody can hand their note to someone else by changing its `Owner`.
- **Write the refusal people see.** A `message "…";` line at the top of a block is what a refused person reads.

## See what you declared

```
osy explain
```

`osy explain` prints your app's access in plain English, per entity and per page, including hidden fields. It needs no database and no running platform.

## Prove it in a test

A rule you have not tested is a rule you believe you wrote. In a test, `[runas(Person)]` acts as that person, so you can check that someone else's rows are not there:

```
[Test(Seed)]
[runas(Bob)]
void Bob_cannot_see_Alices_note() {
  Assert.Equal(0, Note.Count());
}
```

A test body with no `runas` runs as nobody signed in.

`osy docs security` covers every rule shape.

## Related

- [Add sign-in to your app](/help-centre/a/add-sign-in-to-your-app)
- [Privacy requests: what your app gets from the Privacy kit](/help-centre/a/privacy-requests-and-the-privacy-kit)
- [Your first compile error: how to read it and fix it](/help-centre/a/read-and-fix-a-compile-error)
