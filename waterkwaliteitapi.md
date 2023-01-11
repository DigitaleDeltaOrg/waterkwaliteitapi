# Waterkwaliteit API

Dit document wordt gebruikt om door te kunnen linken naar specifieke onderdelen van de specificatie.

## Samenvatting

- Waterkwaliteit API is een voorloper van de 1-API strategie van de Digitale Delta, maar heeft een andere doelgroep
- Zowel [opvragen van data (OData)](filteren-selecteren.md) als [aanbieden van data](bulkverwerking.md) wordt ondersteund.
- Aanbieden van data gaat asynchroon, zodat wachttijden worden voorkomen.
- JSON is het primaire transport formaat, [maar vereist restricties](omgaan-met-data.md)
- [Authenticatie gaat via de OAUTH2 protocollen Authorization Flow voor interactieve zaken (opvragen) en Client Credentials Flow bij machine-to-machine (aanbieden)](beveiliging.md)

## Introductie

De Waterkwaliteit API is een voorloper van de 1-API strategie van de Digitale Delta. De doelgroep is echter anders: de basis is een JSON encoding van IM Metingen-CSV, om grote wijzigingen te voorkomen.
Deze API moet het mogelijk maken om data op te halen via [OData](https://odata.org), maar ook data (meetobjecten, monsters en metingen) aan te leveren.
Dat zal gaan in [bulk-vorm](bulkverwerking.md), waarbij meerdere metingen, monsters en/of meetobjecten als een enkele set wordt aangeboden en achteraf verwerkt.

## Functionele eisen

De functionele eisen zijn [hier](functionele-eisen.md) beschreven.

## Specificaties

De [edmx definitie](waterkwaliteit-api.edmx) kan gebruikt worden voor het aanmaken van de OData definitie.
De [yaml definitie](waterkwaliteit-api-oas.yaml) kan worden gebruikt voor het genereren van het skelet van de client- of server-kant van het systeem.
De [json definitie](waterkwaliteit-api.json) kan als basis worden gebruikt voor het valideren van het formaat van de imports/exports.
