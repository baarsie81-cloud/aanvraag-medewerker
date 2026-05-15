# Medewerkersdashboard verlof en overuren

Dit is een simpel statisch medewerkersdashboard voor verlof- en overurenaanvragen. Medewerkers kunnen met een persoonlijke code hun eigen saldo bekijken en daarna een aanvraagmail openen voor hun leidinggevende.

Het dashboard is bewust beperkt:

- geen database
- geen backend
- geen Supabase
- geen SharePoint Lists
- geen centrale personeelsdata aanpassen
- geen ziektefunctionaliteit
- geen medische of gevoelige gegevens verwerken
- geen opslag in localStorage

Het aparte leidinggevende-dashboard blijft de centrale waarheid. De leidinggevende verwerkt aanvragen handmatig in dat dashboard.

## Hoe persoonlijke codes werken

Een medewerker vult een persoonlijke code in, bijvoorbeeld:

```text
TEST001
```

De app schoont de code automatisch op:

- spaties worden verwijderd
- letters worden hoofdletters
- alleen veilige tekens blijven over voor de bestandsnaam

Daarna probeert de app dit bestand te laden:

```text
saldi/saldo-TEST001.json
```

Als het bestand niet bestaat, krijgt de medewerker een nette foutmelding.

## Waar saldo-bestanden staan

Persoonlijke saldo-bestanden staan in de map `saldi/` en volgen deze naamgeving:

```text
saldi/saldo-CODE.json
```

Voorbeeld:

```text
saldi/saldo-JAN7392.json
```

## Belangrijke privacywaarschuwing

Zet echte saldo-json-bestanden niet in een publieke GitHub-repository. Een publieke repo is voor iedereen zichtbaar. Gebruik in een publieke repo alleen demo- of testdata.

Voor echte medewerkersdata is een afgeschermde omgeving nodig, bijvoorbeeld een private repository, een beveiligde hostingomgeving of een andere interne distributiemethode. Ook dan geldt: dit medewerkersdashboard past geen centrale personeelsdata aan en leest alleen het saldo-bestand van de ingevoerde code.

## Voorbeeld saldo-json

```json
{
  "code": "JAN7392",
  "naam": "Jan Jansen",
  "email": "jan@bedrijf.nl",
  "leidinggevendeEmail": "leidinggevende@bedrijf.nl",
  "verlofsaldo": 128,
  "overurensaldo": 6,
  "laatstBijgewerkt": "2026-05-15 16:30"
}
```

## Aanvragen via mailto

Het dashboard verzendt zelf geen aanvraag. Na klikken op **Open mail om aanvraag te verzenden** opent de browser een mailto-link in het mailprogramma van de medewerker.

De medewerker moet de mail daarna zelf controleren en op verzenden klikken. Pas daarna is de aanvraag ingediend.

De mail wordt opgesteld met:

- aan: `leidinggevendeEmail` uit het persoonlijke saldo-bestand
- cc: `email` van de medewerker uit het persoonlijke saldo-bestand
- onderwerp en inhoud op basis van het gekozen aanvraagtype

## Testen

Er is demo-data aanwezig voor code:

```text
TEST001
```

Bij gebruik via GitHub Pages of een lokale statische webserver kan de app het bestand `saldi/saldo-TEST001.json` ophalen en tonen.

Let op: moderne browsers blokkeren `fetch()` vaak wanneer `index.html` direct als lokaal bestand met `file://` wordt geopend. Gebruik daarom bij voorkeur GitHub Pages of een eenvoudige lokale statische server.
