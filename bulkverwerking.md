# Aanbieden: Bulk verwerking

Bulk-verwerking is niet iets dat ingebakken is in REST. Wegens volume van de data en de verwerkingstijd, is gekozen voor een asynchrone oplossing.
Toevoegen en verwijderen is toegestaan.
Updaten is *niet* toegestaan. Dit om een aantal redenen:

- Metingen behoren immutable te zijn
- Updaten van data, helemaal indien deze in een bepaalde structuur staan, is zeer inefficient en kan vaak niet in een bulk-operatie plaatsvinden.
- Ook de historie bijhouden wordt ingewikkeld.

Moeten metingen worden gemuteerd, dan dienen deze eerst verwijderd te worden, en daarna opnieuw ge&iuml;mporteerd.

Authenticeren gaat met de OAUTH2 Client Credential Flow, die wordt gebruikt als machine-to-machine koppeling.
De API implementatie dient het inlog-account te associëren met een bepaalde organisatie en op basis daarvan te valideren of het verzoek terecht is.

*Data aanleveren moet laagdrempelig gebeuren om draagvlak te krijgen en behouden.*
Mogelijk is het IM Metingen formaat of het O&M formaat te complex en leidt tot een groter datavolume.
We kunnen kijken naar vereenvoudigde formaten: een timeseries formaat en een monster-gebaseerd formaat. Voor timeseries zou het bekende DD-API formaat, met kleine aanpassingen, kunnen worden gebruikt. Voor de andere metingen kunnen we een formaat distilleren die dichter staat bij O&M.

Verwerking zal niet meteen gebeuren. Daarom kan de client een status bij de server opvragen.

Valideren is de eerste fase van verwerking. Pas wanneer de bulk-content volledig gevalideerd is, kan er daadwerkelijk worden ge&iuml;mporteerd.

