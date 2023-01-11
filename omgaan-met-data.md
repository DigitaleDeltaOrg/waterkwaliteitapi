# Omgaan met data

Er zijn een aantal regels met betrekking tot het omgaan met data binnen de Waterkwaliteit-API.

## Omgaan met JSON

JSON kan een breedsprakig formaat zijn, dus groot.
Om de data volume klein te houden, **moeten** client en server beide gebruik maken van data compressie, en wel Brotli compressie. De meeste ontwikkel-frameworks en alle browsers beschikken daarover. Dit zal resulteren in een gemiddelde compressie van 15-21%. Dit geldt zowel voor opvragen als aanbieden dan data.

Aan GeoJSON zitten ook wat haken en ogen. Daarom het volgende:

## Omgaan met GeoJSON

Er wordt geografische data overgedragen in de vorm van GeoJSON binnen de API.  In het verleden bezat GeoJSON een aanduiding voor het geografisch stelsel waarin de data was geprojecteerd. Echter is die definitie verwijderd uit de standaard, wegens interpretatieverschillen.
Daarom wordt hier Kennisplatform APIs aangehouden. Daar is de volgende oplossing bedacht:

- De client kan aangeven in de ```Content-Crs``` header om welke CRS het gaat in het bestand.  De waarde hiervan wordt uitgedrukt als EPSG:xxxxx, waarbij xxxxx de vier- of vijfcijferige [EPSG](epsg.io) code is.
- Wanneer de server niet om kan gaan met de CRS, dan kan de server een BAD REQUEST teruggeven, met daarin een ```Accept-Crs``` header met als data de CRS'en die de server wel accepteert. Dit wordt beschouwd als een validatiefout.

Het effect is dat het opgegeven CRS geldt voor alle GeoJSON binnen de download dan wel upload.
De server dient minimaal de volgende CRS'en te ondersteunen: ```EPSG:4326``` (WGS84), ```EPSG:28992``` (Amersfoort/RD New), ```ESPG:4258``` (ETRS89).
Het wordt aangeraden om ook de specifieke EPSG's voor onze regio te gebruiken (de 31n varianten).

Voor conversie kan RDNAPTRANS of een omgeving-specifieke Proj-library worden gebruikt.

De Proj-libraries zijn eenvoudiger in gebruik binnen APIs dan RDNAPTRANS.
