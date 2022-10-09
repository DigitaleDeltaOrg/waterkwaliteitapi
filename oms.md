# Observations, Measurements and Samples (oftewel O&M 2022)

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

Een observatie:

- is geen meting, maar een gemeten of geobserveerde waarde.
- heeft een bepaalde [type](ObservationCategories). Het type bepaald welke data kan worden vastgelegd. Een tijdsreeks kan wél als één Observation worden vastgelegd.
- kan relaties hebben met andere observaties, zoals in ObservationCollection. _buiten scope_

In klassieke zin is een meting nu een ObservationCollection geworden.

Dit maakt met mogelijk om nieuwe items toe te voegen aan een 'meting' zonder het model te hoeven wijzigen.

OMS kent de volgende (vrij vertaalde) concepten:

Concepten:

| Naam | Omschrijving |
| ----- | ---------- |
| Observer | Die- of datgene die Observation-gebeurtenissen genereert. Vertaling: de organisatie of het apparaat welke de Observaties doet. |
| Deployment | Opdracht voor observaties aan Host. |
| Host | Groepering van Observaties, zoals een fysiek platform, monitoringstation.  |
| Sampler | Apparaat, entiteit (persoon, organisatie) die Samples aanmaakt. |
| ObservationCollection | Een collectie van gelijksoortige Observations. Vrij vertaald: alle elementen die tezamen een klassieke meting vormen. |
| Sample | Monster. Een Observation hoeft niet noodzakelijkerwijs uit een monster voort te komen. Sample komt voort uit een sampler, wat een apparaat kan zijn. |
| Sampling | Het proces van monstername. |
| Specimen | Specialisatie van een monster, waar onder anderen kan worden aangegeven waar het monster genomen is en waar het zich momenteel bevindt. |
| Observation | Een gemeten waarde. Dit kan numeriek zijn, maar ook een type. Zie [ObservationTypes](ObservationTypes). Er kunnen niet meerdere waarden in Observation worden opgenomen (behalve bij een timeseries, waar het gaat om een array van gemeten waarden). Daarom zijn er ObservationCollections, om de verschillende gemeten of geobserveerde waarden, te combineren tot een klassieke meting. |
| FeatureOfInterest | Het te onderzoeken onderwerp, bijvoorbeeld het waterlichaam of een specifieke locatie. |

## Wie/waar is de eigenaar?

Er is onduidelijkheid over hoe moet worden vastgelegd welke organisatie de 'eigenaar' is van de data.
Voorstel is om dit als member-element op te nemen als een Link in ObservationCollection, bijvoorbeeld door het opnemen van de Namespace, zoals die gebruikt wordt in IM Metingen.

Conceptuele vertaling: wat wij in het verleden zagen als een meting, is eigenlijk een een serie afzonderlijke observaties: een ObservationCollection.
Iedere separaat geconstateerd item is een observatie. In het geval van tijdsreeksen kan dat een serie meetwaarden/tijdstip combinaties zijn.

## ObservationTypes

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

## Samples

```Sample```s staan in OMS min of meer _naast_ ```ObservationCollection```s.
Dit zorgt ervoor dat ```Sample```s niet nodig zijn voor ```ObservationCollection```s.
Veelal zijn ```Sample```s niet nodig voor tijdreeksen. Ze kunnen echter nuttig zijn om, bijvoorbeeld, apparaat-informatie kwijt te kunnen.
Bij een ```Sample``` in OMS worden referenties gelegd naar ```ObservationCollection```s.

## Data types

``` temp
ObservationCollection
    *id:string
    type:string (summarizing, homogeneous)
    procedure: ref[procedure]
    featureOfInterest: ref[featureOfInterest>
    samplingStrategy: red[sampling] of link
    observedProperty (gemeenschappelijk in alle Observation binnen deze collectie)
    phenomenonTime (gemeenschappelijk in alle Observation binnen deze collectie)
    resultTime (gemeenschappelijk in alle Observation binnen deze collectie)
    uom (gemeenschappelijk in alle Observation binnen deze collectie)
    vocabulary:0..n[Link] (gemeenschappelijk in alle Observation binnen deze collectie)
    *member:1..n[Observation|Link] (lijst van Observation of links binnen deze collectie)

Observation
    *id:string
    *type:string (measurement(1), category-observation(2), truth-observation(3), count-observation(4), timeseries-observation(5))
    context:ref:Observation[0..n]
    phenomenonTime
    observedProperty
    procedure
    featureOfInterest
    samplingStrategy
    resultTime
    parameter
    *result:1:[Link|Measure|VocabTerm|Truth|Count|Text|TimeseriesTVP]
  
Measure
    *value:number (scaled)
    uom:string

Truth
    *truth:boolean

Text
    *text:string

VocalTerm
    *term:string
    vocabulary:uri

Link
    link
        *href:uri
        rel:uri
        title:string

Sampling -> monster
    *id:string
    *type:string(specimen,spatialSampling)
    sampledFeature:Link[1..n]
    complex:Link[1..n]

SamplingFeature
    *id:string
    *type:string
    sampledFeature:Link[1..n]
    complex:Link[1..n]

SamplingCollection
    id
    type
    sampledFeature
    member:1..n[Link|Sampling|Specimen]

Specimen
    *id:string
    type
    sampledFeature:Link[1..n]
    relatedObservation
    complex
    *samplingTime
    samplingMethod
    samplingLocation
    samplingElevation
        elevation
        verticalDatum
    currentLocation:Link|Text|GeometryObject
    size:Measure

SpatialSampling:Sampling
    *id
    type:string(SamplingPoint|SamplingCurve|SamplingSurface|SamplingSolid)
    *sampledFeature:Link[1..n]
    relatedObservation
    complex
    *shape
    hostedProcedure

TimeseriesTVP
    *id:string
    type:string(TVPairMeasure|TVPairCategory)
    metadata:TimSeriesMetadata
    defaultPointMetadata:PointMetaData
    *points:(TVPairMeasure|TVPairCategory)

TVPairMeasure
    time:TemporalPrimitive
    value:number
    metadata:PointMetaData

TVPairCategory
    time:TemporalPrimitive
    value:VocabTerm

Temporal:
    time: TemporalObject

date-time:string
date:string
g-year-month: string
g-year: string
dateTimePosition:date-tme
DateTimeInstant
    instant:DateTimePosition
DateTimeInterval:
    begin:DateTimePosition
    end:DateTimePosition
TemporalPrimitive:DateTimeInstant|DateTimeInterval
Duration:string
TemporalObject:Duration|DateTimeInstant|DateTimeInterval

TimeseriesMetadata
    temporalExtent:DateTimeInterval
    baseTime:DateTimeInstant
    spacing:Duration
    commentBlock:
        applicablePeriod:DateTimeInterval
        comment:string
    commentBlocks:Comme
```
