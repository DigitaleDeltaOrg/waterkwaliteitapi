# Opvragen: Filteren/selecteren

Opvragen van de data zal gebeuren via OData. De [SensorThings API](https://docs.ogc.org/is/18-088/18-088.html) geeft hiervoor handvatten, echter is de complexiteit bij ecologische metingen groter, doordat meetwaarden in context bewaard moeten worden.

De volgende OData features worden ge&iuml;mplementeerd:

- ```$filter```: eq, ne, gt, ge, lt, le, contains, startswith, endswith, year, month, day, hour, minute, second, and, or, not, geo.intersects, geo.distance
- ```$select```
- ```$skip```
- ```$top```
- ```$count```: wanneer True, dan wordt @count in het [paginablok](paginablok.md) gevuld. ```$count=true``` kan de performance beïnvloeden.
- Data types: Boolean, Int16/32/64, Double, Decimal(p, s), Guid, String, Date, DateTime, DateTimeOffset, Time, TimeOfDay, Duration, Geography (WKT)

De uitvoerformaten (behalve OGC OMS) worden, net als alle attributen, vastgelegd in het Definitieboek. In ieder geval wordt het offici&euml;le OGC OMS ge&euml;xporteerd.

Vanuit het Definitieboek zal de CSDL voor OData worden gegenereerd via een REST webservice.
De OpenAPI Specification kan worden via een merge actie worden gegenereerd via een REST webservice.

_Uitzoeken: Is ```$expand``` nodig?_

Operaties om data toe te voegen, wijzigen of verwijderen worden **niet** ondersteund.

De te gebruiken zoekmogelijkheden staan beschreven in het hoofdstuk [OMS en data](odata-en-oms.md).
De te gebruiken exportformaten staan beschreven in het hoofdstuk [OMS export opbouw](oms-export-opbouw.md).
