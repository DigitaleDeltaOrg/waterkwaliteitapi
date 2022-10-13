# OMS export opbouw

Voor de Waterkwaliteit-API wordt álleen het collectie-uitvoerformaat gebruikt. Daarom wordt er bij een export altijd een paginablok weergegeven
Een OMS export in JSON formaat bestaat uit drie onderdelen:

Het [paginablok](paginablok.md) zal zoveel mogelijk voldoen aan het OData formaat.
Het [referentieblok](referentieblok.md) zal alleen referenties bevatten.
Het [resultaatblok](resultaatblok.md) zal de meting-data bevatten.

Een voorbeeld van een output:

```TODO```

Pagineren gaat op basis van het aantal Observations. Tenzij specifiek opgegeven door de gebruiker, worden alle ObservationCollections en Samples gerelateerd aan de Observations in de export, meegegeven als losse blokken.