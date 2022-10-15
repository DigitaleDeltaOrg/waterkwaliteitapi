# OData en OMS

Het opvraagmechanisme van [SensorThings API](https://docs.ogc.org/is/18-088/18-088.html) geeft een houvast aan hoe aan API voor metingen, gecombineerd met OData eruit kan zien.
Echter, SensorThings lijkt niet gebruik te maken van ObservationCollection, waardoor de meetwaarden los staan. Echter is bij Waterkwaliteit-API juist de samenhang van de meetwaarden van belang.

Het create/update/delete mechanisme van SensorThings API zal *niet* worden gebruikt. Toevoegen van data zal in grote volumes gaan, dus een [bulk-geörienteerde API](bulkverwerking.md) is beter geschikt voor de Waterkwaliteit-API.

Door toevoeging van de ObservationCollection laag tussen Sample (Specimen) en Observation is dat echter wel te bewerkstelligen.
Daarnaast is er veel meer metadata benodigd bij het vastleggen van ecologische waarnemingen. Dat betekent veel meer endpoints in de situatie zoals beschreven in SensorThings API. Echter, door gebruik te maken van het [referentiessysteem](referentieblok.md), is dat ook op te lossen.

OData en OMS kunnen worden geïntegreerd. Voor OData geldt dat een Entity Data Model nodig is voor de beste werking. Vanuit het OMS én de Definitieboek kan een EDM worden gegenereerd als een CSDL.
De CSDL helpt dan ook met de Discovery functionaliteit van OData.

Via [aliassen](https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html#sec_Alias) kunnen complexe velddefinities worden vereenvoudigd.

## Filteren

De volgende endpoints zijn nodig:

- ```/References``` -> Alle gereferenties die binnen het systeem bekend zijn, zoals locaties, parameters, grootheden, eenheden, hoedanigheden, etc.
- ```/Measurements``` -> Specimen en Observations

### Reference filtering

```/References``` retourneert een lijst van bekende types.
```/Reference?$filter=type eq 'locations'``` haalt alle locaties op.
```/Reference?$filter=type eq 'locations' and geo.distance(Location/geo, geography'POINT(-122 43)') gt 1)``` haalt aleen locaties op met een maximale afstand van opgegeven WKT locatie.
```/Reference?$filter=code eq '123'``` haalt alle referenties op, ongeacht het type, met code '123'.

### Measurement filtering

De filtermogelijkheden voor Measurements zijn:

| Filterargument | Operators | Functies  | Toegestaan | Type |
| ---------- | ---- | ---- | ---- | ---- |
| ```sample(id)``` | |  | | String |
| ```sample/samplingTime``` | not, has, in, eq, ne, lt, gt, le, ge, in | day, hour, minute, month, second, year | | DateTime | |
| ```sample/samplingLocation(id)``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```sample/samplingLocation/Code```  | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```sample/samplingLocation/Geometry```  | not, eq, ne | | geo.distance, geo.length, geo.intersects | WKT |  |
| ```sample/samplingLocation/Name```  | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String ||
| ```observation(id)``` | | | | String |
| ```observation/type``` | not, has, in, eq, ne, in | |  measure, category-observation, count-observation, truth-observation | String |
| ```observation/phenomenonTime``` | not, has, in, eq, ne, lt, gt, le, ge | day, hour, minute, month, second, year | | DateTime |
| ```observation/observedProperty(id)``` | | | | String |
| ```observation/observedProperty/Code``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String  |
| ```observation/observedProperty/Name``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String  |
| ```observation/featureOfInterest(id)``` | | | | String |
| ```observation/featureOfInterest/Geometry``` | not, eq, ne | | geo.distance, geo.length, geo.intersects | WKT |
| ```observation/featureOfInterest/Name``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |
| ```observation/samplingStrategy(id))```  | | | | String |
| ```observation/samplingStrategy/Code``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```observation/samplingStrategy/Name``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```observation/procedure``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```observation/resultTime``` | not, has, in, eq, ne, lt, gt, le, ge | day, hour, minute, month, second, year | | DateTime |
| ```observation/parameter(id))```  | | | | String |
| ```observation/parameter/Code``` |  not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```observation/parameter/Name``` |  not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```observation/measure/uom(id))```  | | | | String |
| ```observation/measure/uom/Code``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```observation/measure/uom/Name``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| ```observation/measure/value``` | not, has, in, eq, ne, lt, gt, le, ge |  | | Number |
| ```observation/text``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |
| ```observation/truth``` | not, has, in, eq, ne | | | Bool |
| ```observation/vocabTerm/term``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |
| ```observation/vocabTerm/vocabulary``` | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |

Paden zijn mogelijk: ```/sample(1)/observationcollection(1)/measure/uom(1)``` is een valide zoekopdracht, maar zal zeer onwaarschijnlijk worden gebruikt.

$count is altijd op basis van het aantal samples.



Opmerking: OData kan momenteel niet zoeken op GeoJSON. Het ondersteund wél zoeken via WKT.
