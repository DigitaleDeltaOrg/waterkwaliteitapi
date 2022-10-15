# DD-API mappings

dd-api -> OMS

- Geen Waarnemingssoorten: gebruik eenheid, parameter en grootheid
- Locatie-info wordt in het upload document opgenomen

Timeseries:

- Processname ->
- StartTime ->
- EndTime ->
- LocationCode -> reference(featureOfInterest)
- SourceName ->
- Eenheid ->
- RealizationCount? ->
- ValueType -> (ObservationType derivate) ->
- nodeId? ->
- qualifier ->
- aspectSet? ->
- Quantity -> reference(observedProperty)
- IntervalLength ->

Simplified:

DD-API: Timeseries met aanpassingen.

- location -> /ref/locations/...
- id -> unique
- source -> /ref/sources/...
- observationtype -> uitgesplitst in /ref/quantity/..., /ref/parameter/..., /ref/compartment/...
- qualifier -> /ref/qualifier/...

Iedere timeseries wordt een TimeseriesObservation. Er is geen ObservationCollection of Sample.

``` json


{ 
    "references": [
        "quantity": {  "Id": "", "code": "Debiet", "name": "", "href": "https://www.aquo.nl/index.php/Id-01b89ecb-6f63-4463-b3c7-d9a712d7ed1e"},
        "unit": {  "Id": "", "code": "m3_min", "name": "", "href": "https://www.aquo.nl/index.php/Id-e7385e19-fd7f-4ce5-8099-36d15f733907"},
        "compartment": {  "Id": "", "code": "PC", "name": "", "href": "https://www.aquo.nl/index.php/Id-6134f3bb-6048-431d-a130-01290d84172c"},
        "location": {  "Id": "", "code": "VEEN", "name": "", "href": "https://www.aquo.nl/index.php/Id-6134f3bb-6048-431d-a130-01290d84172c", "geo": {}},
        "source": { "Id": "", "code": "NS_23", "name": ""},
    ]
},


{
    "Observations": [
        {
            "id": "<someuniquenumber>",
            "type": "Scalar",
            "start": "2012-01-10",
            "end": "2012-01-11",
            "location": "VEEN",
            "source": "#/references/source/NS_23",
            "quantity": "#/references/quantity/Debiet",
            "unit": "#/references/unit/m3_min",
            "compartment": "#/references/compartment/PC",
            "result": [
                {
                    "resultTime": "2012-01-10 08:00", "value": "string|number", "limitSymbol": "", "quality": ""
                }
            ]
        }
    ]

}
```
