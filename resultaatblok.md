# Resultaatblok

Het formaat van de output zal afhankelijk zijn van het opgegeven export formaat (```$reportFormat```).
De volgende formaten zijn in ieder geval aanwezig:

- observationcollections: de 'root' is altijd ```ObservationCollection```. Daarbinnen staan de ```Observation```s.
- samples: de 'root' is altijd Sample. Daarbinnen staan de Observations.
- observations: de root is een ```Observation```. Aan ```Sample```s en ```ObservationCollection```s wordt gerefereerd.

## samples

Het volgende fictief voorbeeld is een ```Sample```s en met meerdere ```Observation```s.

``` json
"samples": [
    { 
        "id": "<uniquesampleid>",
        "samplingLocation": "",
        "samplingMethod": "",
        "samplingTime": "",
        "sampledFeature": "",
        "member": [
            {
                "id": "<uniqueid1>-01",
                "type": "CountObservation",
                "featureOfInterest": { "href": "/references/measurementobjects/VEEN" },
                "phenomenonTime": { "datetime": "2015-05-11"},
                "observedProperty": { "href": "/references/quantities/AANTL" },
                "procedure": { "href": "/references/methods/MACEV_S530" },
                "samplingStrategy": { "href": "/references/strategy/random" } ,
                "resultTime":  { "datetime": "2015-05-11"},
                "parameter": [ "/references/parameters/Abra_alba" ],
                "resultQuality": {},
                "validTime":  { "datetime": "2015-05-11"},
                "result": 97
            },
            {
                "id": "<uniqueid1>-02",
                "type": "TruthObservation",
                "procedure": { "href": "http://www.opengis.net/def/waterml/2.0/processType/Manual" },
                "featureOfInterest": { "href": "/references/measurementobjects/VEEN" },
                "resultTime":  { "datetime": "2015-05-11"},
                "result": true  
            },
            {
                "id":"<uniqueid1>-03",
                "type": "CategoryObservation",
                "phenomenonTime":  { "datetime": "2015-05-11"},
                "observedProperty": { "href": "http://environment.data.gov.au/def/property/taxon" },
                "procedure": { "href":"#/references/methods/MACEV_S530" },
                "featureOfInterest": { "href": "/references/measurementobjects/VEEN"},
                "resultTime":  { "datetime": "2015-05-11"},
                "result": {
                    "term": "LS-AD",
                    "vocabulary": "https://www.aquo.nl/index.php/Id-01bfddaf-2f04-44ef-817d-2312f924a85d"
                }
            }
        ] 
    }
]
    

```

## observationcollections

Het volgende fictief voorbeeld is een ```ObservationCollection```s en met meerdere ```Observation```s.

``` json
"observationcollections": [
    { 
        "id": "<uniqueobservationcollectionid>",
        "member": [
        { 
            "id": "<uniqueid1>", 
            "type": "summerize", 
            "member": [
                {
                    "id": "<uniqueid1>-01",
                    "type": "CountObservation",
                    "featureOfInterest": { "href": "/references/measurementobjects/VEEN" },
                    "phenomenonTime": { "datetime": "2015-05-11"},
                    "observedProperty": { "href": "/references/quantities/AANTL" },
                    "procedure": { "href": "/references/methods/MACEV_S530" },
                    "samplingStrategy": { "href": "/references/strategy/random" } ,
                    "resultTime":  { "datetime": "2015-05-11"},
                    "parameter": [ "/references/parameters/Abra_alba" ],
                    "resultQuality": {},
                    "validTime":  { "datetime": "2015-05-11"},
                    "result": 97
                },
                {
                    "id": "<uniqueid1>-02",
                    "type": "TruthObservation",
                    "procedure": { "href": "http://www.opengis.net/def/waterml/2.0/processType/Manual" },
                    "featureOfInterest": { "href": "/references/measurementobjects/VEEN" },
                    "resultTime":  { "datetime": "2015-05-11"},
                    "result": true  
                },
                {
                    "id":"<uniqueid1>-03",
                    "type": "CategoryObservation",
                    "phenomenonTime":  { "datetime": "2015-05-11"},
                    "observedProperty": { "href": "http://environment.data.gov.au/def/property/taxon" },
                    "procedure": { "href":"#/references/methods/MACEV_S530" },
                    "featureOfInterest": { "href": "/references/measurementobjects/VEEN"},
                    "resultTime":  { "datetime": "2015-05-11"},
                    "result": {
                        "term": "LS-AD",
                        "vocabulary": "https://www.aquo.nl/index.php/Id-01bfddaf-2f04-44ef-817d-2312f924a85d"
                    }
                }
            ] 
        }]
    }
]
```

## observations

Het volgende fictief voorbeeld is een ```ObservationCollection```s en met meerdere ```Observation```s.
Via context wordt gerefereerd aan Samples en ObservationCollections.
In dit formaat worden observations, samples en observationcollections als separate entiteiten meegegeven en wordt ernaar gelinkt.

``` json
{ 
    "observations": [
        {
            "id": "<uniqueid1>-01",
            "type": "CountObservation",
            "featureOfInterest": { "href": "/references/measurementobjects/VEEN" },
            "phenomenonTime": { "datetime": "2015-05-11"},
            "observedProperty": { "href": "/references/quantities/AANTL" },
            "procedure": { "href": "/references/methods/MACEV_S530" },
            "samplingStrategy": { "href": "/references/strategy/random" } ,
            "resultTime":  { "datetime": "2015-05-11"},
            "parameter": [ "/references/parameters/Abra_alba" ],
            "resultQuality": {},
            "validTime":  { "datetime": "2015-05-11"},
            "result": 97,
            "context": [ "/observationcollections/observationcollectionid-1", "/samples/sampleid-2"]
        },
        {
            "id": "<uniqueid1>-02",
            "type": "TruthObservation",
            "procedure": { "href": "http://www.opengis.net/def/waterml/2.0/processType/Manual" },
            "featureOfInterest": { "href": "/references/measurementobjects/VEEN" },
            "resultTime":  { "datetime": "2015-05-11"},
            "result": true,
            "context": []  
        },
        {
            "id":"<uniqueid1>-03",
            "type": "CategoryObservation",
            "phenomenonTime":  { "datetime": "2015-05-11"},
            "observedProperty": { "href": "http://environment.data.gov.au/def/property/taxon" },
            "procedure": { "href":"#/references/methods/MACEV_S530" },
            "featureOfInterest": { "href": "/references/measurementobjects/VEEN"},
            "resultTime":  { "datetime": "2015-05-11"},
            "result": {
                "term": "LS-AD",
                "vocabulary": "https://www.aquo.nl/index.php/Id-01bfddaf-2f04-44ef-817d-2312f924a85d"
            },
            "context": []
        }
    ],
    "observationcollections": [],
    "samples": [] 
}
```
