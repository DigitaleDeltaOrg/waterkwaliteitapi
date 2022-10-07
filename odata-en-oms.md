# OData en OMS

https://docs.ogc.org/is/18-088/18-088.html

OData en OMS kunnen worden geïntegreerd. Voor OData geldt dat een Entity Data Model nodig is voor de beste werking. Vanuit het OMS én de Definitieboek kan een EDM worden gegenereerd als een CSDL.
De CSDL helpt dan ook met de Discovery functionaliteit van OData.

/Capabilities
/ObservationOfferings
/ObservedProperties?$select=OfferingID,ObservedPropertyURI
/FeaturesOfInterest?$select=FOIID
/Procedures?$filter=ProcedureID eq 'procedureID'&$select=SensorML
/Observations?$filter=OfferingID eq 'offeringID' and ObservedPropertyURI eq 'observedPropertyURI' and Latitude ge 'minLat’ and Latitude le ‘maxLat' and Longitude ge ‘minLon’ and Longitude le ‘maxLon' and EventTime ge datetime'startTime' and EventTime le datetime'endTime'

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

