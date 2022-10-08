# HTTP Fouten

| Statuscode | Numeriek | Betekenis |
| ---------- | -------- | --------- |
| OK | 200 | Request geaccepteerd |
| ACCEPTED | 202 | Bulk request ontvangen maar nog niet uitgevoerd (Bulk operaties) |
| NO CONTENT | 204 | Request ontvangen en verwerkt, maar geen content |
| BAD REQUEST | 400 | Payload, request of URI waren foutief (ook bij bulk validatie) |
| NOT FOUND | 404 | Foutieve resource URI |
| METHOD NOT ALLOWED | 405 | Methode (GET, POST, PATCH, PUT, DELETE, HEAD) niet toegestaan |
| PRECONDITION FAILED | 412 | Opgegeven condities onjuist (syntax) |
| UNAUTHORIZED | 401 | Niet geauthenticeerd |
| FORBIDDEN | 403 | Geen toestemming om actie uit te voeren |
| TOO MANY REQUESTS | 429 | Teveel requests (ook bulk) |
| INTERNAL SERVER ERROR | 500 | Onvoorziene fout |
