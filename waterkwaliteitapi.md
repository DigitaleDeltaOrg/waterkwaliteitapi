# Waterkwaliteit API - Voorstel

De Waterkwaliteit API is een voorloper van de 1-API strategie van de Digitale Delta.
Deze API moet het mogelijk maken om data op te halen via OData, maar ook data in de vorm van een Digitale Deltaformaat, aan te leveren. Niet alle export formaten zullen als import formaat worden geaccepteerd.
Het aanleveren zal gaan in bulk-vorm, waarbij meerdere metingen of aan metingen gerelateerde entiteiten als een enkele set wordt aangeboden en verwerkt.

Naast metingen, observaties, monsters en tijdseries, zal deze API zich ook richten op meetlocaties/objecten, omdat deze cruciaal voor de context zijn.

## Standaarden

Zoveel mogelijk worden standaards toegepast waaraan we ons hebben gecomitteerd:
 - OGC O&M
 - JSON
 - GeoJSON
 - IM Metingen
 - OData
 - OAUTH2/OpenID Connect
 - REST
 - Kennisplatform APIs

 OGC O&M gaat boven de Aquo-specifieke toevoegingen aan IM Metingen.
 Het pure OGC O&M formaat zal niet verder worden beschreven.

## Functionale eisen

De functionele eisen kunnen worden onderverdeeld in de volgende hoofdstukken:

### Filteren/selecteren

Opvragen van de data zal gebeuren via OData. 
De voor OData benodige CSDL zal worden gegenereerd vanuit het Definitieboek van de Digitale Delta.
Beveiliging van het zoeken zal gebeuren via OpenID Connect voor interactieve toepassingen en OAUTH2 Machine-to-machine voor machine-koppelingen.
De client moet kunnen selecteren welke eigenschappen terug worden gestuurd in de response.

De volgende OData features worden ge&iuml;mplementeerd:
- $filter 
- $select
- $skip
- $top
- $count
- filterfuncties: 'geo.intersects', 'geo.distance'
- Data type: 'geography'

De $expand en $search laten we in deze versie buiten beschouwing. 

De uitvoerformaten worden, net als alle attributen, vastgelegd in het Definitieboek. In ieder geval wordt het offici&euml;le OGC O&M ge&euml;xporteerd.
Mogelijk wordt een additioneel formaat, welke geschikter is voor timeseries en observaties voor het importere gedefinieerd.

Vanuit het Definitieboek zal de CSDL voor OData worden gegenereerd via een REST webservice.
De OpenAPI Specification kan worden via een merge actie worden gegenereerd via een REST webservice.

### Bulk verwerking

Bulk-verwerking is niet iets dat ingebakken is in REST. Wegens volume van de data en de verwerkingstijd, is gekozen voor een asynchrone oplossing.
Toevoegen is toegestaan. Updaten is *niet* toegestaan. Dit om een aantal redenen: 
- Metingen behoren immutable te zijn
- Updaten van data, helemaal indien deze in een bepaalde structuur staan, is zeer inefficient en kan vaak niet in een bulk-operatie plaatsvinden.
- Ook de historie bijhouden wordt ingewikkeld.
Verwijderen is wel toegestaan.

Moeten metingen worden gemuteerd, dan dienen deze eerst verwijderd te worden, en daarna opnieuw ge&iuml;mporteerd.
Authentiseren gaat met de OAUTH2 Client Credential Flow, die wordt gebruikt als machine-to-machine koppeling.
De API implementatie dient het inlog-account te associeren met een bepaalde organisatie en op basis daarvan te valideren of het verzoek terecht is.

#### Geografie in bulk-scenario's 

Geografie is een aandachtspunt in de bulk-verwerking. Alle items in een bulk-set moeten dezelfde CRS hebben. De client kan aangeven in de Content-Crs header om welke CRS het gaat in het bestand.
De waarde hiervan wordt uitgedrukt als EPSG:xxxxx, waarbij xxxxx de numerieke [EPSG](epsg.io) code is.

Wanneer de server niet om kan gaan met de CRS, dan kan de server een BAD REQUEST teruggeven, met daarin een Accept-Crs header met als data de CRS'en die de server wel accepteerd. Dit wordt beschouwd als een validatiefout.

