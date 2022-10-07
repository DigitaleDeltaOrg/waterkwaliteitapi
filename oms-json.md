# OMS en JSON

OMS is groot, en JSON is breedsprakig. Een reden daarvoor is dat in de bestaande DD-API's telkens alle elementen werden herhaald, bijvoorbeeld meetobject/meetlocatie.
Om dit te voorkomen werkt de Waterkwaliteit met een referentiesysteem.
Een dergelijke aanpak is reeds een aantal keren besproken in de DD-API community, maar er was weinig voortgang geboekt.

Deze API introduceert een referentie systeem.
Hierbij wordt in het JSON document een onderdeel aangemaakt genaamd /reference.
Daarin worden alle entiteiten die worden gebruikt binnen het document eenmalig beschreven.
De data die daar gebruik van maakt, verwijst via [JSON Pointers](https://www.rfc-editor.org/rfc/rfc6901.html) naar de referentie.

Via de referentie kan weer worden verwezen naar URI's waar de standaard definitie is vastgelegd.
De reden hiervoor is leesbaarheid: /reference/compartment/OW is leesbaarder dan ```https://www.aquo.nl/index.php/Id-ebfe5456-9b14-4925-b444-0b5c98b6642a```.

Een voorbeeld:

``` json
"reference":
[
    "quantity":
    [
        {
        "@context": {
            "@vocab": "https://www.aquo.nl/index.php/Id-471174de-c041-2d35-103a-1e0f6f55bd87"
        },
        "@id": "https://www.aquo.nl/index.php/Id-01b89ecb-6f63-4463-b3c7-d9a712d7ed1e",
        "@type": "Grootheid",
        "name": "Debiet",
        "id": "DEBIET" 
        }
        ...
    ]
    ...
]
```

In bovenstaand voorbeeld kan data ernaartoe verwijzen via /reference/quantity/DEBIET.

## OMS JSON Schemas

[Geometrie (GeoJSON)](https://raw.githubusercontent.com/peterataylor/om-json/master/Geometry.json)
[Datum/tijd/intervallen](https://raw.githubusercontent.com/peterataylor/om-json/master/Temporal.json)
[Algemeen](https://raw.githubusercontent.com/peterataylor/om-json/master/Common.json)
[Generieke monsters](https://raw.githubusercontent.com/peterataylor/om-json/master/Sampling.json)
[Specimen (lab monsters)](https://raw.githubusercontent.com/peterataylor/om-json/master/Specimen.json)
[Ruimtelijke bemonstering (punt, oppervlak, curve)](https://raw.githubusercontent.com/peterataylor/om-json/master/SpatialSampling.json)
[Monstercollecties](https://raw.githubusercontent.com/peterataylor/om-json/master/SamplingCollection.json)
[Observaties](https://raw.githubusercontent.com/peterataylor/om-json/master/Observation.json)
[Tijdreeksen](https://raw.githubusercontent.com/peterataylor/om-json/master/TimeseriesTVP.json)
[Observatiecollecties](https://raw.githubusercontent.com/peterataylor/om-json/master/ObservationCollection.json)

## Additionele JSON Schemas ten behoeve van Waterkwaliteit-API

## Timeseries-import formaat

``` json
{
    "reference": {
        "measurementobject": [{
            "@id": "NL12-VEEN",
            "geo": {
                "latitude": "40.75",
                "longitude": "73.98"
            }
        }],
        "uom": [{
            "@context": [{"href": "https://www.aquo.nl/index.php/Id-315b7646-3b9d-45c4-a387-d0ebb8d6c04b"}],
            "@vocab": "",
            "@id": "kuub_uur",
            "@type": "",
            "name": "kubieke meter per uur"
        }],
        "quantity": [{
            "@id": "DEBIET",
            "name": "Debiet",
            "@context": [{"href": "https://www.aquo.nl/index.php/Id-01b89ecb-6f63-4463-b3c7-d9a712d7ed1e"}]
        }],
        "source": [{
            "@id": "2",
        }],
        "compartment": [{
            "@id": "OW",
            "@context": [{"href": "https://www.aquo.nl/index.php/Id-ebfe5456-9b14-4925-b444-0b5c98b6642a"}]
        }]
    },
    "observationcollection": [
        { 
            "id": "",
            "measurementobject": "/reference/measurementobject/NL12-VEEN",
            "quantity": "/reference/quantity/DEBIET",
            "uom": "/reference/uom/kuub_uur",
            "compartment": "/reference/compartment/OW",
            "source": "/reference/source/2",
            "analysisTime": "2010-12-12T15:15:00Z",
            "startTime": "2010-12-12T16:00:00Z", 
            "endTime": "2010-12-12T18:00:00Z",
            "realization": 13,
            "observation":
            [
                {
					"timeStamp": "2010-12-12T16:00:00Z",
					"value": 6.46
				},
				{
					"timeStamp": "2010-12-12T17:00:00Z",
					"value": 4.4
				},
				{
					"timeStamp": "2010-12-12T18:00:00Z",
					"value": 4.86
				}
            ]
        }
    ]
}

```


``` json
{
    "reference": {
        "measurementobject": [{
            "id": "NL12-VEEN",
            "geo": "<geojson>"
        }],
        "uom": [{
            "id": "DIMSLS",
            "name": "Dimensieloos",
            "href": "https://www.aquo.nl/index.php/Id-a25c25ec-89c3-47c2-81e0-278ac1f26aed"
        }],
        "quantity": [{
            "id": "AANTPVLME",
        }],
        "parameter": [{
            "id": "Surirella",
        }],
        "source": [{
            "id": "1",
        }],
        "compartment": [{
            "id": "OW",
            "href": "https://www.aquo.nl/index.php/Id-ebfe5456-9b14-4925-b444-0b5c98b6642a"
        }],
        "stageoflife": [
            {
                "id": "AD",
                "name": "Adult",
                "href": "https://www.aquo.nl/index.php/Id-0322b79a-f9e9-42fb-898b-8318985c1de8#adult"
            },
            {
                "id": "JU",
                "name": "Juvenile",
                "href": "https://www.aquo.nl/index.php/Id-0322b79a-f9e9-42fb-898b-8318985c1de8#juvenile"
            }
        ],
    },
    "samplingCollection": [
        { 
            "id": "",
            "type": "",
            "sampledFeature": "",
            "member": [ {"<<observationCollection>>"}]
        }
    ],
    "observationcollection": [
        { 
            "id": "123123",
            "measurementobject": "/reference/measurementobject/NL12-VEEN",
            "quantity": "/reference/quantity/AANTPVLME",
            "parameter": "/reference/parameter/Surirella",
            "uom": "/reference/uom/DIMSLS",
            "compartment": "/reference/compartment/OW",
            "source": "/reference/source/1",
            "analysisTime": "2010-12-12T15:15:00Z",
            "startTime": "2010-12-12T16:00:00Z", 
            "endTime": "2010-12-12T18:00:00Z",
            "observation":
            [
                { "id": "123123-1", "value": 123 },
                { "id": "123123-2", "reference": "/reference/stageoflife/AD" }
            ]
        },
        {
            "id": "123124",
            "measurementobject": "/reference/measurementobject/NL12-VEEN",
            "quantity": "/reference/quantity/AANTPVLME",
            "parameter": "/reference/parameter/Surirella",
            "uom": "/reference/uom/DIMSLS",
            "compartment": "/reference/compartment/OW",
            "source": "/reference/source/1",
            "analysisTime": "2010-12-12T15:15:00Z",
            "startTime": "2010-12-12T16:00:00Z", 
            "endTime": "2010-12-12T18:00:00Z",
            "observation":
            [
                { "id": "123124-1", "value": 234 },
                { "id": "123124-2", "reference": "/reference/stageoflife/JU" }
            ]
        }   
    ]
}

```

Noot: voor meetobjecten en meetlocaties geldt [Geografie in bulk-scenario's](#geografie-in-bulk-scenarios).