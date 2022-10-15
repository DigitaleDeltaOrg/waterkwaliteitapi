# Observations, Measurements and Samples (oftewel O&M 2022)

## Introductie

Voor de Waterkwaliteit-API is de basis OMS, de opvolger van Observations and Measurements.
De basis van IM Metingen is nog de originele versie, met uitbreidingen.

OMS is echter enorm groot. Dat komt mede doordat het moet gaan gelden voor alle soorten metingen die er bestaan.
De Waterkwaliteit-API zal hier een subset van gebruiken en tegelijkertijd proberen te voorkomen dat land- of domein-specifieke aanpassingen moeten gebeuren aan het model.

Het volgende is veranderd in OMS ten opzicht van de originele O&M uit 2011.

## OMS

De transitie van O&M naar OMS is niet triviaal.

Eén van de aspecten is de transitie van hard-typing naar soft-typing en de invloed daarop voor codelists:

``` quote
Observation classification by result type and SpatialSamplingFeature by the shape geometry type 
provided as sub-classes in the ISO 19156 Edition 1 are modelled using soft-typing based classification 
schemes in Edition 2 (AbstractObservationCharacteristics.observationType and AbstractSample.
sampleType). This transition from hard-typing to soft-typing has been done to allow the use of the most 
appropriate Observation and Sample classification schemes to be used in the domain models, as well as 
to allow a single Observation and Sample instance to be classified using multiple classification schemes.
```

Dit voorkomt dus land-specifieke aanpassingen aan het model, bijvoorbeeld met betrekking tot lengteklasse, levensstadium, etc.
Dat komt uitwisseling weer ten goede.

Het concept van meting en observatie is ook veranderd:

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
the dataset-level and feature-level metadata commonly provided via catalogue services 
(e.g. ISO 19115 or other community agreed one).
```

Een observatie:

- is geen meting, beschrijft context en resultaat van een observatie, dus hoe, wie, wat, waar heeft geobserveerd
- heeft een bepaalde [type](#observationtypes). Het type bepaald welke data kan worden vastgelegd. Een tijdsreeks kan wél als één Observation worden vastgelegd.
- kan relaties hebben met andere observaties, zoals in ObservationCollection. _buiten scope_
- de ```result``` van een ```Observation``` bevat het meetresultaat, afhankelijk van ```ObservationType```.
- staat op zichzelf: het heeft geen (directe) relatie met Sample of ObservationCollection.
- samplingStrategy wordt niet voor een directe relatie met Sample gebruikt. Het is bedoeld om aan te geven het mogelijk gaat om een sub-sample, die nu niet geïmplementeerd wordt in de Waterkwaliteit-API.

Een mogelijke rol van ObservationCollection is om meetwaarden in een bepaalde context te houden.
Bijvoorbeeld: van parameter Abra Alba zijn 10 volwassen exemplaren gevonden. Dat resulteerd in een CountObservation van 10 en een CategoryObservation van stageoflife/adult.
Alleen wanneer deze waarden worden bijeengehouden, heeft bovenstaande informatie betekenis. Dat kan dus met een ObservationCollection.

Observations staan dus op zichzelf.
Een Sample kan refereren aan één of meerdere Observations (::relatedObservations).
Een ObservationCollection kan refereren aan één of meerdere Observations (::member).

Dit heeft consequenties voor een importmodel, maar ook voor het opvraagmodel.
  
## Concepten

OMS kent de volgende (vrij vertaalde) concepten:

Concepten:

| Naam | Omschrijving |
| ----- | ---------- |
| Feature | Alles wat een identiteit heeft én eigenschappen bevat. Specifieke soorten features beschrijven de eigenschappen die de feature mag en moet hebben. |
| Observer | Die- of datgene die Observation-gebeurtenissen genereert. Vertaling: de organisatie of het apparaat welke de Observaties doet. |
| Sampler | Apparaat, entiteit (persoon, organisatie) die Samples aanmaakt. |
| ObservationCollection | Een collectie van gelijksoortige Observations. Vrij vertaald: alle elementen die tezamen een klassieke meting vormen. |
| Sample | Monster. Een Observation hoeft niet noodzakelijkerwijs uit een monster voort te komen. Sample komt voort uit een sampler, wat een apparaat kan zijn. |
| SamplingProcedure | Het proces van monstername. |
| Specimen | Specialisatie van een monster, waar onder anderen kan worden aangegeven waar het monster genomen is en waar het zich momenteel bevindt. |
| Observation | Een gemeten waarde. Dit kan numeriek zijn, maar ook een type. Zie [ObservationTypes](#observationtypes). Er kunnen niet meerdere waarden in Observation worden opgenomen (behalve bij een timeseries, waar het gaat om een array van gemeten waarden). Daarom zijn er ObservationCollections, om de verschillende gemeten of geobserveerde waarden, te combineren tot een klassieke meting. |
| FeatureOfInterest | Het te onderzoeken onderwerp, bijvoorbeeld het waterlichaam of een specifieke locatie. proximate is het te onderzoeken monster. ultimate is het totale object. Dus proximate is een subset van ultimate. Voor het waterdomein zal dit vaak een locatie zijn. |

## Wie/waar is de eigenaar?

Er is onduidelijkheid over hoe moet worden vastgelegd welke organisatie de 'eigenaar' is van de data. OMS legt dit niet vast. Maar om te voorkomen dat er specifieke uitbreidingen komen, kunnen we aangeven hoe we dit kunnen doen als metadata.
Voorstel is om dit als member-element op te nemen als een Link in Observation, bijvoorbeeld door het opnemen van de Namespace, zoals die gebruikt wordt in IM Metingen. Hierdoor wordt het filteren op 'eigenaar' ook gemakkelijk. Om alle termen Engels te houden, kunnen we dit ```Owner``` noemen.

## ObservationTypes

Ieder Observation heeft een type, en de type bepaalde de eigenschappen van een Observation.
Voor de Waterkwaliteit-API zijn de volgende types van belang:

| Type | Beschrijving | Eigenschappen |
| ---- | ------------- | ------------ |
| measurement | Meting, als numerieke waarde | Bevat waarde + eenheid (uom) |
| category-observation | Een geconstateerde categorie, bijvoorbeeld lengteklasse. | Referentie naar waarde. _Deze worden in het [Definitieboek](definitieboek.md) vastgelegd_. |
| truth-observation | Waar of onwaar | Boolean |
| count-observation | Telling, bijvoorbeeld aantal cellen | Geheel getal, geen eenheid. Aquo DIMSLS |
| text-observation | Text | Bijvoorbeeld een opmerking? |

Er zijn meer typen, maar bovengenoemde liggen in de scope van dit project.

Sampling kan in OMS van alles zijn, tot en met een monster dat in een museum wordt beheerd. Niet alles zal binnen de scope van dit project plaatsvinden.
In de Waterkwaliteit-API zal een subset van Sample worden gebruikt.

## OMS-light

Het gehele OMS implementeren is te groot voor het doel, en zal ook draagvlak in de weg zitten.
Daarom wordt een [verkorte versie van OMS](oms-light.md) geïntroduceerd, die echter wél compatibel is met de uitgebreide versie.
Deze versie houdt _geen_ rekening met tijdsreeksen.
