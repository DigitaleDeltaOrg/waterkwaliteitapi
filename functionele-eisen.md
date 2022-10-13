# Functionele eisen

De Waterkwaliteit-API heeft een aantal functionele eisen. Die staan hieronder verder uitgelegd.

## Zowel export (opvragen) and import (data toevoegen of wijzigen)

Via de Waterkwaliteit API moet zowel data kunnen worden [opgevraagd in OData formaat](filteren-selecteren.md).
Data moet ook kunnen worden toegevoegd of verwijderd, [in bulk formaat](aanbieden-bulk-verwerking).

## Standaarden

De API committeert zich zoveel mogelijk aan [standaarden](standaarden.md).
Daarnaast worden een aantal zaken aangescherpt, zoals welke delen we gebruiken, compacter maken van de data, etc.

## Taal

Er wordt zoveel mogelijk gebruik gemaakt van Engelse termen in imports en exports. Namen van entiteiten zijn daarom zoveel mogelijk Engels. Dit om verschillen in uitwisselingen met andere landen te voorkomen.

## Fouten

Fouten worden gestandaardiseerd weergegeven.
De HTTP Status codes (o.a. fouten) zijn beschreven in onderdeel [HTTP Fouten](http-fouten.md).
Validatiefouten zijn beschreven in onderdeel [validatie](validatie.md).

## Buiten scope

- Deze specificatie houdt zich primair bezig met tijdreeksen en observaties. In de Waterkwaliteit-API worden grids gezien als buiten de scope van het project. Dat geldt ook voor MultiPoint- en AspectSets-gebaseerde timeseries.
- Het OMS model is _enorm_. De beschrijving is maar liefst 130 pagina's lang. Er wordt in deze API slechts rekening gehouden met onderdelen die voor de API van belang zijn.
