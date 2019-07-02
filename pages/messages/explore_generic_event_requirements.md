---
title: Generic Event Message Requirements
keywords:  messaging, bundles
tags: [fhir,messaging]
sidebar: overview_sidebar
permalink: explore_genreic_event_requirements.html
summary: "These are a set of generic requirements which are applicable to all evet messages passing through the NEMS"
---

# Event Message Structure

Each event message which passes through the NEMS will carry a standard set of event information to allow the receiver to identify:
- the the patient who is the focus of the event
- information about the provider who published the event message, including [contact details](overview_msg_architecture_feedback.html) for issues with the event message
- information about the event that occurred
- information to allow receivers to perform [message sequencing](overview_msg_architecture_sequencing.html)


This page provides common FHIR resource population requirements for all event messages, which should be followed in addition to the requirements outlined in the individual [event message specific](overview_supported_events.html) guidance pages.
 
All event messages will be wrapped in a FHIR bundle resource of type `message` and therefore will also include a `MessageHeader` resource as the first resource in the bundle.



# Resource Population

## [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle)

The event Bundle resource which contains the event message resources SHALL conform to the [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle) base FHIR profile and be of type `message`.

This follows the [HL7 FHIR specification](http://hl7.org/fhir/bundle.html#message) for the Bundle resource when being used for messaging, which states that 'A message bundle (type = "message") consists of a series of entries, the first of which is a MessageHeader.'


## [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1)

The MessageHeader resource included as part of the event message SHALL conform to the [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1) constrained FHIR profile and the additional population guidance as per the table bellow:

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| meta.versionId | 0..1 | **Message Sequencing** - A sequence number for the purpose of ordering messages for processing. The sequence number must be an integer which is patient and event-type specific and the publisher must increment the sequence number each time a new event of the same type is issued by the same system for the same patient. |
| meta.lastUpdated | 0..1 | **Message Sequencing** - A FHIR instant (time stamp with sub-second accuracy) which represents the point in time that the change occurred which should be used for ordering messages for processing. |
| extension(eventMessageType) | 1..1 | The type value which shall appear in this element will be defined within the separate event message implementation guide for each of the event messages, as the value will depend on the life cycle of the specific event message. |
| id | 1..1 | An originator/publisher unique publication reference, which will use a UUID format |
| event | 1..1 | **Event Type** - The type of event as specified within the event message implementation guides, e.g. PDS Birth Notification, Failsafe Alert |
| source | 1..1 | The IT system which holds the information that originated the event |
| source.name | 1..1 | A human readable name for the IT system which holds the information that originated the event |
| source.contact | 1..1 | The email address or telephone number to be used by subscribers to contact the publisher for any issues with event message. Additional requirements and information available on the [Event Feedback Mechanism](overview_msg_architecture_feedback.html) page |
| source.contact.system | 1..1 | Must contain a value of `phone` or `email` matching the included contact method within the `value` element |
| source.contact.value | 1..1 | A phone number or email address |
| responsible | 1..1 | A reference to the organization resource which represents the organization responsible for the event. |
| focus | 1..1 | The focus element will reference a resource as defined by the event message specific implementation guide for each specific event message. The referenced resource will contain a `subject` element pointing to a Patient resource which represents the patient who is the focus of the event message.  |


## [CareConnect-Patient-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1)

The patient resource included in the event message SHALL conform to the [CareConnect-Patient-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1) constrained FHIR profile and the additional population guidance as per the table below:

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| identifier | 1..1 | Patient NHS Number SHALL be included within the `nhsNumber` identifier slice |
| name (official) | 1..1 | Patients name as registered on PDS, included within the resource as the `official` name element slice |
| birthDate | 1..1 | The patient birth date shall be included in the patient resource |
| address | 0..* | If an address is included in the patient resource the publisher **SHALL** included as a minimum the `text` element containing the full address. The address SHOULD also be included as structured data if all elements of the address can be populated, a minimum of the `line` and `postalCode` elements are required. |


## [CareConnect-Organization-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1)

Multiple organisations may be referenced from the event message. Organisations are included to:

- to convey the service provider which originated the event
- to identify the publisher of the event

### Referencing the ODS API

When referencing an organisation from an event message, the publisher constructing the event message SHOULD use an absolute URL reference to an Organization resource stored on the Spine directory, which can be retrieved as described in the [FHIR ODS Lookup API Implementation guide](https://developer.nhs.uk/apis/ods/restfulapis_identification_organization.html), rather than including the Organization resource within the message bundle.

The benefit of referencing the organisation resource on the Spine directory is that the information relating to that organisation such as contact details will be kept up to date. If the organisation information is included in the event message bundle as a resource the details for that organisation will become out of date.

Within the resource referencing out to the Organization resource, the `Reference` type element SHALL include:

- the `reference` with the absolute url to the Organization on the ODS API
- the `display` element within the reference with a human readable string representation of the organisation which subscribers can use if they with to display the organisation to a user


### Including an Organization resource

Where there is reason to include the Organization resource within the message bundle the following population requirements SHALL be followed:

The organization resources included in the event message SHALL conform to the [CareConnect-Organization-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1) constrained FHIR profile and the additional population guidance as per the table below:

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| name | 1..1 | A human readable name for the organization SHALL be included in the organization resource |
| identifier | 1..1 | The organization ODS code SHALL be included within the `odsOrganizationCode` identifier slice |
| telecom | 1..* | The organization resource SHALL include a contact number for the organization |