---
title: Subscriptions Overview
keywords:  messaging, subscriptions
tags: [fhir,messaging,subscriptions]
sidebar: overview_sidebar
permalink: explore_subscriptions.html
summary: "Types of Subscription and how they are used"
---

To receive event messages a consumer will need to subscribe to events they want to receive. The flow of event messages from publishers to subscribers is described on the [Messaging Architecture Overview](explore_msg_architecture_overview.html) page.


## Types of Subscription ##

### Explicit Subscriptions ###

An explicit subscription relates to where a subscriber wishes to receive event messages for a specific patient, for example, a Pharmacist wishing to receive hospital admission and discharge events for a specific Patient.

Explicit event message subscriptions for patients can be created using the Subscription API, using the [Create Subscription](explore_create_subscription.html) interaction. The organizations MESH mailbox must be configured to receive the required event message types as per the requirements on the [Event Receiver Requirements](receiver_requirements.html#mesh-mailbox-configuration) page.

Explicit subscriptions SHOULD only be active for patients under the subscribing organisations direct care. Explicit subscriptions for a patient should be stopped when the patient leaves the subscribing organisations direct care. This can be done either by removing the explicit subscription using the Delete Subscription API interaction or by including the `end` element as part of the subscription resource.

### Rule-Based (Generic) Subscriptions ###

A rule-based subscription relates to where a subscriber wishes to receive all published events that meet a particular rule set. There are two specific types of rule-based subscription currently:

- Geographical: Subscriptions that relate to individuals who reside within the geographic boundaries of a specific organisation (For example, a Health Visiting Service wishing to view events for all children within a specific local authority area). 
- Registered Org: Subscriptions that relate to individuals who are registered with a specific organisation (currently only applicable for GP organisations).

Currently the Subscription API does not support the configuration of generic subscriptions. Details of any generic subscriptions required should be appended to the service request required to configure the MESH mailbox to receive event messages. This process is outlined on the [Event Receiver Requirements](receiver_requirements.html#mesh-mailbox-configuration) page.


## Subscription matching and message delivery ##

### Multiple matched subscriptions ###

A subscriber may create a number of different subscriptions, some explicit and some generic. If an event message published to the National Events Management Service (NEMS) matches the criteria of multiple subscriptions for a single receiving MESH mailbox, the NEMS will only send one copy of the event message to the receiving mailbox.

{% include important.html content="If a publisher sends multiple copies of the same event message, subscribers of this event will receive a copy of the event message for each repeated send by the publisher." %}