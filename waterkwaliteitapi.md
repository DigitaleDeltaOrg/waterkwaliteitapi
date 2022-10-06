# Waterkwaliteit API - Voorstel

De Waterkwaliteit API is een voorloper van de 1-API strategie van de Digitale Delta.
Deze API moet het mogelijk maken om data op te halen via OData, maar ook data in de vorm van een Digitale Delta-formaat, aan te leveren. Niet alle export formaten zullen als import formaat worden geaccepteerd.
Het aanleveren zal gaan in bulk-vorm, waarbij meerdere metingen of aan metingen gerelateerde entiteiten als een enkele set wordt aangeboden en verwerkt.

Naast metingen, observaties, monsters en tijdseries, zal deze API zich ook richten op meetlocaties/objecten, omdat deze cruciaal voor de context zijn.

Om de import- en exportbestanden compact te houden, wordt gebruik gemaakt van JSON Pointers.

Het te maken Definitieboek zal leidend zijn met betrekking tot de data die **moet** en _kan_ worden uitgewisseld.

## Standaarden

Zoveel mogelijk worden standaards toegepast waaraan we ons hebben gecomitteerd:
 - [OGC O&M](https://www.ogc.org/standards/om)
 - [JSON](https://www.json.org/json-en.html)
 - [GeoJSON](https://geojson.org/)
 - [IM Metingen](https://www.aquo.nl/index.php/IM_Metingen)
 - [OData](https://www.odata.org/)
 - [OAUTH2](https://oauth.net/2/)/[OpenID Connect](https://openid.net/connect/)
 - [REST](https://en.wikipedia.org/wiki/Representational_state_transfer)
 - [Kennisplatform APIs](https://www.geonovum.nl/themas/kennisplatform-apis)
 - [EPSG voor geografische datums (Coordinate Reference System, CRS)](https://epsg.io)
 - [JSON for Linking Data](https://json-ld.org/)
 - [JSON Pointers](https://www.rfc-editor.org/rfc/rfc6901.html)

 OGC O&M gaat boven de Aquo-specifieke toevoegingen aan IM Metingen.
 Het pure OGC O&M formaat zal niet verder worden beschreven.

## Definitieboek

Er wordt een Definitieboek aangelegd. Hierin worden definities vastgelegd, zoals: welke attributen kunnen voorkomen in een meting, en wat is de definitie ervan.
Ook exportformaten zullen hier worden vastgelegd.

## Functionale eisen

De functionele eisen kunnen worden onderverdeeld in de volgende hoofdstukken:

### Algemeen

Een aantal algemene principes worden als volgt gespecificeerd:

#### GeoJSON 

Er wordt geografische data overgedragen in de vorm van GeoJSON binnen de API.  In het verleden bezat GeoJSON een aanduiding voor het geografisch stelsen waarin de data was geprojecteerd. Echter is die definitie verwijderd uit de standaard, wegens interpretatieverschillen.
Daarom wordt hier Kennisplatform APIs aangehouden. Daar is de volgende oplossing bedacht:
- De client kan aangeven in de Content-Crs header om welke CRS het gaat in het bestand.  De waarde hiervan wordt uitgedrukt als EPSG:xxxxx, waarbij xxxxx de vier- of vijfcijferige [EPSG](epsg.io) code is.
- Wanneer de server niet om kan gaan met de CRS, dan kan de server een BAD REQUEST teruggeven, met daarin een Accept-Crs header met als data de CRS'en die de server wel accepteerd. Dit wordt beschouwd als een validatiefout.

Het effect is dat het opgegeven CRS geldt voor alle GeoJSON binnen de download dan wel upload.
De server dient minimaal de volgende CRS'en te ondersteunen: EPSG:4326 (WGS84), EPSG:28992 (Amersfoort/RD New), ESPG:4258 (ETRS89). Het wordt aangeraden om ook de specifieke EPSG's voor onze regio te gebruiken (de 31n varianten).

Voor conversie kan RDNAPTRANS of een omgeving-specifieke Proj-library worden gebruikt. 
*De Proj-libraries zijn eenvoudiger in gebruik binnen APIs dan RDNAPTRANS*

#### Hergebruik (linked data)

Om het data volume zo klein mogelijk te houden, wordt gebruikt gemaakt van refenties, gebaseerd op [JSON for Linking Data](https://json-ld.org/). 
Boven in de data (zowel export als import) wordt een referentietabel gespecificeerd. Die bevat alle in de dataset gebruikte relevante entiteiten, bijvoorbeeld grootheden, eenheden, parameters, meetobjecten, etc.

In de data wordt gerefereerd aan die referentietabel via zogenaamde [JSON Pointers](https://www.rfc-editor.org/rfc/rfc6901.html). Dat is in wezen niet meer dan een URL die kan linken naar een intern onderdeel van de data, of een extern onderdeel.
Voor de Waterkwaliteit-API (en de 1-API) zullen de referenties intern in het document zitten. De reden daarvoor is leesbaarheid. Binnen de referentietabel kan wel naar een externe bron worden verwezen, zoals Aquo.

Een referentiesysteem als dit is al een aantal keren besproken in de DD-API groep, maar er is nooit een klap opgegeven.
De hier beschreven methode voldoet meer aan de omarmde standaards.

_Uitzoeken: Is dit voldoende voor NEN3610: Linked Data?_


#### JSON

Om de data volume klein te houden, **moeten** client en server beide gebruik maken van data compressie, en wel Brotli compressie. De meeste ontwikkel-frameworks en alle browsers beschikken daarover. Dit zal resulteren in een gemiddelde compressie van 15-21%.

### Opvragen: Filteren/selecteren

Opvragen van de data zal gebeuren via OData. 
De voor OData benodige CSDL zal worden gegenereerd vanuit het Definitieboek van de Digitale Delta.
Beveiliging van het zoeken zal gebeuren via OpenID Connect voor interactieve toepassingen en OAUTH2 Machine-to-machine voor machine-koppelingen.
De client moet kunnen selecteren welke eigenschappen terug worden gestuurd in de response.

De volgende OData features worden ge&iuml;mplementeerd:
- $filter: eq, ne, gt, ge, lt, le, contains, startswith, endswith, year, month, day, hour, minute, second, and, or, not, 'geo.intersects', 'geo.distance'
- $select
- $skip
- $top
- $count: wanneer True, dan wordt totalObjectCount in het paging-blok gevuld.
- Data types: Boolean, Int16/32/64, Double, Decimal(p, s), Guid, String, Date, DateTime, DateTimeOffset, Time, TimeOfDay, Duration, Geography (GeoJSON)

De $expand en $search laten we in deze versie buiten beschouwing.

De uitvoerformaten (behalve OGC O&M) worden, net als alle attributen, vastgelegd in het Definitieboek. In ieder geval wordt het offici&euml;le OGC O&M ge&euml;xporteerd.
Mogelijk wordt een additioneel formaat, welke geschikter is voor timeseries en observaties voor het importere gedefinieerd.

Vanuit het Definitieboek zal de CSDL voor OData worden gegenereerd via een REST webservice.
De OpenAPI Specification kan worden via een merge actie worden gegenereerd via een REST webservice.

_Uitzoeken: Is $expand nodig?_
_Uitzoeken: Is $count nodig?_

Operaties om data toe te voegen, wijzigen of verwijderen worden **niet** ondersteund.

### Aanbieden: Bulk verwerking

Bulk-verwerking is niet iets dat ingebakken is in REST. Wegens volume van de data en de verwerkingstijd, is gekozen voor een asynchrone oplossing.
Toevoegen en verwijdere is toegestaan. 
Updaten is *niet* toegestaan. Dit om een aantal redenen: 
- Metingen behoren immutable te zijn
- Updaten van data, helemaal indien deze in een bepaalde structuur staan, is zeer inefficient en kan vaak niet in een bulk-operatie plaatsvinden.
- Ook de historie bijhouden wordt ingewikkeld.

Moeten metingen worden gemuteerd, dan dienen deze eerst verwijderd te worden, en daarna opnieuw ge&iuml;mporteerd.

Authentiseren gaat met de OAUTH2 Client Credential Flow, die wordt gebruikt als machine-to-machine koppeling.
De API implementatie dient het inlog-account te associeren met een bepaalde organisatie en op basis daarvan te valideren of het verzoek terecht is.

*Data aanleveren moet laagdrempelig gebeuren om draagvlak te krijgen en behouden.*
Mogelijk is het IM Metingen formaat of het O&M formaat te complex en leidt tot een groter datavolume.
We kunnen kijken naar vereenvoudigde formaten: een timeseries formaat en een monster-gebaseerd formaat. Voor timeseries zou het bekende DD-API formaat, met kleine aanpassingen, kunnen worden gebruikt. Voor de andere metingen kunnen we een formaat distileren die dichter staat bij O&M.

Verwerking zal niet meteen gebeuren. Daarom kan de client een status bij de server opvragen.

Valideren is de eerste fase van verwerking. Pas wanneer de bulk-content volledig gevalideerd is, kan er daadwerkelijk worden ge&iuml;mporteerd.

__TODO: gestandaardiseerd formaat voor foutrapportage__

#### Bulk data valideren/toevoegen
[![Valideren/toevoegen](https://mermaid.ink/img/pako:eNqNkdFKw0AQRX9l2CeF9gfyUGhNxSIk0BhBycu4O02WJLNxMylq6b-7shYEaek8Xe7cOTMwB6WdIZWokd4nYk2pxdpjXzGEGtCL1XZAFrjrLLH89wvye_LRj5n5YhHNBN6mrgXLI_nfycwJgbd1I-B2cIo9Y2cNhRDoP1tiN9AiNoEyW5ZPD_l287pOr8N50o0Qn-Hd59vVJk3X2SXYsoMPCOrLUUsMiFzT3iPW5gx1VRYvF6-LLBBHowTcGUz-eCMeeUQt1vHcmtuK1Uz15Hu0Jnzs8DNYKWmop0olQRr0baUqPoYcTuKKT9YqET_RTE2DQTl9VyU77EY6fgOcsKye)](https://mermaid.live/edit#pako:eNqNkdFKw0AQRX9l2CeF9gfyUGhNxSIk0BhBycu4O02WJLNxMylq6b-7shYEaek8Xe7cOTMwB6WdIZWokd4nYk2pxdpjXzGEGtCL1XZAFrjrLLH89wvye_LRj5n5YhHNBN6mrgXLI_nfycwJgbd1I-B2cIo9Y2cNhRDoP1tiN9AiNoEyW5ZPD_l287pOr8N50o0Qn-Hd59vVJk3X2SXYsoMPCOrLUUsMiFzT3iPW5gx1VRYvF6-LLBBHowTcGUz-eCMeeUQt1vHcmtuK1Uz15Hu0Jnzs8DNYKWmop0olQRr0baUqPoYcTuKKT9YqET_RTE2DQTl9VyU77EY6fgOcsKye)

#### Status transactie opvragen
[![Status transactie opvragen](https://mermaid.ink/img/pako:eNqNklFPgzAQx7_KpU-abF-AhyVMtrgsQhyg0fBya2_QDFoshajLvrtFXGI0oH26XO9-_7t_7sS4FsQ81tBLS4pTIDE3WGUK3KvRWMlljcrCTSlJ2d_5mExHZsgPNfPFYkh60Fi0bQO67gzmpOCqIgvWoGqQW6nVXIrroTXUlsDIvLCgD3Dpf8BSCiJjgX-TH36dzKDnQRr6aXIb7TbPq-B_OEO8sKRGeOtot9wEwSqcgvklvIKL3jUd3WqIKie3JuZihJpEEdz54dMUNFL7niZ-mDS69zaMHienTC4cAqlgTwUqQaVU-QhymcaTA65163zrWV1vJjrwGMkPYLe6T1dxcuWc-bJeELgmhP4QDp-svw-gF4Gm5ZyaTpcjctE2U2zGKjIVSuEu-tQXZswWVFHGPBcKNMeMZers6rC1On5TnHnWtDRjbe1kLtfPvAOWDZ0_AH6UDdE)](https://mermaid.live/edit#pako:eNqNklFPgzAQx7_KpU-abF-AhyVMtrgsQhyg0fBya2_QDFoshajLvrtFXGI0oH26XO9-_7t_7sS4FsQ81tBLS4pTIDE3WGUK3KvRWMlljcrCTSlJ2d_5mExHZsgPNfPFYkh60Fi0bQO67gzmpOCqIgvWoGqQW6nVXIrroTXUlsDIvLCgD3Dpf8BSCiJjgX-TH36dzKDnQRr6aXIb7TbPq-B_OEO8sKRGeOtot9wEwSqcgvklvIKL3jUd3WqIKie3JuZihJpEEdz54dMUNFL7niZ-mDS69zaMHienTC4cAqlgTwUqQaVU-QhymcaTA65163zrWV1vJjrwGMkPYLe6T1dxcuWc-bJeELgmhP4QDp-svw-gF4Gm5ZyaTpcjctE2U2zGKjIVSuEu-tQXZswWVFHGPBcKNMeMZers6rC1On5TnHnWtDRjbe1kLtfPvAOWDZ0_AH6UDdE)

#### Bulk verwijderen
[![Bulk verwijderen](https://mermaid.ink/img/pako:eNqNkt1ugzAMhV_FytUmtS_ARSU6qFZVA63Apk3cuIkLWSF0IbCfqu--MIY0qYItV5Zz8h3b8YnxShBzWE2vDSlOnsRMY5kqsOeI2kguj6gM3BSSlLnMR6Rb0n2-18wXiz7pwK4pDmCjN_kiSJPqZUFlCLTMcgPVHgbtAxZSEGkD_JdVf2uRPduBJHCT-Dbcrp997384TTw3g_cFbxVul2vP84MpmFvAe9fHZ0UHUoCoMmo1YiZGqHEYwp0bPE1BQ7XraAKMRlUjN7JScylG-94E4eNklfHAIZAKdpSjElRIlY0gl0k0WeCqauzcOlbbDRMteIzkerD17xM_iq_sZH5GLwjsI4SSDOy_Wdd__lhnAnXDOdVtVYzYhZtUsRkrSZcohd3eUydMmcmppJQ5NhSoDylL1dnqsDFV9KE4c4xuaMaao7UZNp05eyxqOn8BddoGpw)](https://mermaid.live/edit#pako:eNqNkt1ugzAMhV_FytUmtS_ARSU6qFZVA63Apk3cuIkLWSF0IbCfqu--MIY0qYItV5Zz8h3b8YnxShBzWE2vDSlOnsRMY5kqsOeI2kguj6gM3BSSlLnMR6Rb0n2-18wXiz7pwK4pDmCjN_kiSJPqZUFlCLTMcgPVHgbtAxZSEGkD_JdVf2uRPduBJHCT-Dbcrp997384TTw3g_cFbxVul2vP84MpmFvAe9fHZ0UHUoCoMmo1YiZGqHEYwp0bPE1BQ7XraAKMRlUjN7JScylG-94E4eNklfHAIZAKdpSjElRIlY0gl0k0WeCqauzcOlbbDRMteIzkerD17xM_iq_sZH5GLwjsI4SSDOy_Wdd__lhnAnXDOdVtVYzYhZtUsRkrSZcohd3eUydMmcmppJQ5NhSoDylL1dnqsDFV9KE4c4xuaMaao7UZNp05eyxqOn8BddoGpw)

### Historie opvragen

Om transparant te zijn, moeten alle **acties** worden gelogged. De aanvrager kan ten allen tijde de history opvragen.

[![Historie opvragen](https://mermaid.ink/img/pako:eNqNkd1qwzAMhV_F-GqD9gV8UciWjpWxGPoz2PCNsNXEq2NnilO2lb77PExgY6RMV-Lo6BOSTlwHg1zwHt8G9BpLCzVBqzxL0QFFq20HPrJbZ9HHv_oG6YiU9eyZLxZZFKyxfQxkMZerEJGRrZvIwp6Nnidw1iBSZPrHiFxNqMwUbFcVu-29XK9eluX_cIS6iegneHdyfbMqy2V1CVY49s5S9hnwgJ4B-BqPBFCbCepWSvZYVM-XoLJrwCXaeJy5s6_91Nry4eq371p5PuMtUgvWpMedvhsVjw22qLhIqQE6KK78OflgiGHz4TUXkQac8aEzEMcnc7EH1-P5C00Nrvw)](https://mermaid.live/edit#pako:eNqNkd1qwzAMhV_F-GqD9gV8UciWjpWxGPoz2PCNsNXEq2NnilO2lb77PExgY6RMV-Lo6BOSTlwHg1zwHt8G9BpLCzVBqzxL0QFFq20HPrJbZ9HHv_oG6YiU9eyZLxZZFKyxfQxkMZerEJGRrZvIwp6Nnidw1iBSZPrHiFxNqMwUbFcVu-29XK9eluX_cIS6iegneHdyfbMqy2V1CVY49s5S9hnwgJ4B-BqPBFCbCepWSvZYVM-XoLJrwCXaeJy5s6_91Nry4eq371p5PuMtUgvWpMedvhsVjw22qLhIqQE6KK78OflgiGHz4TUXkQac8aEzEMcnc7EH1-P5C00Nrvw)

__TODO: history log-formaat__

## To do's/gedachten...

### Importformaat

Een dergelijke aanpak is reeds een aantal keren besproken in de DD-API community. 

De JSON Pointers verwijzen naar het lokale document. De reden daarvoor: hierdoor blijven de codes herkenbaar voor mensen. /reference/compartment/OW is leesbaarder dan https://www.aquo.nl/index.php/Id-ebfe5456-9b14-4925-b444-0b5c98b6642a. Aangezien de referentietabel slechts &#xE9&#xE9nmaal wordt opgenomen, is de overhead minimaal.

De referenties zelf kunnen in [JSON-LD](https://json-ld.org/) worden uitgedrukt.

#### Timeseries-import formaat

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
            "member": []
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

### Definitieboek

- Moet gevuld worden. 
- Nederlandse termen moeten worden vervangen door Engelse termen.
- Ook kijken wat weg kan. 

Redenen: 
     
- IM Metingen is mogelijk te complex geworden door een definitie-verschil (semantisch?) met OGC O&M. Er zijn veel Nederlandse toevoegingen die mogelijk overbodig zijn en via VocabTerms kan worden opgelost. Dit wordt onderzocht. 

Zo te zien is dit het interpretatie-probleem:
- Observation wordt in IM Metingen opgevat als zijnde een meting, waar verschillende eigenschappen aan gekoppeld kunnen worden.
- Uit analyse van het JSON schema, welke genoemd wordt in de specificatie, blijkt dat een Observation geen meting is, maar een (reeks van) gemeten waarde(n): een meet*waarde*, dus. Deze worden bijeengehouden door een ObservationCollection. Een Observation kan dus verschillende meet*waarden* hebben, die kunnen afwijken in type.
- De afzonderlijke eigenschappen die in IM Metingen worden vastgelegd bij een enkele meting, zouden als VocabTerms moeten worden vastlegd in een separate 'meting'. Bijvoorbeeld lengteklasse zou een afzonderlijke Observation zijn, uitgedrukt in een VocabTerm. VocabTerms kunnen referenties zijn naar een andere bron, dus in dit geval de lijst lengteklasse van Aquo. Het bepalen van de specifieke entiteiten in IM Metingen zijn daarmee overbodig geworden. Wat de meetwaarden gemeen hebben, is het Id van de ObservationCollection.


### Terminologie

Indien bovenstaande analyse juist is, dan heeft dat flinke inpact op de te gebruiken terminologie en daarmee het Definitieboek.

Wat wij nu metingen noemen, zijn ObservationCollections. De eigenschappen zijn Observations, in wezen de waarden die behoren bij de meting.

### Interpretatie en mapping

- Sample -> Monster
- ObservationCollection

