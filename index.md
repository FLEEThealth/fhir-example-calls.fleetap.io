---
layout: page
---

# Fleet Health: FHIR API Access For Everyone

This page shows FHIR API calls that we can support across different vendor EMR systems, including Epic, Cerner and other large national vendors.

Some of these API calls for FHIR resources are supported in various way by the vendors involved in the Sync For Science pilot for the Precision Medicine Initiative.

One important thing to note about FHIR is that it supports many types of data that can point to each other 'by reference'. For example:

 + `patientId` indicates the FHIR `id` of the `Patient` Resource in context.
 
For example, in the request example below:
 
 + "resourceType": "Patient",
    "id": "51200",

shows the FHIR ID number for the patient on this server.

This means that in all other Resources, like for a Medication or Appointment, the patient will be referenced by their number, and not by their name. Like here (shortened json here on purpose...)

 +    {"resourceType": "MedicationOrder"},
         "patient": {
         "reference": "Patient/51200"
         },
         "prescriber": {
         "reference": "Practitioner/7903"
         },
         "medicationCodeableConcept": {
         "coding": [{
         "system": "http://www.fda.gov/Drugs/InformationOnDrugs/ucm142438.htm",
         "code": "00062141116",
         "display": "Ortho Micronor 0.35 mg Tab"
         }
         ]},"dosageInstruction": [{"text": "Take one tablet by mouth daily"}]
         }

