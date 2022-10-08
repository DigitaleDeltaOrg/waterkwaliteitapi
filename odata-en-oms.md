# OData en OMS

temp: [](https://docs.ogc.org/is/18-088/18-088.html)

OData en OMS kunnen worden geïntegreerd. Voor OData geldt dat een Entity Data Model nodig is voor de beste werking. Vanuit het OMS én de Definitieboek kan een EDM worden gegenereerd als een CSDL.
De CSDL helpt dan ook met de Discovery functionaliteit van OData.

## Filteren

Op de volgende onderdelen kan worden gefilterd:

Via [aliassen](https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html#sec_Alias) in de CSDL is dit op te lossen.

| Filterargument | Alias | Operators | Functies  | Toegestaan | Type |
| ---------- | ------ | ---- | ---- | ---- | ---- |
| sample/type | sampletype | eq, ne | | homogeneous, summarize  | string |
| sample/samplingTime | samplingTime | eq, ne, lt, gt, le, ge, in | day, hour, minute, month, second, year | | Datetime |
| sample/samplingLocation/Geometry | sampleGeo | | | distance_to, intersects | WKT |
| sample/samplingLocation/Geometry/latitude | sampleLat | eq, ne, lt, gt, le, ge |  | Number |
| sample/samplingLocation/Geometry/longitude | sampleLon | eq, ne, lt, gt, le, ge |  | Number |
| sample/samplingLocation/Name | samplingLocation | eq, ne, startswith, endswith, like, in | | | String |
| observation/type | observationType | eq, ne, in | |  measure, category-observation, count-observation,timeseries-observation, truth-observation | String |
| observation/phenomenonTime | phenomenonTime | eq, ne, lt, gt, le, ge | day, hour, minute, month, second, year | | Datetime |
| observation/observedProperty | observedProperty | | |  |
| observation/featureOfInterest/Geometry | locationGeo | | | distance_to, intersects | WKT |
| observation/featureOfInterest/Geometry/latitude | lat | eq, ne, lt, gt, le, ge |  | Number |
| observation/featureOfInterest/Geometry/longitude | lon | eq, ne, lt, gt, le, ge |  | Number |
| observation/featureOfInterest/Name | locationName | eq, ne, startswith, endswith, like, in | | | String |
| observation/samplingStrategy | strategy | | |  |
| observation/procedure | procedure | | |  |
| observation/resultTime | resultTime | eq, ne, lt, gt, le, ge | day, hour, minute, month, second, year | | Datetime |
| observation/parameter | parameter | | | | String |
| observation/measure/uom | uom | | | | String |
| observation/measure/value | value | eq, ne, lt, gt, le, ge |  | | Number |
| observation/text | text | | | | String |
| observation/truth | truth | | | | Bool |
| observation/vocabTerm/term | term | | | | String |
| observation/vocabTerm/vocabulary | vocab |  | | | String |
| observation/link/href | observationRef  | | | | String  |
| observation/link/rel | observationRel | | | | String  |
| observation/link/title | observationTitle | | | | String |

Er wordt altijd standaard de hele boom met data geretourneerd, dus ```Sample -> Observation -> Meetwaarden```.
```$expand``` is daarom niet nodig.

## Selecteren

Het URI segment ```$select``` kan worden gebruik om bepaalde attributen te verwijderen uit het resultaat.
Indien ```$select``` niet is opgegeven, komen dus alle attributen in het resultaat mee.

Alleen timeseries: observationType eq 'timeseries-observation'
Alleen ecologie:  observationType ne 'timeseries-observation'
Debiet (grootheid): term eq  'DEBIET'

Wanneer gezocht wordt in observation op attributen, dan wordt tevens gezocht daarop in ObservationCollection.

Het uitvoerformaat is standaard een set van resultaten met een reference sectie en een paginablok.
