# Omgaan met data

Er zijn een aantal regels met betrekking tot het omgaan met data binnen de Waterkwaliteit API.

## Omgaan met JSON

JSON kan een breedsprakig formaat zijn, dus groot.
Om de data volume klein te houden, **moeten** client en server beide gebruik maken van data compressie, en wel Brotli compressie. De meeste ontwikkel-frameworks en alle browsers beschikken daarover. Dit zal resulteren in een gemiddelde compressie van 15-21%. Dit geldt zowel voor opvragen als aanbieden dan data.

In gebruikelijke DD-* formaten wordt data vaak herhaald. Denk hierbij aan meetobjecten.
Het volgende kan een uitkomst bieden.

## Omgaan met hergebruik (linked data)

Om het data volume zo klein mogelijk te houden, wordt gebruikt gemaakt van referenties, gebaseerd op [JSON for Linking Data](https://json-ld.org/).
Boven in de data (zowel export als import) wordt een referentietabel gespecificeerd. Die bevat alle in de dataset gebruikte relevante entiteiten, bijvoorbeeld grootheden, eenheden, parameters, meetobjecten, etc.

In de data wordt gerefereerd aan die referentietabel via zogenaamde [JSON Pointers](https://www.rfc-editor.org/rfc/rfc6901.html). Dat is in wezen niet meer dan een URL die kan linken naar een intern onderdeel van de data, of een extern onderdeel.
Voor de Waterkwaliteit-API (en de 1-API) zullen de referenties intern in het document zitten. De reden daarvoor is leesbaarheid. Binnen de referentietabel kan wel naar een externe bron worden verwezen, zoals Aquo.

Een referentiesysteem als dit is al een aantal keren besproken in de DD-API groep, maar er is nooit een klap opgegeven.
De hier beschreven methode voldoet meer aan de omarmde standaards.

Meer informatie staat beschreven in [referentieblock](referentieblok.md).

_Uitzoeken: Is dit voldoende voor NEN3610: Linked Data?_

Aan GeoJSON zitten ook wat haken en ogen. Daarom het volgende:

## Omgaan met GeoJSON

Er wordt geografische data overgedragen in de vorm van GeoJSON binnen de API.  In het verleden bezat GeoJSON een aanduiding voor het geografisch stelsel waarin de data was geprojecteerd. Echter is die definitie verwijderd uit de standaard, wegens interpretatieverschillen.
Daarom wordt hier Kennisplatform APIs aangehouden. Daar is de volgende oplossing bedacht:

- De client kan aangeven in de Content-Crs header om welke CRS het gaat in het bestand.  De waarde hiervan wordt uitgedrukt als EPSG:xxxxx, waarbij xxxxx de vier- of vijfcijferige [EPSG](epsg.io) code is.
- Wanneer de server niet om kan gaan met de CRS, dan kan de server een BAD REQUEST teruggeven, met daarin een Accept-Crs header met als data de CRS'en die de server wel accepteert. Dit wordt beschouwd als een validatiefout.

Het effect is dat het opgegeven CRS geldt voor alle GeoJSON binnen de download dan wel upload.
De server dient minimaal de volgende CRS'en te ondersteunen: EPSG:4326 (WGS84), EPSG:28992 (Amersfoort/RD New), ESPG:4258 (ETRS89). Het wordt aangeraden om ook de specifieke EPSG's voor onze regio te gebruiken (de 31n varianten).

Voor conversie kan RDNAPTRANS of een omgeving-specifieke Proj-library worden gebruikt.

De Proj-libraries zijn eenvoudiger in gebruik binnen APIs dan RDNAPTRANS.
