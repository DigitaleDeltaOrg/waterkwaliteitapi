# OMS Import opbouw

Een OMS import heeft een andere struktuur dan het export formaat. Dat komt doordat Samples en ObservationCollections optioneel zijn. Bij het importeren worden daarom Observations los gezien van Samples en ObservationCollections.
Dat zullen drie verschillende blokken zijn.

Het import-bestand zal verder duidelijk aangeven welke geografische datum wordt gebruikt.
Het import-bestand zal ook een referentieblok bevatten, om de grootte van het bestand te beperken.
Daarnaast kan het referentieblok dienen voor het aanmaken van ontbrekende referenties in het doelsysteem.

```json
{
  "crs": "EPSG:28992",
  "references": [
    "locations": [],
    "units": [],
    "compartments": [],
    "quantities": [],
    "samplingmethods": [],
    "parameters": [],
    "...": [],
  ],
  "observations": [],
  "samples": [{ "Id": "", "samplinglocation": "/references/locations/VEEN", "relatedObservation": [ "observation-id1", "observation-id2", "observation-id3" ]}],
  "observationcollections": [ {"Id": "", "member": [ "observation-id1", "observation-id2", "observation-id3" ]}]
}
```