This allows for a lot of good things (like say, a reserach study that wanted to study a population in real time, without seeing everyone's names and home address), though it does mean that you need to request a patient resource first, so you can lookup other information about them on the server. 


## Patient demographics
Includes: name, birth sex, birthdate, race, ethnicty, preferred language

| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| FHIR DSTU2 Resource | <http://hl7.org/fhir/patient.html#resource>                    |

##### On *first-connection*, *periodic-update*.
    GET /Patient/{% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Patient/51200 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

## Smoking status

| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| FHIR DSTU2 Resource | <http://hl7.org/fhir/observation.html#resource>                    |

##### On *first-connection*
    GET /Observation?category=social-history&patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Observation?category=social-history&patient=51200 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

##### On *periodic-update*
    GET /Observation?category=social-history&patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Observation?category=social-history&patient=51200&_lastUpdated=>2016-04-16 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

## Problems

| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| FHIR DSTU2 Resource | <http://hl7.org/fhir/condition.html#resource>                    |


##### On *first-connection*
    GET /Condition?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Condition?patient=51200 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

##### On *periodic-update*
    GET /Condition?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Condition?patient=51200&_lastUpdated=>2016-04-17T04:00:00 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```


## Medications and allergies
| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| FHIR DSTU2 Resource | <http://hl7.org/fhir/medicationstatement.html#resource> <http://hl7.org/fhir/medicationorder.html#resource> <http://hl7.org/fhir/medicationdispense.html#resource> <http://hl7.org/fhir/medicationadministration.html#resource>    <http://hl7.org/fhir/allergyintolerance.html#resource>                    |


##### On *first-connection*
    GET /MedicationOrder?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/MedicationOrder?patient=51200&_count=1 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

    GET /MedicationStatement?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/MedicationStatement?patient=51200&_count=1 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

    GET /MedicationDispense?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/MedicationDispense?patient=51200&_count=1 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

    GET /MedicationAdministration?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/MedicationAdministration?patient=51200&_count=1 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

    GET /AllergyIntolerance?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/AllergyIntolerance?patient=51200&_count=1 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

##### On *periodic-update*
    GET /MedicationOrder?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}
    GET /MedicationStatement?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}
    GET /MedicationDispense?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}
    GET /MedicationAdministration?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}
    GET /AllergyIntolerance?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}}


## Lab results


| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| FHIR DSTU2 Resource | <https://hl7.org/fhir/observation.html#resource>                   |

##### On *first-connection*
    GET /Observation?category=laboratory?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Observation?category=laboratory&patient=51200&_count=5 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

##### On *periodic-update*
    GET /Observation?category=laboratory?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}

## Vital signs ([MU CCDS #13](https://www.healthit.gov/sites/default/files/2015Ed_CCG_CCDS.pdf))


| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| Argonaut Guide      | <http://argonautwiki.hl7.org/index.php?title=Vital_Signs> |
| FHIR DSTU2 Resource | <https://hl7.org/fhir/observation.html#resource>                   |


##### On *first-connection*
    GET /Observation?category=vital-signs?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Observation?category=vital-signs&patient=51200&_count=5 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

##### On *periodic-update*
    GET /Observation?category=vital-signs?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}

## Procedures

| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| FHIR DSTU2 Resource | <https://hl7.org/fhir/procedure.html#resource>                   |


##### On *first-connection*
    GET /Procedure?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Procedure?patient=51200 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

##### On *periodic-update*
    GET /Procedure?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}}

## Immunizations ([MU CCDS #17](https://www.healthit.gov/sites/default/files/2015Ed_CCG_CCDS.pdf))

| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| Argonaut Guide      | <http://argonautwiki.hl7.org/index.php?title=Immunizations> |
| FHIR DSTU2 Resource | <https://hl7.org/fhir/immunization.html#resource>                   |

##### On *first-connection*
    GET /Immunization?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/Immunization?patient=51200 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```

##### On *periodic-update*
    GET /Immunization?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %}

## Patient documents

That is: whatever is available for portal download — not a CCDS requirement


| Details             | URL                                                 |
|---------------------|-----------------------------------------------------|
| Argonaut Guide      | <http://argonautwiki.hl7.org/index.php?title=Argonaut_Document_Access> |
| FHIR DSTU2 Resource | <https://hl7.org/fhir/documentreference.html#resource>                   |


##### On *first-connection*
    GET /DocumentReference?patient={% raw %}{{patientId}}{% endraw %}

###### Request

```HTTP
GET /baseDstu2/DocumentReference?patient=51200 HTTP/1.1
Host: api.fleetap.io
Accept: application/json+fhir
```

###### Response

```JSON
```


## Authorization

As part of Sync for Science<sup>TM</sup> we implement support for the OAuth2-based [SMART on FHIR authorization specification](http://docs.smarthealthit.org/authorization). But we don't need all the moving parts. In particular, we can get away with a minimum of:

1. "**confidential clients**", meaning that apps get assigned a `client_id` and `client_secret` to authenticate to EHRs.The general SMART and Argonaut specs also require support for "public clients", but it's not strictly a requirement in S4S.

2.  "**standalone launch**" flow, meaning that the patient (research participant) can begin by interacting with the PMI app, and from there, can launch into an "connect to my EHR" workflow. The general SMART and Argonaut specs also require support for the "EHR launch flow" (where apps launch from an EHR or portal), but it's not strictly a requirement in S4S.

3.  **`patient/*.read launch/patient offline_access`** scopes, meaning that when the app launches, it will ask the EHR for permission to read all data available to the patient, and it will ask to learn the FHIR id of the patient whose records are shared.

## Server metadata

Return a FHIR conformance statement with [SMART extensions for OAuth](http://docs.smarthealthit.org/authorization/conformance-statement/)

    GET /metadata

Also refer to regarding "request times":

 * `lastCheck` indiclates a FHIR `instant`, with millisecond-level precision including a timezone. For example, `2016-04-01T02:52:32.000Z`

 * *first-connection* for broad queries that the app will make once, after first approval, to back-fill historical data
 * *periodic-update* for narrow queries the app will make frequently (e.g. weekly)

(Note: a production-quality app might repeat the "broad" queries on an occasional basis to discover data that may have fallen _out_ of the record.)

##### On *periodic-update*
    GET /DocumentReference?patient={% raw %}{{patientId}}{% endraw %}&_lastUpdated=>{% raw %}{{lastCheck}}{% endraw %
