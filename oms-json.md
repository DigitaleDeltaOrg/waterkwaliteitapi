# OMS en JSON

OMS is groot, en JSON is breedsprakig. Een reden daarvoor is dat in de bestaande DD-API's telkens alle elementen werden herhaald, bijvoorbeeld meetobject/meetlocatie.
Om dit te voorkomen werkt de Waterkwaliteit-API met een referentiesysteem.
Een dergelijke aanpak is reeds een aantal keren besproken in de DD-API community, maar er was weinig voortgang geboekt.
JSON is bedoelt als compact formaat die echter toch door mensen leesbaar moet zijn.

In OMS is het gebruikelijk om aan entiteite te refereren, buiten het systeem.
In de meeste gevallen zal het Aquo zijn. Echter, Aquo maakt gebruikt van koppelen via Ids, bij Aquo de tekst 'Id-', gevolgd door uuid (universally unique identifier). Het is heel valide om geen betekenis aan een computersleutel te geven. Een nadeel echter, in een situatie waar de data herkenbaarheid en betekenis moet hebben, is de Aquo-sleutel dan niet.
Dan zou bijna net zo goed [BSON](https://bsonspec.org/) of [MsgPack](https://msgpack.org/) gebruikt kunnen worden.

Het doel is echter om de data leesbaar te houden. Ook hier kan een referentiesysteem helpen.

In het Waterkwaliteit-API referentiesystem wordt gebruik gemaak van [JSON Pointers](https://www.rfc-editor.org/rfc/rfc6901.html) om te refereren naar een in-document referentiesysteem.
Dat referentiesysteem kan weer links naar buiten het document hebben.

Hiertoe wordt in het JSON document een onderdeel aangemaakt genaamd /r, voor reference.
Daarin worden alle entiteiten die worden gebruikt binnen het document eenmalig beschreven.

Het referentieblok staat [hier](referentieblok.md) beschreven.

Het toevoegen van references levert geen problemen op: OMS beschrijft delen van het document (de payload van verschillende onderdelen), en wel het deel dat gerelateerd is aan een sample of een observation. Het beschrijft alleen de relevante elementen, dus bijvoorbeeld niet hoe een paging block er uit komt te zien.

Voor de Waterkwaliteit-API 
Een verdere complexiteit komt voort uit de relatie tussen Samples, ObservationCollections en Observations.
De links tussen Samples en Observationsa is altijd indirect: door referenties.
ObservationCollection kan echter Observations bevatten (embedden) óf er aan refereren.
Daarnaast kan de optionele SamplingStrategy van Observation een Sample bevatten óf eraan refereren. Wanneer het een Sample bevat, kan dat weer tot infinite loops en inconsistente data leiden. Daarnaast wordt de data mogelijk recursief, waardoor verwerking ingewikkeld wordt.

Door referenties (via JSON Pointers) te gebruiken i.p.v. embedded Observations (of embedded Samples) te gebruiken, kan het volgende worden vereenvoudigd:

- Samples en ObservationCollections kunnen losse entiteiten worden, die optioneel verwijderd kunnen worden uit de export kunnen worden. Veelal is er alleen interesse in Observations.
- Het import proces kan worden vereenvoudigd, doordat de volgorde van entiteiten in de import kan worden bepaald, en er geen updates hoeven plaats te vinden om entiteiten aan elkaar te relateren.

Daarom worden Observations, ObservationCollections en Samples als separate onderdelen binnen het bestand beschouwd.

De volgorde:

- references
- observations
- observationcollections
- samples
