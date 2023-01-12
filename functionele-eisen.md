# Functionele eisen

De Waterkwaliteit-API heeft een aantal functionele eisen. Die staan hieronder verder uitgelegd.

## Zowel export (opvragen) and import (data toevoegen of wijzigen)

Via de Waterkwaliteit API moet zowel data kunnen worden [opgevraagd in OData formaat](filteren-selecteren.md). Het exportformaat moet voldoen aan de specificaties zoals aangegeven in het [waterkwaliteit-api-oas.yaml](Open API model). Een visuele representatie kan [Hier](https://editor.swagger.io/?url=https://raw.githubusercontent.com/DigitaleDeltaOrg/waterkwaliteitapi/main/waterkwaliteit-api-oas.yaml) worden bekeken.
Data moet ook kunnen worden toegevoegd of verwijderd, [in bulk formaat](bulkverwerking.md).

## Standaarden

De API committeert zich zoveel mogelijk aan [standaarden](standaarden.md).
Daarnaast worden een aantal zaken aangescherpt, zoals welke delen we gebruiken, compacter maken van de data, etc.

## Taal

Er wordt zoveel mogelijk gebruik gemaakt van Engelse termen in imports en exports. Namen van entiteiten zijn daarom zoveel mogelijk Engels. Dit om verschillen in uitwisselingen met andere landen te voorkomen.

## Fouten

Fouten worden gestandaardiseerd weergegeven.
De HTTP Status codes (o.a. fouten) zijn beschreven in onderdeel [HTTP Fouten](http-fouten.md).
Validatiefouten zijn beschreven in onderdeel [validatie](validatie.md).

## Beveiliging

[Beveiliging](beveiliging.md) is uiteraard een belangrijk onderdeel van deze specificatie.
