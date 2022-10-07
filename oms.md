# Observations, Measurements and Samples (oftewel O&M 2)

## Introductie

Voor de Waterkwaliteit-API is de basis OMS, de opvolger van Observations and Measurements.
De basis van IM Metingen is nog de originele versie, met uitbreidingen.

Het volgende is veranderd in OMS ten opzicht van de originele O&M uit 2011.

## OMS

Het concept van meting en observatie is veranderd:

``` quote
An observation is an act associated with a discrete time instant or period through which a number, 
term or other symbol is assigned to a characteristic. This act involves application of a specified 
procedure, such as a sensor, instrument, algorithm or process chain. The procedure may be applied 
in-situ, remotely, or ex-situ with respect to the sampling location. The result of an observation is an 
estimate of the value of a property of some feature; an observation is a property-value-provider for a 
feature-of-interest. Use of a common model allows observation data using different procedures to be 
combined unambiguously.
In conventional measurement theory the term "measurement" is used. 
However, a distinction between measurement and category-observation has been adopted in more 
recent work, so the term "observation" is used here for the general concept. "Measurement" 
may be reserved for cases where the result is a numerical quantity.

The observation itself is also a feature, since it has properties and identity.
Observation details are important for data discovery and for data quality estimation.
The observation could be considered to carry "property-level" instance metadata, which complements 
the dataset-level and feature-level metadata commonly provided via catalogue services (e.g. ISO 19115 or other community agreed one).
```

Conceptuele vertaling: wat wij in het verleden zagen als een meting, is eigenlijk een een serie afzonderlijke observaties: een ObservationCollection.
Iedere separaat geconstateerd item is een observatie. In het geval van tijdsreeksen kan dat een serie meetwaarden/tijdstip combinaties zijn.

Ieder Observation heeft een type, en de type bepaalde de eigenschappen van een Observation.

| Type | Beschrijving | Eigenschappen |
| ---- | ------------- | ------------ |
| measurement | Meting, als numerieke waarde | Bevat waarde + eenheid (uom) |
| category-observation | Een geconstateerde categorie, bijvoorbeeld lengteklasse. | Referentie naar waarde. _Deze worden in het [Definitieboek](definitieboek.md) vastgelegd_. |
| truth-observation | Waar of onwaar | Boolean |
| count-observation | Telling, bijvoorbeeld aantal cellen | Geheel getal, geen eenheid. Aquo DIMSLS |
| timeseries-observation | Tijdreeks | Set van combinaties van tijdstip + gemeten waarde |

Er zijn meer typen, maar bovengenoemde liggen in de scope van dit project.

Sampling kan in OMS van alles zijn, tot en met een monster dat in een museum wordt beheerd. Niet alles zal binnen de scope van dit project plaatsvinden.
