# OData en OMS

temp: [](https://docs.ogc.org/is/18-088/18-088.html)

OData en OMS kunnen worden geïntegreerd. Voor OData geldt dat een Entity Data Model nodig is voor de beste werking. Vanuit het OMS én de Definitieboek kan een EDM worden gegenereerd als een CSDL.
De CSDL helpt dan ook met de Discovery functionaliteit van OData.

Via [aliassen](https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html#sec_Alias) kunnen complexe velddefinities worden vereenvoudigd.

``` Complicatie

Probleem:
---------

Filteren op Specimen is beperkt, omdat het model alleen referenties heeft naar Observations.
Door de natuur van ObservationCollection (namelijk: kan eigenschappen vasthouden die voor alle onderliggende Observations gelden) betekend het dat deze eigenschappen niet hoeven te worden opgenomen in onderliggende Observations. Dat maakt het moeilijk om te filteren van uit Specimen. De route zou dan zijn: 

Sample => Observations => ObservationCollections

Oplossing: we kunnen definiëren dat alleen de core-elementen voor een ObservationCollection wordt ingevuld.
De gedeelde informatie moet dan altijd in Observations worden opgenomen. 
In dat scenario kan Specimen ook goed zoeken in Observation.

Het is iets meer overhead, maar dit kan het zoeken flink optimaliseren. 
```

## Filteren

Er kunnen twee data-centrische 'hoofdingangen' worden onderscheiden waarop gefilterd kan worden:

- ObservationCollection
- Specimen (een specialisatie van Sampling)

ObservationCollections __bevatten__ Observations.
Specimen kunnen __refereren__ aan Observations.

Observations zijn dus altijd verbonden met een ObservationCollection.
Observations zonder ObservationCollections zijn dus niet mogelijk.

## ObservationCollection

Een ObservationCollection bevat geobserveerde waarden behorende bij dezelfde 'meting'.

## ObservationCollection filtering

| Filterargument | Alias | Operators | Functies  | Toegestaan | Type |
| ---------- | ------ | ---- | ---- | ---- | ---- |
| observation/type | observationType | not, has, in, eq, ne, in | |  measure, category-observation, count-observation,timeseries-observation, truth-observation | String |
| observation/phenomenonTime | phenomenonTime | not, has, in, eq, ne, lt, gt, le, ge | day, hour, minute, month, second, year | | DateTime |
| observation/observedProperty | observedProperty | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String  |
| observation/featureOfInterest/Geometry | locationGeo | not, eq, ne | | distance_to, intersects | WKT |
| observation/featureOfInterest/Geometry/latitude | lat | not, has, in, eq, ne, lt, gt, le, ge | |  | Number |
| observation/featureOfInterest/Geometry/longitude | lon | not, has, in, eq, ne, lt, gt, le, ge | | | Number |
| observation/featureOfInterest/Name | locationName | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |
| observation/samplingStrategy | strategy | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| observation/procedure | procedure | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| observation/resultTime | resultTime | not, has, in, eq, ne, lt, gt, le, ge | day, hour, minute, month, second, year | | DateTime |
| observation/parameter | parameter | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| observation/measure/uom | uom | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
| observation/measure/value | value | not, has, in, eq, ne, lt, gt, le, ge |  | | Number |
| observation/text | text | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |
| observation/truth | truth | not, has, in, eq, ne | | | Bool |
| observation/vocabTerm/term | term | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |
| observation/vocabTerm/vocabulary | vocab | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String |
| observation/link/href | observationRef  | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim | | String  |
| observation/link/rel | observationRel | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim  | | String  |
| observation/link/title | observationTitle | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring  | toupper, tolower, trim  | | String |

## Specimen

Een Sample is een monster. Een monster staat in wezen los van de de meetwaarden die eraan 'hangen'. Een Sample heeft referenties naar Observations.

### Specimen filtering

| Filterargument | Alias | Operators | Functies  | Toegestaan | Type |
| ---------- | ------ | ---- | ---- | ---- | ---- |
| sample/type | sampletype | eq, ne | toupper, tolower, trim | homogeneous, summarize  | String |
| sample/samplingTime | samplingTime | not, has, in, eq, ne, lt, gt, le, ge, in | day, hour, minute, month, second, year | | DateTime |
| sample/samplingLocation/Geometry | sampleGeo | not, eq, ne | | distance_to, intersects | WKT |
| sample/samplingLocation/Geometry/latitude | sampleLat | not, has, in, eq, ne, lt, gt, le, ge |  | | Number |
| sample/samplingLocation/Geometry/longitude | sampleLon | not, has, in, eq, ne, lt, gt, le, ge | |  | Number |
| sample/samplingLocation/Name | samplingLocation | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String |
