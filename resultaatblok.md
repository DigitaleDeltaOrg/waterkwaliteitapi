# Resultaatblok

Omdat in OMS er geen directe relatie bestaat tussen monster en observatiecollecties, worden beiden als separate entiteiten in de resultaatset meegegeven:

{ "result": {
    "samples": [],
    "observationcollections": []
}}

Het volgende voorbeeld is een ```ObservationCollection``` met alle mogelijke ```Observation```-types voor de Waterkwaliteit-API.
De data is fictief en deze combinatie van timeseries en niet-timeseries is erg onwaarschijnlijk.

``` json
{ 
    "observationcollections": [
        "id": "observation collection a34",
        "phenomenonTime": { "instant":"2015-05-11" },
        "featureOfInterest": { "href":"http://example.org/sighting345" },
        "member": [
            {
                "id":"o96",
                "type": "CategoryObservation",
                "observedProperty": { "href": "http://environment.data.gov.au/def/property/taxon" },
                "procedure": { "href":"http://www.opengis.net/def/waterml/2.0/processType/Manual" },
                "resultTime": "2015-05-12T09:00:00+10:00",
                "result": { "term": "johnstons-crocodile", "vocabulary": "http://environment.data.gov.au/def/object/"
                }
            },
            {
                "id": "boolean-test-instance",
                "type": "TruthObservation",
                "phenomenonTime": { "instant": "2011-05-11T00:00:00+10:00" },
                "observedProperty": { "href": "urn:example:Truth" },
                "procedure": { "href": "http://www.opengis.net/def/waterml/2.0/processType/Manual" },
                "featureOfInterest": { "href": "http://waterml2.csiro.au/rgs-api/v1/monitoring-point/419009/" },
                "resultTime": "2011-05-12T09:00:00+10:00" ,
                "result": true
            },
            {
                "id": "boolean-test-instance",
                "type": "CountObservation",
                "phenomenonTime": { "instant": "2011-05-11" },
                "observedProperty": { "href": "urn:example:Frequency" },
                "procedure": { "href": "http://www.opengis.net/def/waterml/2.0/processType/Manual" },
                "featureOfInterest": { "href": "http://waterml2.csiro.au/rgs-api/v1/monitoring-point/419009/" },
                "resultTime": "2011-05-12T09:00:00+10:00",
                "result": 97
            },
            {
                "id":"measure-instance-test",
                "type" : "Measurement",
                "phenomenonTime": { "instant":"2011-05-11T00:00:00+10:00" },
                "observedProperty": {"href":"http://environment.data.gov.au/def/property/air_temperature"},
                "procedure": {"href":"http://www.opengis.net/def/waterml/2.0/processType/Sensor"},
                "featureOfInterest": {"href":"http://waterml2.csiro.au/rgs-api/v1/monitoring-point/419009/"},
                "resultTime": "2011-05-12T09:00:00+10:00",
                "result": { "value": 3.2, "uom": "http://qudt.org/vocab/unit#DegreeCelsius" }
            },
            {
                "id":"timeseries-instance-test",
                "type" : "http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_DiscreteTimeSeriesObservation",
                "phenomenonTime": { "begin":"2015-08-03", "end":"2015-08-05" },
                "observedProperty": {"href":"http://environment.data.gov.au/def/property/air_temperature"},
                "procedure": {"href":"http://www.opengis.net/def/waterml/2.0/processType/Sensor"},
                "featureOfInterest": {"href":"http://waterml2.csiro.au/rgs-api/v1/monitoring-point/419009/"},
                "resultTime": "2015-08-05T09:00:00+10:00",
                "result": {
                    "metadata": { },
                    "defaultPointMetadata": {
                        "interpolationType": { "href": "http://www.opengis.net/def/waterml/2.0/interpolationType/Continuous" },
                        "quality": { "href": "http://www.opengis.net/def/waterml/2.0/quality/unchecked" },
                        "uom" : "http://qudt.org/vocab/unit#DegreeCelsius"
                    },
                    "points": [
                        { "time":  { "instant": "2015-08-03" }, "value": 3.2 },
                        { "time":  { "instant": "2015-08-04" }, "value": 3.5 },
                        { "time":  { "instant": "2015-08-05" }, "value": 3.6 }
                    ]
                }
            }
        ]
    }

```
