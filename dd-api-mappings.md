# DD-API mappings

dd-api -> OMS

- Geen Waarnemingssoorten: gebruik eenheid, parameter en grootheid
- Locatie-info wordt in het upload document opgenomen

Timeseries:

- Processname ->
- StartTime ->
- EndTime ->
- LocationCode -> reference(featureOfInterest)
- SourceName ->
- Eenheid ->
- RealizationCount? ->
- ValueType -> (ObservationType derivate) ->
- nodeId? ->
- qualifier ->
- aspectSet? ->
- Quantity -> reference(observedProperty)
- IntervalLength ->