[Fouten in validatie](validatie.md#validatiefouten) worden ook gestandaardiseerd.

## Bulk data valideren/toevoegen

[![Valideren/toevoegen](https://mermaid.ink/img/pako:eNqNkdFKw0AQRX9l2CeF9gfyUGhNxSIk0BhBycu4O02WJLNxMylq6b-7shYEaek8Xe7cOTMwB6WdIZWokd4nYk2pxdpjXzGEGtCL1XZAFrjrLLH89wvye_LRj5n5YhHNBN6mrgXLI_nfycwJgbd1I-B2cIo9Y2cNhRDoP1tiN9AiNoEyW5ZPD_l287pOr8N50o0Qn-Hd59vVJk3X2SXYsoMPCOrLUUsMiFzT3iPW5gx1VRYvF6-LLBBHowTcGUz-eCMeeUQt1vHcmtuK1Uz15Hu0Jnzs8DNYKWmop0olQRr0baUqPoYcTuKKT9YqET_RTE2DQTl9VyU77EY6fgOcsKye)](https://mermaid.live/edit#pako:eNqNkdFKw0AQRX9l2CeF9gfyUGhNxSIk0BhBycu4O02WJLNxMylq6b-7shYEaek8Xe7cOTMwB6WdIZWokd4nYk2pxdpjXzGEGtCL1XZAFrjrLLH89wvye_LRj5n5YhHNBN6mrgXLI_nfycwJgbd1I-B2cIo9Y2cNhRDoP1tiN9AiNoEyW5ZPD_l287pOr8N50o0Qn-Hd59vVJk3X2SXYsoMPCOrLUUsMiFzT3iPW5gx1VRYvF6-LLBBHowTcGUz-eCMeeUQt1vHcmtuK1Uz15Hu0Jnzs8DNYKWmop0olQRr0baUqPoYcTuKKT9YqET_RTE2DQTl9VyU77EY6fgOcsKye)

## Status transactie opvragen

[![Status transactie opvragen](https://mermaid.ink/img/pako:eNqNklFPgzAQx7_KpU-abF-AhyVMtrgsQhyg0fBya2_QDFoshajLvrtFXGI0oH26XO9-_7t_7sS4FsQ81tBLS4pTIDE3WGUK3KvRWMlljcrCTSlJ2d_5mExHZsgPNfPFYkh60Fi0bQO67gzmpOCqIgvWoGqQW6nVXIrroTXUlsDIvLCgD3Dpf8BSCiJjgX-TH36dzKDnQRr6aXIb7TbPq-B_OEO8sKRGeOtot9wEwSqcgvklvIKL3jUd3WqIKie3JuZihJpEEdz54dMUNFL7niZ-mDS69zaMHienTC4cAqlgTwUqQaVU-QhymcaTA65163zrWV1vJjrwGMkPYLe6T1dxcuWc-bJeELgmhP4QDp-svw-gF4Gm5ZyaTpcjctE2U2zGKjIVSuEu-tQXZswWVFHGPBcKNMeMZers6rC1On5TnHnWtDRjbe1kLtfPvAOWDZ0_AH6UDdE)](https://mermaid.live/edit#pako:eNqNklFPgzAQx7_KpU-abF-AhyVMtrgsQhyg0fBya2_QDFoshajLvrtFXGI0oH26XO9-_7t_7sS4FsQ81tBLS4pTIDE3WGUK3KvRWMlljcrCTSlJ2d_5mExHZsgPNfPFYkh60Fi0bQO67gzmpOCqIgvWoGqQW6nVXIrroTXUlsDIvLCgD3Dpf8BSCiJjgX-TH36dzKDnQRr6aXIb7TbPq-B_OEO8sKRGeOtot9wEwSqcgvklvIKL3jUd3WqIKie3JuZihJpEEdz54dMUNFL7niZ-mDS69zaMHienTC4cAqlgTwUqQaVU-QhymcaTA65163zrWV1vJjrwGMkPYLe6T1dxcuWc-bJeELgmhP4QDp-svw-gF4Gm5ZyaTpcjctE2U2zGKjIVSuEu-tQXZswWVFHGPBcKNMeMZers6rC1On5TnHnWtDRjbe1kLtfPvAOWDZ0_AH6UDdE)

## Bulk verwijderen

[![Bulk verwijderen](https://mermaid.ink/img/pako:eNqNkt1ugzAMhV_FytUmtS_ARSU6qFZVA63Apk3cuIkLWSF0IbCfqu--MIY0qYItV5Zz8h3b8YnxShBzWE2vDSlOnsRMY5kqsOeI2kguj6gM3BSSlLnMR6Rb0n2-18wXiz7pwK4pDmCjN_kiSJPqZUFlCLTMcgPVHgbtAxZSEGkD_JdVf2uRPduBJHCT-Dbcrp997384TTw3g_cFbxVul2vP84MpmFvAe9fHZ0UHUoCoMmo1YiZGqHEYwp0bPE1BQ7XraAKMRlUjN7JScylG-94E4eNklfHAIZAKdpSjElRIlY0gl0k0WeCqauzcOlbbDRMteIzkerD17xM_iq_sZH5GLwjsI4SSDOy_Wdd__lhnAnXDOdVtVYzYhZtUsRkrSZcohd3eUydMmcmppJQ5NhSoDylL1dnqsDFV9KE4c4xuaMaao7UZNp05eyxqOn8BddoGpw)](https://mermaid.live/edit#pako:eNqNkt1ugzAMhV_FytUmtS_ARSU6qFZVA63Apk3cuIkLWSF0IbCfqu--MIY0qYItV5Zz8h3b8YnxShBzWE2vDSlOnsRMY5kqsOeI2kguj6gM3BSSlLnMR6Rb0n2-18wXiz7pwK4pDmCjN_kiSJPqZUFlCLTMcgPVHgbtAxZSEGkD_JdVf2uRPduBJHCT-Dbcrp997384TTw3g_cFbxVul2vP84MpmFvAe9fHZ0UHUoCoMmo1YiZGqHEYwp0bPE1BQ7XraAKMRlUjN7JScylG-94E4eNklfHAIZAKdpSjElRIlY0gl0k0WeCqauzcOlbbDRMteIzkerD17xM_iq_sZH5GLwjsI4SSDOy_Wdd__lhnAnXDOdVtVYzYhZtUsRkrSZcohd3eUydMmcmppJQ5NhSoDylL1dnqsDFV9KE4c4xuaMaao7UZNp05eyxqOn8BddoGpw)

## Historie opvragen

Om transparant te zijn, moeten alle __acties__ worden vastgelegd. De aanvrager kan ten allen tijde de history opvragen.

[![Historie opvragen](https://mermaid.ink/img/pako:eNqNkd1qwzAMhV_F-GqD9gV8UciWjpWxGPoz2PCNsNXEq2NnilO2lb77PExgY6RMV-Lo6BOSTlwHg1zwHt8G9BpLCzVBqzxL0QFFq20HPrJbZ9HHv_oG6YiU9eyZLxZZFKyxfQxkMZerEJGRrZvIwp6Nnidw1iBSZPrHiFxNqMwUbFcVu-29XK9eluX_cIS6iegneHdyfbMqy2V1CVY49s5S9hnwgJ4B-BqPBFCbCepWSvZYVM-XoLJrwCXaeJy5s6_91Nry4eq371p5PuMtUgvWpMedvhsVjw22qLhIqQE6KK78OflgiGHz4TUXkQac8aEzEMcnc7EH1-P5C00Nrvw)](https://mermaid.live/edit#pako:eNqNkd1qwzAMhV_F-GqD9gV8UciWjpWxGPoz2PCNsNXEq2NnilO2lb77PExgY6RMV-Lo6BOSTlwHg1zwHt8G9BpLCzVBqzxL0QFFq20HPrJbZ9HHv_oG6YiU9eyZLxZZFKyxfQxkMZerEJGRrZvIwp6Nnidw1iBSZPrHiFxNqMwUbFcVu-29XK9eluX_cIS6iegneHdyfbMqy2V1CVY49s5S9hnwgJ4B-BqPBFCbCepWSvZYVM-XoLJrwCXaeJy5s6_91Nry4eq371p5PuMtUgvWpMedvhsVjw22qLhIqQE6KK78OflgiGHz4TUXkQac8aEzEMcnc7EH1-P5C00Nrvw)
