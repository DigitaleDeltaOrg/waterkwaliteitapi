# Resultaatblok

De volgende formaten zijn in ieder geval aanwezig:

- [observationcollections](voorbeelden/oms-observation-collection-style.json): de 'root' is altijd ```ObservationCollection```. Daarbinnen staan de ```Observation```s.
- [samples](voorbeelden/oms-sample-style.json): de 'root' is altijd Sample. Daarbinnen staan de Observations.
- [observations](voorbeelden/oms-observation-style.json): de root is een ```Observation```. Aan ```Sample```s en ```ObservationCollection```s wordt gerefereerd.

In het geval van observations, worden ```Sample```s als 'samples' en ```ObservationCollection```s als 'observationcollections' opgenomen in het referentieblok.

In alle gevallen zal eerst het [pagingblock](paginablok.md), daarna het [referentieblok](referentieblok.md) en daarna het specifieke [resultaatblok](#resultaatblok) worden geëxporteerd.
