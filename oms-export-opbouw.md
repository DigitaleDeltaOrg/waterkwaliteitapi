# OMS export opbouw

Voor de Waterkwaliteit-API wordt álleen het collectie-uitvoerformaat gebruikt. Daarom wordt er bij een export altijd een paginablok weergegeven
Een OMS export in JSON formaat bestaat uit drie onderdelen:

Het [paginablok](paginablok.md) zal zoveel mogelijk voldoen aan het OData formaat.
Het [referentieblok](referentieblok.md) zal alleen referenties bevatten.
Het [resultaatblok](resultaatblok.md) zal de meting-data bevatten.

Een voorbeeld van een output:

``` json
{
    "@context": "",
    "@nextLink" "",
    "@count": "",
    "r": {
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
    "specimen": [
        { 
            "id": "",
            "type": "",
            "sampledFeature": "",
            "member": [ { "href": "/r/obs/123123-1" },  "href": "/r/obs/123123-2" } ]
        }
    ],
    "observations": [
        { 
            "id": "123123",
            "measurementobject": "/r/measurementobject/NL12-VEEN",
            "quantity": "/r/quantity/AANTPVLME",
            "parameter": "/r/parameter/Surirella",
            "uom": "/r/uom/DIMSLS",
            "compartment": "/r/compartment/OW",
            "source": "/r/source/1",
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
            "measurementobject": "/r/measurementobject/NL12-VEEN",
            "quantity": "/r/quantity/AANTPVLME",
            "parameter": "/r/parameter/Surirella",
            "uom": "/r/uom/DIMSLS",
            "compartment": "/r/compartment/OW",
            "source": "/r/source/1",
            "analysisTime": "2010-12-12T15:15:00Z",
            "startTime": "2010-12-12T16:00:00Z", 
            "endTime": "2010-12-12T18:00:00Z",
            "observation":
            [
                { "id": "123124-1", "value": 234 },
                { "id": "123124-2", "reference": "/r/stageoflife/JU" }
            ]
        }   
    ]
}

```
