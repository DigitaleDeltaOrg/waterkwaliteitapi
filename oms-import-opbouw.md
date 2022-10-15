# OMS Import opbouw

Een OMS import heeft dezelfde struktuur als de [Observations-style](oms-observation-style.md) export.
Samples en ObservationCollections worden beschouwd als referenties.
Via de ```Content-csr``` header wordt de geografische datum meegegeven.

Het referentieblok dient om eventuele gerefereerde entiteiten toe te voegen.

```json
{
  "references": [
    "locations": [],
    "units": [],
    "compartments": [],
    "quantities": [],
    "samplingmethods": [],
    "parameters": [],
    "samples": [],
    "observationcollections": [],
    "...": [],
  ],
  "observations": []
}
```

Voor verwerking, moet de integriteit worden gewaakt. Daartoe worden éérst alle referenties _behalve_ samples en observationcollections verwerkt.
Daarna worden observations verwerkt.
Daarna pas alle referenties naar samples en observationcollections.
