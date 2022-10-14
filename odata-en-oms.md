# OData en OMS

temp: [](https://docs.ogc.org/is/18-088/18-088.html)

OData en OMS kunnen worden geïntegreerd. Voor OData geldt dat een Entity Data Model nodig is voor de beste werking. Vanuit het OMS én de Definitieboek kan een EDM worden gegenereerd als een CSDL.
De CSDL helpt dan ook met de Discovery functionaliteit van OData.

Via [aliassen](https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html#sec_Alias) kunnen complexe velddefinities worden vereenvoudigd.

## Filteren

Er komt één data-centrisch 'hoofdingang': Observation. Deze zal standaard óók de Specimen en ObservationCollections retourneren.
Via $select kan gekozen worden ome ze niet mee te geven.

| Filterargument | Alias | Operators | Functies  | Toegestaan | Type |
| ---------- | ------ | ---- | ---- | ---- | ---- |
| sample/type | sampletype | eq, ne | toupper, tolower, trim | homogeneous, summarize  | String |
| sample/samplingTime | samplingTime | not, has, in, eq, ne, lt, gt, le, ge, in | day, hour, minute, month, second, year | | DateTime |
| sample/samplingLocation/Geometry | sampleGeo | not, eq, ne | | distance_to, intersects | WKT |
| sample/samplingLocation/Geometry/latitude | sampleLat | not, has, in, eq, ne, lt, gt, le, ge |  | | Number |
| sample/samplingLocation/Geometry/longitude | sampleLon | not, has, in, eq, ne, lt, gt, le, ge | |  | Number |
| sample/samplingLocation/Name | samplingLocation | not, has, in, eq, ne, startswith, endswith, contains, concat, indexof, length, substring | toupper, tolower, trim | | String || observation/type | observationType | not, has, in, eq, ne, in | |  measure, category-observation, count-observation,timeseries-observation, truth-observation | String |
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
