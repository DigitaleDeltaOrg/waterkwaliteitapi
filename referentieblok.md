# Referentieblok

Het referentieblok is bedoeld om:

- herhaling van data in JSON te voorkomen
- de JSON leesbaarder te houden dan wanneer alleen met Ids wordt gewerkt

Dit is diverse keren besproken in de Digitale Delta gremia, maar er is niet definitie iets vastgesteld. Daarom neemt de Waterkwaliteit-API het voortouw.

Er wordt gebruik gemaakt van [JSON-LD](https://json-ld.org/).

Het referentieblok ziet er als volgt uit (eigenschappen beschreven in JSON-LD worden geprefixed door een '@'):

``` json
    "reference": {
        "measurementobject": [{
            "@id": "NL12-VEEN",
            "code": "NL12-VEEN",
            "@type": "NamedGeoObject",
            "geo": {
                "latitude": "40.75",
                "longitude": "73.98"
            },
            "name": "Veen"
        }],
        "uom": [{
            "@context": [{"href": "https://www.aquo.nl/index.php/Id-315b7646-3b9d-45c4-a387-d0ebb8d6c04b"}],
            "@id": "Id-315b7646-3b9d-45c4-a387-d0ebb8d6c04b",
            "code": "kuub_uur",
            "@type": "Units",
            "name": "kubieke meter per uur"
        }],
        "quantity": [{
            "@id": "Id-01b89ecb-6f63-4463-b3c7-d9a712d7ed1e",
            "code": "Debiet",
            "context": [{"href": "https://www.aquo.nl/index.php/Id-01b89ecb-6f63-4463-b3c7-d9a712d7ed1e"}]
        }],
        "source": [{
            "@id": "2",
            "code": "RWS",
            "name": "Rijkswaterstaat"
        }],
        "compartment": [{
            "@id": "Id-ebfe5456-9b14-4925-b444-0b5c98b6642a",
            "@context": [{"href": "https://www.aquo.nl/index.php/Id-ebfe5456-9b14-4925-b444-0b5c98b6642a"}],
            "code": "OW",
            "name": "oppervlaktewater"
        }]
    }
```

Referen naar een bepaald item in de reference gaat dan met:
```/reference/measurementobject/NL12-VEEN``` of ```/reference/compartment/OW```

Het referentieblok komt meteen na het pagingblok, dus vóór het resultaatblok.
