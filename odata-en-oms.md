# OData en OMS

temp: [](https://docs.ogc.org/is/18-088/18-088.html)

OData en OMS kunnen worden geïntegreerd. Voor OData geldt dat een Entity Data Model nodig is voor de beste werking. Vanuit het OMS én de Definitieboek kan een EDM worden gegenereerd als een CSDL.
De CSDL helpt dan ook met de Discovery functionaliteit van OData.

Via [aliassen](https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html#sec_Alias) kunnen complexe velddefinities worden vereenvoudigd.

## Filteren op Specimen of ObservationCollection?

Beide zijn legitime ingangen, voor andere doelgroepen.

Het probleem is echter dat er een directe relatie ligt tussen ```ObservationCollection``` en ```Observation```, want de eerste omsluit de tweede.
Echter tussen ```Specimen``` en ```Observation``` is er alleen een referentie.
```ObservationCollection``` heeft de mogelijkheid om data te bepalen die gelden voor alle onderliggende ```Observation```s. Dit kan echter leiden tot problemen wanneer men wil gaan selecteren op ```Specimen```.
Immers zou het zoeken gaan via ```Specimen``` naar ```Observation``` naar ```ObservationCollection```.
Dat zorgt voor grote performance verliezen, op zijn minst.

Het is daarom prudenter om bij ```ObservationCollection``` alleen de basiszaken bij te houden, dus Id en member.
De andere eigenschappen kunnen beter aan ```Observation``` worden toegekend, ondanks dat het iets meer data genereerd.

Hiermee kan in wezen de verschillende ingangen (```Specimen``` of ```ObservationCollection```) vervallen: er hoeft alleen gezocht te worden op waarden van ```Observation```, met als secundaire filter specifieke ```Specimen``` eigenschappen.

Daarmee kan de uitvoer óók worden geuniformeerd: er komen in alle gevallen separate delen voor ```ObservationCollection``` en ```Specimen```. De geunformeerde zoekingang kan dan eenvoudigweg ```Observation``` heten.

## Filteren

Er komt één data-centrisch 'hoofdingang': Observation.

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
