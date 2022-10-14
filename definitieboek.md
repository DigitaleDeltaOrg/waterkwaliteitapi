# Definitieboek

In het Definitieboek worden entiteiten en hun mogelijke attributen vastgelegd in een eenvoudig Excel spreadsheet.
Deze kan worden gebruikt voor het genereren van onder anderen de [Common Schema Definition Language (CSDL)](https://docs.oasis-open.org/odata/odata-csdl-xml/v4.01/os/odata-csdl-xml-v4.01-os.html) definitie die gebruikt kan worden voor de [OData opvragingen](filteren-selecteren.md).

## Aquo toevoegingen

Aquo legt wel grootheden vast, maar er wordt niet gedefinieerd in welke eenheden een dergelijke grootheid mag worden vastgelegd.
Bij typeringen legt Aquo ook niet vast wat de acceptable waarden zijn.

Dit kan leiden tot inconsistenties en (interpretatie-)fouten.

Het Definitieboek kan een dienen als een bron om deze waarden wel te gaan standaardiseren.

## Observation referenties

- BiologischKenmerk\Gedrag (CategoryObservation)
- BiologischKenmerk\Lengteklasse (CategoryObservation)
- BiologischKenmerk\Levensstadium (CategoryObservation)
- BiologischKenmerk\Levensvorm (CategoryObservation)
- BiologischKenmerk\Verschijningsvorm (CategoryObservation)
- Compartiment (CategoryObservation)
- Eenheid (CountObservation)
- Geslacht (CategoryObservation)
- Hoedanigheid\Koolwaterstoffractie (CategoryObservation)
- Hoedanigheid\Korrelgroottefractie (CategoryObservation)
- J of N (TruthObservation)
- Kleur (CategoryObservation)
- Kleursterkte (CategoryObservation)
- Kwaliteitsoordeel (CategoryObservation)
- Meetapparaat (CategoryObservation)
- Meetinstantie (CategoryObservation)
- Monsterbewerkingsmethode (CategoryObservation)
- MonsterVoorbehandeling (CategoryObservation)
- Meetapparaat (CategoryObservation)
- Meetapparaat (CategoryObservation)
- Parameter (inclusief Biotaxon)
- RelatedObservationRollen
- RelatedSamplingFeatureRollen
- Waardebepalingsmethode
- Waardebepalingstechniek
- Waardebepalingsmethode
- Parameters\Grootheid -> ObservedProperty

Toevoegingen: mogelijk nog niet in Aquo, of slechts deels:

- Statistiek
- Sediment
- Golflengte
- Conditie
- Kwaliteitsoordeel
- Habitat
- Meetpositie

## Sample referenties

- Bemonsteringapparaat
- Bemonsteringsmethode
- Bemonsteringssoort
- Bodemsoort
- KenmerkBodemlaag
- KenmerkMonstername
- LocatietypeWaardeBepaling
- Onderzoekssoort
- RelatedObservationRollen
- RelatedSamplingFeatureRollen
