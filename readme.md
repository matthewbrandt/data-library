# Welcome the Data Library!
## Introduction
The goal of any kind of frontend tracking is to capture behavioural data in a way that is clear, comprehensive, and actionable. The data captured must be able to provide answers to a wide range of business questions.

In order to design such a tracking system, we have to start by understanding what are the behaviours we want to capture, which means understanding the user experience of the service in question.

## Core Concept
This concept revolves around a core set of interactions that are possible on different types of UX elements. These given elements are defined as Components in our Data Library and on these elements, certain events are possible, according to our pre-defined specification.

## Reasons for Doing
- reduction of efforts in development for tracking specifications, implementing tracking and testing due to the "one-shot" implementation strategy

- ensure a better alignment of data collection to the systems that are optimised for them due to a tool-agnostic strategy

- increase data quality through a systematic approach and more consistent implementation

- guarantee principles of data protection and privacy by design (e.g. by preventing leakage of personal data)

# Semantics
## Syntax
The schemas are defined in JSON. The reasons for doing this are because JSON:

- …is a universal standard in programming
- …allows for nesting of keys which automatically forms a relationship that we do not need to explicitly describe
- …can be used across varying systems and is not vendor-specific
- …syntax is relatively easy to read
- …does not pre-determine how data should be stored

## Format
All event properties and values use `snake_case` and all values are always **lowercased**. All values are sent in English (although translation keys can additionally be used if needed).

## Stateless
All events and values within are stateless; they neither refer to other events that already happened nor do the values reflect any other state than what is currently known at the time of the event occurring.

# Specification
The specification consists of three main parts:

**1. Data Library Components & Events**

A component consists of a UX element that is visible in the frontend. The components are grouped as sensibly as possible to avoid a large amount of similar components. Events that are possible on the component are listed as separate unique events.

**2. Event Core**

The event core covers the information of what event was triggered, such as which UX component it was and whether it was triggered by a click or other type of interaction.

**3. Event Contexts**

As part of the event, additional information can be sent about things related to the event, such as user identity, region and language settings, etc.

## Global Event Schema
**The Event Core and Event Contexts are covered under the Global Event Schema.**

The schema is a "contract" that must be upheld and consists of properties and their acceptable values. Each event must adhere to the schema. There are a set of event properties that are non-nullable and some that are dependent on which event was sent. 

The event schema stored in this repository covers all possible properties and acceptable values.

## Data Library Components & Events
Each component is visible under the relevant components subdirectory, e.g. /components/modal. The possible events are defined individually and have their own individual readme & JSON files (which adhere to the Global Event Schema) but offer examples and more detailed information about each event.

## Event Contexts
Each Event Context is defined within the /_contexts directory as it's own individual JSON file. 
> **N.B.**: An event will always be sent with both the Event Core as well as the Event Contexts but depending on which analytics system you are sending the information too, the syntax may need to be adapted.

# Examples
Concrete examples of various component implementations can be found in the /_examples directory.