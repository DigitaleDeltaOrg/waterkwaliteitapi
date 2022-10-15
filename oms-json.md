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

Hiertoe wordt in het JSON document een onderdeel aangemaakt genaamd /reference.

Het referentieblok staat [hier](referentieblok.md) beschreven.

Het toevoegen van references levert geen problemen op: OMS beschrijft delen van het document (de payload van verschillende onderdelen), en wel het deel dat gerelateerd is aan een sample of een observation. Het beschrijft alleen de relevante elementen, dus bijvoorbeeld niet hoe een paging block er uit komt te zien.

Voor de Waterkwaliteit-API is context altijd van belang, omdat meetwaarden bij elkaar kunnen horen. Pas dan geven de meetwaarden betekenis. Omdat slechts bepaalde soorten Observaties verbanden hebben met Samples en ObservationCollections, worden Observaties niet gekoppeld aan Samples en ObservationCollections, maar juist andersom: ObservationCollections en Samples verwijzen naar Observaties, via hun afzonderlijke eigenschappen.
