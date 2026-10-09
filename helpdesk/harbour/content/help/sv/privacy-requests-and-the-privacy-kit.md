---
slug: privacy-requests-and-the-privacy-kit
title: "Integritetsförfrågningar: vad din app får från Privacy-kitet"
summary: Låt människor se, ta med sig eller radera sina uppgifter, med varje förfrågan planerad, godkänd och besvarad i tid.
version: 1
---
Privacy-kitet, `Osysharp.Privacy`, hanterar de förfrågningar som människor gör om sina personuppgifter: skärmarna, tidsfristerna och svaret, byggt på de personuppgifter som din app deklarerar.

## De fyra förfrågningarna

- **Tillgång:** en kopia av det du har om dem, med varje kategoris syfte, rättslig grund och hur länge den sparas.
- **Dataportabilitet:** de uppgifter de har lämnat, i en form som en annan tjänst kan läsa.
- **Radering:** ta bort det du inte får spara, och spara bara det du måste eller får, begränsat.
- **Begränsning:** behåll uppgifterna men sluta använda dem, tills de ber om att begränsningen hävs.

## Så frågar man

- **En inloggad person** frågar på `/privacy`. De bekräftar att det är de med en engångskod som skickas till adressen på kontot. Det kommer med följekitet `Osysharp.Privacy.Accounts`.
- **En person utan konto** frågar på `/privacy/ask` och bevisar sin adress med en kod som mejlas dit. Det kommer med `Osysharp.Privacy.Conversations`.
- **Ditt team** kan registrera en förfrågan åt någon och anteckna hur de kontrollerade vem det är.

## Sedan händer det här

1. **Förfrågan planeras, kategori för kategori.** Vid radering raderas det som kan tas bort nu, och det som lagen kräver att du sparar behålls begränsat tills perioden är slut.
2. **Någon godkänner planen.** Som standard väntar varje plan på en person som hanterar integritet, i teamets inkorg.
3. **Tidsfristen är en kalendermånad** från mottagandet. Den kan förlängas en gång, med en eller två månader, med ett skäl som personen får veta.
4. **Personen får ett svar** om vad som raderades, vad som sparas, på vilken grund och till när, och var de kan klaga.

En kopia består av två filer, `your-data.json` och en läsbar `your-data.html`. Den kan laddas ner en gång, inom sju dagar.

## Vad radering gör med dina rader

Som standard töms personfälten och raden behålls. Du kan välja att radera raden i stället. Rader som sparas på rättslig grund döljs för alla vanliga läsningar tills perioden är slut. Ett rättsligt kvarhållande av en person gör att en radering får vänta.

## Det här deklarerar du

1. Märk personfälten med sin sort, till exempel `[Classification(Personal.Contact)]`.
2. Säg vems varje rad är och varför du sparar den: `[PersonalData(SupportNotes)]` på entiteten, där `SupportNotes` är en kategori som du deklarerar med syfte och rättslig grund, och `[DataSubject]` på fältet som pekar ut personen. Plattformen gör resten.
3. Deklarera vem som hanterar integritet och vem som lägger rättsliga kvarhållanden. En förfrågan är arbete i teamets inkorg, så inkorgens egna policyer deklareras också:

```
policy ManagesPrivacy => IsAdmin;
policy PlacesLegalHolds => IsAdmin;
policy WorksInbox => IsAdmin;
policy LeadsInbox => IsAdmin;
policy AdministersInbox => IsAdmin;
policy ManagesOrganizations => IsAdmin;
policy CollaboratesOnRecords => IsAdmin;
```

`osy docs privacy-kit` visar hela upplägget. `osy docs personal-data` beskriver klassificeringarna.

## Att få tag på kitet

Privacy-kitet är ännu inte publicerat som paket, så `osy lock` kan inte hämta det i dag och säger det. `osy search` listar det, med sin `use`-rad, när det har släppts. Att märka personfälten med `[Classification(Personal.…)]` fungerar redan i dag och är det som kitet bygger på.

## Relaterat

- [Säkerhetsregler: vem får läsa och skriva vad](/sv/help-centre/a/security-rules-who-can-read-and-write-what)
- [Lägg till ett kit i din app](/sv/help-centre/a/add-a-kit-to-your-app)
- [Lägg till inloggning i din app](/sv/help-centre/a/add-sign-in-to-your-app)
