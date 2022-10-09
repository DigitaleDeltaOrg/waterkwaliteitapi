# Validatie

## Validatiefouten

Iedere entiteit (Sample, ObservationCollection, Observation, grootheid, etc.) heeft een **globaal** uniek Id.
Dit Id wordt gebruikt om fouten te registreren. De validatiefouten komen in een aparte property te staan ```validationerrors```.

{ "error": [ { "id": [ error1, error2, ... ] } ] }

Waarden-fouten
 100: ongeldige waarde (bereik)
 101: ongeldige waarde (type)
 102: ongeldige waarde (context)

Type-fouten
 200: niet-ondersteund ObservationType
 201: niet-ondersteund ObservationCollectionType

Referentie-fouten
 300: onbekende referentie
 301: niet-ondersteunde context
