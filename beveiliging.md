# Beveiliging

Beveiliging is uiteraard een belangrijk aspect. Zowel opvragen als aanbieden van data moet worden beveiligd.

De implementator van de API moet in de OpenAPI specification aangeven met welk system de authenticatie verloopt. Ook moet aangegeven worden hoe eventueel een account kan worden aangevraagd.

Voor ophalen van data wordt gebruik gemaakt van OAUTH2 Authentication Flow.

Voor het wijzigen van data wordt ook gebruik gemaakt van OAUTH2, maar dan de Client Credential Flow. Deze staat op de nominatielijst van Kennisplatform API. Voor de Waterkwaliteit-API mogen we daar alvast op voorsorteren.

__Alle__ verbindingen in een test- of productie-scenario __moeten__ TLS 1.3 zijn, of beter.
