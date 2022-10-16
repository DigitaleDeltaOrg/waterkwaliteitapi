# Referentieblok

Het referentieblok is bedoeld om:

- herhaling van data in JSON te voorkomen
- de JSON leesbaarder te houden dan wanneer alleen met Ids wordt gewerkt

Dit is diverse keren besproken in de Digitale Delta gremia, maar er is niet definitie iets vastgesteld. Daarom neemt de Waterkwaliteit-API het voortouw.

Er wordt gebruik gemaakt van [JSON-LD](https://json-ld.org/).

Een voorbeeld van het referentieblok staat [hier](voorbeelden/referentieblok.json).

Referen naar een bepaald item in de reference gaat dan met:
```/reference/measurementobject/NL12-VEEN``` of ```/reference/compartments/OW```

Het referentieblok komt meteen na het pagingblok, dus vóór het resultaatblok.
