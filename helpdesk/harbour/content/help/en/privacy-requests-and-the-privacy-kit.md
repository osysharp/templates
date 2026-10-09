---
slug: privacy-requests-and-the-privacy-kit
title: "Privacy requests: what your app gets from the Privacy kit"
summary: Let people see, take or erase their data, with each request planned, approved and answered on time.
category: building-your-app
section: kits-and-privacy
audience: Everyone
position: 2
version: 1
---
The Privacy kit, `Osysharp.Privacy`, handles the requests people make about their personal data: the screens, the deadlines and the answer, built on the personal data your app declares.

## The four requests

- **Access:** a copy of what you hold about them, with each category's purpose, basis and how long it is kept.
- **Portability:** the data they gave you, in a form another service can read.
- **Erasure:** remove what you may not keep, and keep only what you must or may, restricted.
- **Restriction:** keep their data but stop using it, until they ask to lift it.

## How people ask

- **A signed-in person** asks on `/privacy`. They confirm it is them with a one-time code sent to the address on their account. This comes with the companion kit `Osysharp.Privacy.Accounts`.
- **A person with no account** asks on `/privacy/ask` and proves their address with a code mailed there. This comes with `Osysharp.Privacy.Conversations`.
- **Your team** can file a request for someone, and records how they checked who it is.

## What happens next

1. **The request is planned, category by category.** For erasure: what can go now is erased, and what the law requires you to keep is kept restricted until its period ends.
2. **Someone approves the plan.** By default every plan waits for a person who manages privacy, on your team's inbox.
3. **The deadline is one calendar month** from receipt. It can be extended once, by one or two months, with a reason the person is told.
4. **The person gets an answer** saying what was erased, what is kept, on what basis and until when, and where they can complain.

A copy is two files, `your-data.json` and a readable `your-data.html`. It can be downloaded once, within seven days.

## What erasure does to your rows

By default the personal fields are cleared and the row is kept. You can choose to delete the row instead. Rows kept on a legal basis are hidden from every ordinary read until their period ends. A legal hold on a person makes an erasure wait.

## What you declare

1. Mark personal fields with their kind, for example `[Classification(Personal.Contact)]`.
2. Say whose each row is and why you keep it: `[PersonalData(SupportNotes)]` on the entity, where `SupportNotes` is a category you declare with its purpose and legal basis, and `[DataSubject]` on the field that names the person. The platform does the rest.
3. Declare who manages privacy and who places legal holds. A request is work on your team's inbox, so the inbox's own policies are declared too:

```
policy ManagesPrivacy => IsAdmin;
policy PlacesLegalHolds => IsAdmin;
policy WorksInbox => IsAdmin;
policy LeadsInbox => IsAdmin;
policy AdministersInbox => IsAdmin;
policy ManagesOrganizations => IsAdmin;
policy CollaboratesOnRecords => IsAdmin;
```

`osy docs privacy-kit` has the full setup. `osy docs personal-data` covers the classifications.

## Getting the kit

The Privacy kit is not yet published as a package, so `osy lock` cannot fetch it today and says so. `osy search` lists it, with its `use` line, once it is released. Marking your personal fields with `[Classification(Personal.…)]` works today and is what the kit builds on.

## Related

- [Security rules: who can read and write what](/help-centre/a/security-rules-who-can-read-and-write-what)
- [Add a kit to your app](/help-centre/a/add-a-kit-to-your-app)
- [Add sign-in to your app](/help-centre/a/add-sign-in-to-your-app)
