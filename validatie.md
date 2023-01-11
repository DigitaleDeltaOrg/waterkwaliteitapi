# Validatie

Validatie gebeurd *na* het uploaden.
Alle entiteiten (MeasurementObjects, Samples and Measurements) moeten worden gecontroleerd op bestaan, aanwezigheid, bereik en valide combinaties.

__De combinatie-validatie zijn afhankelijk van de business rules. Deze specificatie beschrijft deze niet.__

De validatie moet gebeuren op zowel entiteiten in het bestand als in het opslagsysteem.
Voorbeeld: een meetobject kan afwezig zijn in het bestand, maar aan7wezig zijn in het opslagsysteem.
Het kan echter ook zijn dat een meetobject aanwezig is in het bestand, maar afwezig in het opslagsysteem.
Beide situaties zijn geldig.
Wanneer het bestand echter een meetobject heeft, maar de data afwijkt van de informatie in het opslagsysteem, dan moet het meetobject in het opslagsysteem overschreven worden.
Monsters en metingen worden geacht immutabel te zijn, en mogen dus niet worden overschreven via het bestand. Hiervoor geldt dat bestaande monsters en metingen eerst via bulkverwerking verwijderd moeten worden.

## Validatiefouten

Iedere entiteit (Sample, ObservationCollection, Observation, grootheid, etc.) heeft een *globaal* uniek Id, van het type IdentityType.
Dit Id wordt gebruikt om fouten te registreren. De validatiefouten komen in een aparte property te staan: ```validationerrors```.

Waarden-fouten
 100: ongeldige waarde (bereik)
 101: ongeldige waarde (type)
 102: ongeldige waarde (context)

Referentie-fouten
 300: onbekende referentie
 301: niet-ondersteunde context

Identity-fouten
 401: duplicaat Id in bestand
 402: Id is al in gebruik
