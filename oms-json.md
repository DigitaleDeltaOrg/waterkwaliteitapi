# OMS en JSON

OMS is groot, en JSON is breedsprakig. Een reden daarvoor is dat in de bestaande DD-API's telkens alle elementen werden herhaald, bijvoorbeeld meetobject/meetlocatie.
Om dit te voorkomen werkt de Waterkwaliteit-API met een referentiesysteem.
Een dergelijke aanpak is reeds een aantal keren besproken in de DD-API community, maar er was weinig voortgang geboekt.

Deze API introduceert daarom een referentie systeem.
Hierbij wordt in het JSON document een onderdeel aangemaakt genaamd /r, voor reference.
Daarin worden alle entiteiten die worden gebruikt binnen het document eenmalig beschreven.
De data die daar gebruik van maakt, verwijst via [JSON Pointers](https://www.rfc-editor.org/rfc/rfc6901.html) naar de referentie.

Het referentieblok staat [hier](referentieblok.md) beschreven.

Het toevoegen van references levert geen problemen op: OMS beschrijft delen van het document, en wel het deel dat gerelateerd is aan een sample of een observation. Het beschrijft alleen de relevante elementen, dus bijvoorbeeld niet hoe een paging block er uit komt te zien.
