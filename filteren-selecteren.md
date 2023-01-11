# Opvragen: Filteren/selecteren

Opvragen van de data zal gebeuren via OData.

De volgende OData features worden ge&iuml;mplementeerd:

- ```$filter```: eq, ne, gt, ge, lt, le, contains, startswith, endswith, year, month, day, hour, minute, second, and, or, not, geo.intersects, geo.distance
- ```$select```
- ```$skip```
- ```$top```
- ```$count```: wanneer True, dan wordt @count in het [paginablok](paginablok.md) gevuld. ```$count=true``` kan de performance beïnvloeden.
- Data types: Boolean, Int16/32/64, Double, Decimal(p, s), Guid, String, Date, DateTime, DateTimeOffset, Time, TimeOfDay, Duration, Geography (WKT)

Het uitvoerformaat is OData met als payload de definitie in [het waterkwaliteit JSON schema](waterkwaliteit-api.json).

OData werkt met een database model. Deze specificatie gaat echter niet vastleggen wat het onderliggende model moet zijn.
Daarom wordt er een virtuele definitie gemaakt ten behoeve van OData. Een OData verzoek kan dan via een Visitor design pattern worden vertaald naar een verzoek voor het onderliggende datamodel.
De virtuele definitie is beschreven in de vorm van een [Common Schema Definition Language](https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html) document.
Deze is [hier](waterkwaliteit-api.edmx) te vinden.
Daarnaast voorziet deze specicatie in een Open API specificatie, die kan dienen voor het beschrijven en specificeren van de REST services.
