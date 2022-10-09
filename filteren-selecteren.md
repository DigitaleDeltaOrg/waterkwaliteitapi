# Opvragen: Filteren/selecteren

Opvragen van de data zal gebeuren via OData.
De voor OData benodige CSDL zal worden gegenereerd vanuit het Definitieboek van de Digitale Delta.
Beveiliging van het zoeken zal gebeuren via OpenID Connect voor interactieve toepassingen en OAUTH2 Machine-to-machine voor machine-koppelingen.
De client moet kunnen selecteren welke eigenschappen terug worden gestuurd in de response.

De volgende OData features worden ge&iuml;mplementeerd:

- ```$filter```: eq, ne, gt, ge, lt, le, contains, startswith, endswith, year, month, day, hour, minute, second, and, or, not, geo.intersects, geo.distance
- ```$select```
- ```$skip```
- ```$top```
- ```$count```: wanneer True, dan wordt @count in het [paginablok](paginablok.md) gevuld. ```$count=true``` kan de performance beïnvloeden.
- Data types: Boolean, Int16/32/64, Double, Decimal(p, s), Guid, String, Date, DateTime, DateTimeOffset, Time, TimeOfDay, Duration, Geography (GeoJSON)

De ```$expand``` en ```$search``` laten we in deze versie buiten beschouwing.

De uitvoerformaten (behalve OGC OMS) worden, net als alle attributen, vastgelegd in het Definitieboek. In ieder geval wordt het offici&euml;le OGC OMS ge&euml;xporteerd.

Vanuit het Definitieboek zal de CSDL voor OData worden gegenereerd via een REST webservice.
De OpenAPI Specification kan worden via een merge actie worden gegenereerd via een REST webservice.

_Uitzoeken: Is ```$expand``` nodig?_

Operaties om data toe te voegen, wijzigen of verwijderen worden **niet** ondersteund.
