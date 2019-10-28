---
title: Encounter
keywords:  messaging, bundles
tags: [fhir,messaging]
sidebar: overview_sidebar
permalink: encounter_1.html
summary: "Guidance and requirements for the Encounter event message"
---

The `encounter` event is a event message which represent an encounter between a patient and practitioner. The event message will be used in a number of different care settings for a variety of different use cases, therefore subscribers to this event need to be aware that they will receive encounter events which may not be relevant to their use cases.

## Event Message Content

The `encounter` event is a ["linked information"](overview_msg_architecture_event_content.html) event message, as described on the [Message Content](overview_msg_architecture_event_content.html) page. This means that the event-related information should not be included in the encounter event but rather made available by the publisher through an endpoint URL and the event message will instead contain a pointer which links to that endpoint URL.

Within the encounter event message the pointers to event-related information will be included in the form of `DocumentReference` resources which conform to the pointer model and retrieval mechanism defined by the [National Record Locator](https://developer.nhs.uk/apis/nrl/index.html) service.

All encounter event messages that are published to the NEMS **MUST** be created inline with guidance and requirements specified on this page and on the [Generic Event Message Requirements](explore_genreic_event_requirements.html) page. Programs that choose to use this encounter event will need to define additional requirements and implementation guidance for their publishers, around population of the event message, and outline what their subscribers should look for in the event message to identify if the event message is relevant to their use case.


## Bundle structure

The event message will contain a mandatory `MessageHeader` resource as the first element within the event message bundle as per FHIR messaging requirements. The MessageHeader resource references an `encounter` resource as the focus of the event message. The encounter resource represents the encounter that happened and should be populated as per the guidance below.

Publishers **MUST** include NRL DocumentReference resources pointing to event-related information, where event-related information it is available. Where there is a strong use case for including the event-related information as resources within the event message bundle, it may be include as resources but the event message **MUST** also contain pointers to where the same event-related information can be retrieved.

<div class="tabPanel">

	<div class="tabHeadings">
		<span class="tabHeading" id="logicalModelTab">Logical Model</span>
		<span class="tabHeading" id="fhirModelTab">FHIR Model</span>
	</div>
	
	<div class="tabBodies">
	
		<div class="tabBody" id="logicalModelTabBody">
			The diagram below shows the logical model of data in the event message:
			<div style="text-align:center; margin-bottom:20px" >
				<a href="images/messages/generic_encounter_logical.png" target="_blank"><img src="images/messages/generic_encounter_logical.png"></a>
			</div>
		</div>

		<div class="tabBody" id="fhirModelTabBody">
			The diagram below shows the referencing between FHIR resources within the encounter message bundle:
			<div style="text-align:center; margin-bottom:20px" >
				<a href="images/messages/generic_encounter.png" target="_blank"><img src="images/messages/generic_encounter.png"></a>
			</div>
		</div>
		
	</div>
</div>


## Encounter Message Life Cycle ##

The `Encounter` event message is light weight and carries minimal event-related information so the content of the event message is unlikely to change, but there is still a chance the information within the event message or referenced information will change so the generic encounter event will support use of the `messageEventType` extension within the `MessageHeader` resource to indicate `new`, `update` and `delete` versions of the event message when they are required.

The different types of the encounter event message should be published by publishers as follows:

| Message Event Type | Description |
| --- | --- |
| new | A `new` encounter event should be sent when the encounter occurs. |
| update | An `update` encounter event message should be sent when information related to the encounter event has been updated. |
| delete | A `delete` encounter event message should be used to indicate that the encounter event message should not be used by subscribers, such as when the event sent incorrectly and should not have been sent. |


## Onward Delivery ##

The delivery of the Encounter event messages to subscribers via MESH will use the following `WorkflowID` within the MESH control file. This `WorkflowID` will need to be added to the receiving MESH mailbox configuration before event messages can be received.

| MESH WorkflowID | `ENCOUNTER_1` |


## Resource Population Requirements and Guidance ##

The following requirements and resource population guidance must be followed in addition to the requirements and guidance outlined in the [Generic Requirements](https://developer.nhs.uk/apis/ems-beta/explore_genreic_event_requirements.html) page.


### [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle)

The Bundle resource is the container for the event message and SHALL conform to the [Bundle](http://hl7.org/fhir/STU3/StructureDefinition/Bundle) FHIR profile.

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| type | 1..1 | Fixed value: `message` |


### [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1)

The MessageHeader resource included as part of the event message SHALL conform to the [Event-MessageHeader-1](https://fhir.nhs.uk/STU3/StructureDefinition/Event-MessageHeader-1) constrained FHIR profile and the additional population guidance as per the table below:

| Resource Cardinality | 1..1 |

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| extension(messageEventType) | 1..1 | Value as per the "Encounter Message Life Cycle" table above |
| event | 1..1 | Fixed Value: encounter-1 (Encounter) |
| responsible | 1..1 | This will reference the source organization of the event message. |
| focus | 1..1 | This will reference the "CareConnect-Encounter-1" resource which contains information relating to the encounter that occurred. |


### [CareConnect-Encounter-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Encounter-1)

The Encounter resource included in the event message SHALL conform to the [CareConnect-Encounter-1](https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Encounter-1) constrained FHIR profile and the additional population guidance as per the table below:

| Resource Cardinality | 1..* |

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| period.start | 0..1 | Then encounter event **SHOULD** include the dateTime when the encounter started. |
| period.end | 0..1 | Then encounter event **SHOULD** include the dateTime when the encounter ended if available or calculatable by the publisher. |
| **type** | 0..* | The `type` element within the encounter **SHOULD** be used to identify the type of encounter that took place. |
| participant | 0..* | Publishers **SHOULD** include the participants who were present during the encounter. |


### [DocumentReference](https://fhir.nhs.uk/STU3/StructureDefinition/NRL-DocumentReference-1)

Pointers to endpoint URLs exposed by the publisher, where event-related information can be retrieved by the subscriber **MUST** be included as DocumentReference within the event message.

These DocumentReference resources **MUST** conform to the [National Record Locator (NRL)](https://developer.nhs.uk/apis/nrl/) pointer model. These DocumentReferences may point to event-related information at different levels of granularity, some may point at an encounter as a whole or may point at specific event-related information such as vaccinations or allergies as defined within the NRL specification. Retrieval of this supporting information using the information in the DocumentReference pointer **MUST** use the same mechanisms as required for the [NRL retrieval](https://developer.nhs.uk/apis/nrl/retrieval_overview.html)

The DocumentReference resource(s) included in the event message **MUST** conform to the requirements in the [National Record Locator (NRL)](https://developer.nhs.uk/apis/nrl/) specification and the additional population guidance as per the table below:

| Resource Cardinality | 0..* |

| Element | Cardinality | Additional Guidance |
| --- | --- | --- |
| subject | 1..1 | This will reference the patient resource representing the patient who is the subject of this event. |
| context.encounter | 1..1 | A reference to the encounter this document reference pointer is related to. |


## Examples

[Example 1](pages/messages/examples/encounter_1_1.xml)

[Example 2](pages/messages/examples/encounter_1_2.xml)