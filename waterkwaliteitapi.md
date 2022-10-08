# Waterkwaliteit API - Voorstel

## Status

Dit document is momenteel in voorstel-fase. Er zal de komende tijd nog flink aan gesleuteld worden.

## TODO

- [Definitieboek](definitieboek.md)
- [OMS JSON definitie](oms-json.md)
- [Definitie van log-berichten](logging.md)
- [Standaardisatie van HTTP fouten](http-fouten.md)
- [Definitie van gestandaardiseerde validatiefouten als antwoord op bulk-verwerking](validatie.md)
- [OData voorbeelden voor OMS](odata-en-oms.md)
- Open API Specification

## Samenvatting

- Waterkwaliteit API is een voorloper van de 1-API strategie van de Digitale Delta
- Zowel [opvragen van data (OData)](filteren-selecteren.md) als aanbieden van data
- Aanbieden van data gaat asynchroon
- OGC O&M 1956:2022 is het primaire import/export formaat
- JSON is het primaire transport formaat, [maar vereist restricties](omgaan-met-data.md).
- [Definitieboek](definitieboek.md) gaat bepalen welke onderdelen optioneel dan wel verplicht zijn voor data
- [Authenticatie gaat via de OAUTH2 protocollen Authorization Flow voor interactieve zaken (opvragen) en Client Credentials Flow bij machine-to-machine (aanbieden)](beveiliging.md)

## Introductie

De Waterkwaliteit API is een voorloper van de 1-API strategie van de Digitale Delta.
Deze API moet het mogelijk maken om data op te halen via [OData](https://odata.org), maar ook data in de vorm van een Digitale Delta-formaat, aan te leveren. Niet alle export formaten zullen als import formaat worden geaccepteerd.
Het aanleveren zal gaan in [bulk-vorm](bulkverwerking.md), waarbij meerdere metingen of aan metingen gerelateerde entiteiten als een enkele set wordt aangeboden en verwerkt.

Naast metingen, observaties, monsters en tijdseries, zal deze API zich ook richten op meetlocaties/objecten, omdat deze cruciaal voor de context zijn. Er is echter een manier om beiden tezamen te houden in een enkel bestand.

Het [Definitieboek](definitieboek.md) zal leidend zijn met betrekking tot de data die **moet** en _kan_ worden uitgewisseld.

## Functionele eisen

De functionele eisen zijn [hier](functionele-eisen.md) beschreven.