De server dient minimaal de volgende CRS'en te ondersteunen: EPSG:4326 (WGS84), EPSG:28992 (Amersfoort/RD New), ESPG:4258 (ETRS89). Het wordt aangeraden om ook de specifieke EPSG's voor onze regio te gebruiken (de 31n varianten).

Voor conversie kan RDNAPTRANS of een omgeving-specifieke Proj-library worden gebruikt.

Client is in onderstaande schema's de aanroeper van de API. De server is de aangeroepene.

#### Verwerkingsproces

Verwerking zal niet meteen gebeuren. Daarom kan de client een status bij de server opvragen.

#### Valideren

##### Bulk data valideren/toevoegen
[![Valideren/toevoegen](https://mermaid.ink/img/pako:eNqNkdFKw0AQRX9l2CeF9gfyUGhNxSIk0BhBycu4O02WJLNxMylq6b-7shYEaek8Xe7cOTMwB6WdIZWokd4nYk2pxdpjXzGEGtCL1XZAFrjrLLH89wvye_LRj5n5YhHNBN6mrgXLI_nfycwJgbd1I-B2cIo9Y2cNhRDoP1tiN9AiNoEyW5ZPD_l287pOr8N50o0Qn-Hd59vVJk3X2SXYsoMPCOrLUUsMiFzT3iPW5gx1VRYvF6-LLBBHowTcGUz-eCMeeUQt1vHcmtuK1Uz15Hu0Jnzs8DNYKWmop0olQRr0baUqPoYcTuKKT9YqET_RTE2DQTl9VyU77EY6fgOcsKye)](https://mermaid.live/edit#pako:eNqNkdFKw0AQRX9l2CeF9gfyUGhNxSIk0BhBycu4O02WJLNxMylq6b-7shYEaek8Xe7cOTMwB6WdIZWokd4nYk2pxdpjXzGEGtCL1XZAFrjrLLH89wvye_LRj5n5YhHNBN6mrgXLI_nfycwJgbd1I-B2cIo9Y2cNhRDoP1tiN9AiNoEyW5ZPD_l287pOr8N50o0Qn-Hd59vVJk3X2SXYsoMPCOrLUUsMiFzT3iPW5gx1VRYvF6-LLBBHowTcGUz-eCMeeUQt1vHcmtuK1Uz15Hu0Jnzs8DNYKWmop0olQRr0baUqPoYcTuKKT9YqET_RTE2DQTl9VyU77EY6fgOcsKye)

##### Status transactie opvragen
[![Status transactie opvragen](https://mermaid.ink/img/pako:eNqNklFPgzAQx7_KpU-abF-AhyVMtrgsQhyg0fBya2_QDFoshajLvrtFXGI0oH26XO9-_7t_7sS4FsQ81tBLS4pTIDE3WGUK3KvRWMlljcrCTSlJ2d_5mExHZsgPNfPFYkh60Fi0bQO67gzmpOCqIgvWoGqQW6nVXIrroTXUlsDIvLCgD3Dpf8BSCiJjgX-TH36dzKDnQRr6aXIb7TbPq-B_OEO8sKRGeOtot9wEwSqcgvklvIKL3jUd3WqIKie3JuZihJpEEdz54dMUNFL7niZ-mDS69zaMHienTC4cAqlgTwUqQaVU-QhymcaTA65163zrWV1vJjrwGMkPYLe6T1dxcuWc-bJeELgmhP4QDp-svw-gF4Gm5ZyaTpcjctE2U2zGKjIVSuEu-tQXZswWVFHGPBcKNMeMZers6rC1On5TnHnWtDRjbe1kLtfPvAOWDZ0_AH6UDdE)](https://mermaid.live/edit#pako:eNqNklFPgzAQx7_KpU-abF-AhyVMtrgsQhyg0fBya2_QDFoshajLvrtFXGI0oH26XO9-_7t_7sS4FsQ81tBLS4pTIDE3WGUK3KvRWMlljcrCTSlJ2d_5mExHZsgPNfPFYkh60Fi0bQO67gzmpOCqIgvWoGqQW6nVXIrroTXUlsDIvLCgD3Dpf8BSCiJjgX-TH36dzKDnQRr6aXIb7TbPq-B_OEO8sKRGeOtot9wEwSqcgvklvIKL3jUd3WqIKie3JuZihJpEEdz54dMUNFL7niZ-mDS69zaMHienTC4cAqlgTwUqQaVU-QhymcaTA65163zrWV1vJjrwGMkPYLe6T1dxcuWc-bJeELgmhP4QDp-svw-gF4Gm5ZyaTpcjctE2U2zGKjIVSuEu-tQXZswWVFHGPBcKNMeMZers6rC1On5TnHnWtDRjbe1kLtfPvAOWDZ0_AH6UDdE)

##### Bulk verwijderen
[![Bulk verwijderen](https://mermaid.ink/img/pako:eNqNkt1ugzAMhV_FytUmtS_ARSU6qFZVA63Apk3cuIkLWSF0IbCfqu--MIY0qYItV5Zz8h3b8YnxShBzWE2vDSlOnsRMY5kqsOeI2kguj6gM3BSSlLnMR6Rb0n2-18wXiz7pwK4pDmCjN_kiSJPqZUFlCLTMcgPVHgbtAxZSEGkD_JdVf2uRPduBJHCT-Dbcrp997384TTw3g_cFbxVul2vP84MpmFvAe9fHZ0UHUoCoMmo1YiZGqHEYwp0bPE1BQ7XraAKMRlUjN7JScylG-94E4eNklfHAIZAKdpSjElRIlY0gl0k0WeCqauzcOlbbDRMteIzkerD17xM_iq_sZH5GLwjsI4SSDOy_Wdd__lhnAnXDOdVtVYzYhZtUsRkrSZcohd3eUydMmcmppJQ5NhSoDylL1dnqsDFV9KE4c4xuaMaao7UZNp05eyxqOn8BddoGpw)](https://mermaid.live/edit#pako:eNqNkt1ugzAMhV_FytUmtS_ARSU6qFZVA63Apk3cuIkLWSF0IbCfqu--MIY0qYItV5Zz8h3b8YnxShBzWE2vDSlOnsRMY5kqsOeI2kguj6gM3BSSlLnMR6Rb0n2-18wXiz7pwK4pDmCjN_kiSJPqZUFlCLTMcgPVHgbtAxZSEGkD_JdVf2uRPduBJHCT-Dbcrp997384TTw3g_cFbxVul2vP84MpmFvAe9fHZ0UHUoCoMmo1YiZGqHEYwp0bPE1BQ7XraAKMRlUjN7JScylG-94E4eNklfHAIZAKdpSjElRIlY0gl0k0WeCqauzcOlbbDRMteIzkerD17xM_iq_sZH5GLwjsI4SSDOy_Wdd__lhnAnXDOdVtVYzYhZtUsRkrSZcohd3eUydMmcmppJQ5NhSoDylL1dnqsDFV9KE4c4xuaMaao7UZNp05eyxqOn8BddoGpw)

##### Historie opvragen

[![Historie opvragen](https://mermaid.ink/img/pako:eNqNkd1qwzAMhV_F-GqD9gV8UciWjpWxGPoz2PCNsNXEq2NnilO2lb77PExgY6RMV-Lo6BOSTlwHg1zwHt8G9BpLCzVBqzxL0QFFq20HPrJbZ9HHv_oG6YiU9eyZLxZZFKyxfQxkMZerEJGRrZvIwp6Nnidw1iBSZPrHiFxNqMwUbFcVu-29XK9eluX_cIS6iegneHdyfbMqy2V1CVY49s5S9hnwgJ4B-BqPBFCbCepWSvZYVM-XoLJrwCXaeJy5s6_91Nry4eq371p5PuMtUgvWpMedvhsVjw22qLhIqQE6KK78OflgiGHz4TUXkQac8aEzEMcnc7EH1-P5C00Nrvw)](https://mermaid.live/edit#pako:eNqNkd1qwzAMhV_F-GqD9gV8UciWjpWxGPoz2PCNsNXEq2NnilO2lb77PExgY6RMV-Lo6BOSTlwHg1zwHt8G9BpLCzVBqzxL0QFFq20HPrJbZ9HHv_oG6YiU9eyZLxZZFKyxfQxkMZerEJGRrZvIwp6Nnidw1iBSZPrHiFxNqMwUbFcVu-29XK9eluX_cIS6iegneHdyfbMqy2V1CVY49s5S9hnwgJ4B-BqPBFCbCepWSvZYVM-XoLJrwCXaeJy5s6_91Nry4eq371p5PuMtUgvWpMedvhsVjw22qLhIqQE6KK78OflgiGHz4TUXkQac8aEzEMcnc7EH1-P5C00Nrvw)

