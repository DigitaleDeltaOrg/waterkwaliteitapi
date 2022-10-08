# OData en OMS

https://docs.ogc.org/is/18-088/18-088.html

OData en OMS kunnen worden geïntegreerd. Voor OData geldt dat een Entity Data Model nodig is voor de beste werking. Vanuit het OMS én de Definitieboek kan een EDM worden gegenereerd als een CSDL.
De CSDL helpt dan ook met de Discovery functionaliteit van OData.

```
/Capabilities
/ObservationOfferings
/ObservedProperties?$select=OfferingID,ObservedPropertyURI
/FeaturesOfInterest?$select=FOIID
/Procedures?$filter=ProcedureID eq 'procedureID'&$select=SensorML
/Observations?$filter=OfferingID eq 'offeringID' and ObservedPropertyURI eq 'observedPropertyURI' and Latitude ge 'minLat’ and Latitude le ‘maxLat' and Longitude ge 'minLon' and Longitude le 'maxLon' and EventTime ge datetime'startTime' and EventTime le datetime'endTime'
```

Observation
    ObservationLocation
    ResultType


| Entiteit | Omschrijving | Alternatief |
| -------- | ------------ | ----------- |
| ObservedProperty | | |
| FeatureOfInterest | | foi |
| Procedure | | |
| uom | | |
| capacity | | |
| observation | | |
| sample | | |
| compartment | | |
| device | | |


/locations
/capabilities
/featuresofinterest
/procedures
/observations
/samples
/uom
/capacity
/parameter
/compartment
/devices

/observations/$filter=Latitude ge 'minLat' and Latitude le 'maxLat' and Longitude ge 'minLon' and Longitude le 'maxLon' and EventTime ge datetime'startTime' and EventTime le datetime'endTime' and ObservedProperty eq 'capacity' and ObservedProperty.value > 90

/observedProperty filter:
name[1]
definition[1]
description[1]
properties[0..1]

/observations filter:
phenomenonTime[1]
result[1]
resultTime[1]
resultQuality[0..n]
validTime[0..1]
parameters[0..1]

/featureOfInterest filter
name[1]
description[1]
encodingType[1]
feature[1]
properties[0..1]


http://example.org/v1.1/Observations?$filter=result lt 10.00
http://example.org/v1.1/Things?$filter=geo.distance(Locations/location, geography'POINT(-122 43)') gt 1


/unitOfMeasurements: A JSON array of JSON objects that containing three key-value pairs. The name property presents the full name of the unitOfMeasurement; the symbol property shows the textual form of the unit symbol; and the definition contains the URI defining the unitOfMeasurement. (see Req 42 for the constraints between unitOfMeasurement, multiObservationDataType and result).



:2022(E)
© ISO 2022 – All rights reserved
 
2
Licensed to: Boersma, K. Dhr.
Downloaded: 2022-04-04
Single user licence only, copying and networking prohibited
3.12
measurement
set of operations having the object of determining the value of a quantity
[SOURCE: ISO 19101-2:2018, 3.21]
3.13
observation
act carried out by an observer to determine the value of an observable property of an object (feature-ofinterest) by using a procedure, with the value is provided as the result
3.14
observer
identifiable entity that can generate observations pertaining to an observable property by implementing 
a procedure
Note 1 to entry: An observer is an instance of a sensor, instrument, implementation of an algorithm or a being 
such as a person.
3.15
procedure
specified way to carry out an activity or a process
[SOURCE: ISO 9000:2015, 3.4.5, modified — Note 1 to entry has been deleted.]
3.16
process
set of interrelated or interacting activities that use inputs to deliver an intended result
[SOURCE: ISO 9000:2015, 3.4.1, modified — Notes 1-6 have been deleted.]
3.17
property
facet or attribute of an object referenced by a name
EXAMPLE Abby's car has the colour red, where "colour red" is a property of the car.
[SOURCE: ISO 19143:2010, 4.21, modified — Example has been added to the entry.]
3.18
property type
characteristic of a feature type
Note 1 to entry: The value for an instance of an observable property type can be estimated through an act of 
observation.
EXAMPLE Cars (a feature type) all have a characteristic colour, where "colour" is a property type.
Note 2 to entry: In chemistry-related applications, the term “determinand” or “analyte” is often used.
Note 3 to entry: Adapted from ISO 19109:2005.
3.19
proximate feature-of-interest
entity that is directly of interest in the act of observing
Note 1 to entry: This is a specialized form of the feature-of-interest
ISO/DIS 19156:2022(E)
© ISO 2022 – All rights reserved 3
Licensed to: Boersma, K. Dhr.
Downloaded: 2022-04-04
Single user licence only, copying and networking prohibited
3.20
range
<coverage> set of feature attribute values associated by a function, the coverage, with the elements of 
the domain of a coverage
Note 1 to entry: This is consistent with the more generic definition of range in ISO 19107:2019.
[SOURCE: ISO/DIS 19123-1, 3.1.47]
3.21
sample
object that is representative of a concept, real-world object or phenomenon
3.22
sampler
device or entity (including humans) that is used by, or implements, a sampling procedure to create or 
transform one or more sample(s)
3.23
sensor
element of a measuring system that is directly affected by a phenomenon, body, or substance carrying a 
quantity to be measured
[SOURCE: ISO/IEC Guide 99:2007, 3.8, modified — Example and Note 1 to entry were deleted.]
3.24
ultimate feature-of-interest
entity that is ultimately of interest in the act of observing
Note 1 to entry: This is a specialized form of the feature-of-interest.
3.25
unit of measure
reference quantity chosen from a unit equivalence group
Note 1 to entry: In positioning services, the usual units of measurement are either angular units or linear units. 
Implementations of positioning services must clearly distinguish between SI units and non-SI units. When non-SI 
units are employed, it is required that their relation to SI units be specified.
[SOURCE: ISO 19116:2019, 3.29]
3.26
value
element of a type domain
Note 1 to entry: A value considers a possible state of an object within a class or type (domain).
Note 2 to entry: A data value is an instance of a datatype, a value without identity.
Note 3 to entry: A value can use one of a variety of scales including nominal, ordinal, ratio and interval, spatial and 
temporal. Primitive datatypes can be combined to form aggregate datatypes with aggregate values, including 
vectors, tensors and images.
[SOURCE: ISO/IEC 19501:2005, 0000_5, modified — Note 3 to entry has been added.]


Observation
    phenomenonTime
    resultTime
    validTime
    ref: featureOfInterest
    ref: observedProperty
    result
    ref: observingProcedure
    ref: host
    ref: observableProperty
    ref: uom
    ref: parameter
    ref: proximateFeatureOfInterest
    ref: ultimateFeatureOfInterest
    observationType
    resultTime


ObservableProperty
Procedure
ObservingProcedure
Observer
Deployment -> Onderzoeksschip, sensorinformatie, monitoringinformatie, persoon

