# OMS-light

OMS-light is een vereenvoudigde versie van OMS, waar toch alle zaken in staan die nodig zijn voor de Waterkwaliteit-API.
Hierdoor wordt compatibiliteit met OMS gehandhaald: er worden alleen niet-relevante elementen weggelaten, gezien vanuit het bereik van deze versie van de Waterkwaliteit-API.

OMS-light is beschreven in [YAML](https://yaml.org) formaat omdat het leesbaarder is dan JSON. Daarnaast zijn voorbeelden gegeven in JSON formaat.

De volgende elementen worden weggelaten uit OMS:

## Observations

- TimeseriesObservation
- CoverateObservation
- GeometryObservation
- ComplexObservation
- PointCoverageObservation
- ObservationType:Homogeneous
- Observation:Context
- Observation:result:temporalObject
- Observation:result:geometryObject
- Observation:result:TimeseriesTVP

## ObservationCollections

- TimeseriesObservation
- CoverateObservation
- GeometryObservation
- ComplexObservation
- PointCoverageObservation
- ObservationType:Homogeneous
- Procedure
- FeatureOfInterest
- SamplingStrategy
- ObservedProperty
- PhenomenonTime
- ResultTime
- Uom
- Vocabulary

## Samples

- Deployment
- DeploymentRequirement
- RelatedObservation
- MaterialSample:RelatedSample
- Host
- StatisticalSample
- SpatialSample
- SpatialSamplingFeature
- SamplingCurve
- SamplingSolid
- MaterialSample
- SampleCollection
- Specimen:CurrentLocation
- Specimen:SamplingElevation
- Specimen:Size
- Specimen:SamplingLocation:text
- Specimen:SamplingLocation:geometryObject
- Specimen:SamplingElevation

## Temporal

Temporal beschrijft formaten die gebruikt kunnen worden, gerelateerd aan tijd.
De volgende zijn niet relevant voor ecologische metingen:

- g-year-month
- g-year
- dateTimeInstant
- dateTimeInterval
- temporalPrimitive
- duration

In essentie blijft alleen datum-type dateTimePosition, over, wat een datum-tijd is, met eventueel een tijdzone, conform ISO 8610.

## Redenering

Bij ecologische meting is altijd een monster aanwezig. De superset Specimen wordt daarom gebruikt, in plaats van (Material)Sampling.
Het pad Specimen -> Observation (via ObservationCollection) zal dus altijd aanwezig zijn.

ObservationCollection heeft alleen nog de twee verplichte elementen: Id (identificatie van de ObservationCollection) en member (de onderliggende meetwaarden).
De reden: de andere velden geven aan wat identiek is voor *alle* onderliggende Observations. Dit komt bijna nooit voor bij ecologische metingen.
Daarnaast zou het de complexiteit verhogen: naast zoeken in Observation zou ook naar hetzelfde gezocht moeten worden in ObservationCollection.

## Light-plus

Er is wél een toevoeging, die de OMS specificatie niet in de weg zit: het [referentiesysteem](referentieblok.md), om links naar Aquo leesbaar te houden.
Het doel van JSON is namelijk het leesbaar houden van de content. Links met alleen Ids zonder context, doet dat te niet.

## Vastlegging

Regels:

- Ieder Specimen heeft een uniek Id
- Ieder ObservationCollection heeft een uniek Id
- Ieder Observation heeft een uniek Id

Wanneer een veld gemarkeerd is met een *, dan is het een verplicht veld.

### Specimen

- *Uniek Id: string
- *SamplingTime: DateTimePosition
- SamplingMethod: link: /references/samplingmethods/...
- *SamplingLocation: link: /references/locations/...
- SamplingSampledFeature: link: /references/features/...
- members: Links:ObservationCollection[1..n]

### ObservationCollection

- *Uniek Id: string
- *member: Links:Observation[1..n]

### Observation

- *Uniek Id: string
- *Type: string: CategoryObservation, CountObservation, TruthObservation, Measure, Text
- *member: Observation[1..n]
- phenomenonTime: DateTimePosition
- *observedProperty: link: /references/quantities/...
- procedure: link: /references/procedures/...
- featureOfInterest: link: /references/locations/...
- samplingStrategy: link: /references/samplingStrategy/...
- parameter: link: /references/parameters/...
- *resultTime: DateTimePosition
- *result:
  - text: string
  - CategoryObservation
    - *term: string
    - *vocabulary: href
  - CountObservation
    - *count: int
  - Measure
    - *value: decimal
    - *uom: link: /references/units/...
  - TruthObservation
    *truth: bool
  - link
    - *href: string
    - rel: string
    - title: string

Iedere Observation kan dus slechts één enkele waarde bevatten. Bij elkaar behorende meetwaarden worden verzameld in een ObservationCollection.

ResultTime is hier verplicht, want niet verplicht is in OMS. De reden: resultTime moet ergens in de structuur worden vastgelegd. Dat kan in de context van de Waterkwaliteit-API slechts op één plek: Observation.

Een veld welke niet standaard aanwezig is, is de eigenaar. Daartoe kan echter bij de ObservationCollection een extra Observation worden opgegeven van het type CategoryObervation. Die kan dan als term de code van de organisatie bevatten, en als vocabulary een link naar /references/relations.
In het Definitieboek kan worden vastgelegd dat deze vereist is (meetinstantie).
