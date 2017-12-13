---
title: PDS Related Person
keywords:  messaging, bundles
tags: [fhir,messaging]
sidebar: foundations_sidebar
permalink: explore_pds_related_person.html
summary: "The FHIR profiles used for the PDS Related Person event message bundle"
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide the FHIR messaging components for the Events Management Service. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis. More detail is given in [Guide Versioning](/overview_guide_versioning.html). It is advised not to develop against these specifications until a formal announcement has been made." %}

## PDS Related Person event message bundle ##

The PDS Related Person event message bundle is expected to include a combination of the following resources to support the event header and data item requirements:

| PDS Related Person Event Message Bundle |
|-----------------------------------------|
| EMS-Bundle                              |
| EMS-MessageHeader-1                     |
| CareConnect-Organization-1              |
| EMS-HealthcareService-1                 |
| CareConnect-Patient-1                   |
| EMS-Communication-1                     |
| PDS-RelatedPerson-1                     |

The data item requirements are expected to be fulfilled as below:

| PDS Related Person data item name              | FHIR Resource       | FHIR element                                     | Mandatory/Optional/Required |
|------------------------------------------------|---------------------|--------------------------------------------------|-----------------------------|
| Related Person Role                            | PDS-RelatedPerson-1 | category                                         | Mandatory                   |
| Relationship type                              | PDS-RelatedPerson-1 | relationship                                     | Mandatory                   |
| NHS Number                                     | PDS-RelatedPerson-1 | identifier using NHSNumber slice                 | Required                    |
| Contact ranking                                | PDS-RelatedPerson-1 | extension contactRanking                         | Required                    |
| Next of Kin indicator                          | PDS-RelatedPerson-1 | extension nextOfKinIndicator                     | Mandatory                   |
| Copy correspondence indicator                  | PDS-RelatedPerson-1 | extension copyCorrespondenceIndicator            | Required                    |
| Call centre call back consent                  | PDS-RelatedPerson-1 | extension callCentreCallbackConsent              | Required                    |
| Relationship Business Effective from date      | PDS-RelatedPerson-1 | period.start                                     | Mandatory                   |
| Relationship Business Effective to date        | PDS-RelatedPerson-1 | period.end                                       | Required                    |
| Name type                                      | PDS-RelatedPerson-1 | name.use                                         | Mandatory                   |
| Family name                                    | PDS-RelatedPerson-1 | name.family                                      | Mandatory                   |
| First given name                               | PDS-RelatedPerson-1 | name.given                                       | Required                    |
| Other given name(s)                            | PDS-RelatedPerson-1 | name.given                                       | Required                    |
| Name prefix                                    | PDS-RelatedPerson-1 | name.prefix                                      | Required                    |
| Name suffix                                    | PDS-RelatedPerson-1 | name.suffix                                      | Required                    |
| Address type                                   | PDS-RelatedPerson-1 | address.use                                      | Required                    |
| Address line                                   | PDS-RelatedPerson-1 | address.line                                     | Required                    |
| Postcode                                       | PDS-RelatedPerson-1 | address.postalCode                               | Required                    |
| PAF Key                                        |                     | not yet profiled                                 | Required                    |
| Address Business Effective from date           | PDS-RelatedPerson-1 | address.period.start                             | Mandatory                   |
| Address Business Effective to date             | PDS-RelatedPerson-1 | address.period.end                               | Required                    |
| Telecom usage                                  | PDS-RelatedPerson-1 | telecom.use                                      | Required                    |
| Communication contact method                   | PDS-RelatedPerson-1 | telecom.system                                   | Required                    |
| Communication contact string                   | PDS-RelatedPerson-1 | telecom.value                                    | Required                    |
| Telecommunication Business Effective from date | PDS-RelatedPerson-1 | telecom.period.start                             | Mandatory                   |
| Telecommunication Business Effective to date   | PDS-RelatedPerson-1 | telecom.period.end                               | Required                    |
| Language                                       | PDS-RelatedPerson-1 | extension nHSCommunication.language              | Mandatory                   |
| Interpreter required indicator                 | PDS-RelatedPerson-1 | extension relatedPersonCommunication.interpreter | Mandatory                   |
| Preferred contact method                       | PDS-RelatedPerson-1 | extension.preferredContactMethod                 | Mandatory                   |
| Preferred contact times                        | PDS-RelatedPerson-1 | extension.preferredContactTimes                  | Required                    |
| Preferred written communication format         | PDS-RelatedPerson-1 | extension.preferredWrittenCoomunicationFormat    | Required                    |










