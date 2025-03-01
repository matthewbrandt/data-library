# Welcome the Data Library!
## Introduction
The goal of any kind of frontend tracking is to capture behavioural data in a way that is clear, comprehensive, and actionable. The data captured must be able to provide answers to a wide range of business questions.

In order to design such a tracking system, we have to start by understanding what are the behaviours we want to capture, which means understanding the user experience of the service in question.

## Core Concept
This concept revolves around a core set of interactions that are possible on different types of UX elements. These given elements are defined as Components in our Data Library and upon these elements, certain interactions (events) are possible, according to our pre-defined specification.

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

**Non-Nullable Properties**

The following properties must ALWAYS be sent with a value that is not null:

- action
- label
- component
- component_event

**Nullable Properties**
If one of the properties below is not required for an event it does not need to be declared in the event.

| | start_state | end_state | options | media_type | media_interaction_type |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| screen_view |  |  |  |  |  |
| hyperlink_select |  |  |  |  |  |
| button_select |  |  |  |  |  |
| radio_select | x | x |  |  |  |
| checkbox_select | x | x |  |  |  |
| switch_select | x | x |  |  |  |
| slider_adjust | x | x |  |  |  |
| card_select |  |  |  |  |  |
| card_flip |  |  |  |  |  |
| card_state | x | x |  |  |  |
| notification_display |  |  |  |  |  |
| notification_dismiss |  |  |  |  |  |
| notification_select |  |  |  |  |  |
| modal_display |  |  |  |  |  |
| modal_dismiss |  |  |  |  |  |
| modal_select |  |  |  |  |  |
| navigation_select |  |  |  |  |  |
| navigation_state | x | x |  |  |  |
| list_select |  |  |  |  |  |
| list_state | x | x |  |  |  |
| search_initiate |  |  | x |  |  |
| search_filter |  |  |  |  |  |
| search_result_select |  |  | x |  |  |
| media_interaction |  |  |  | x | x |
| input_exit | x | x |  |  |  |

**3. Event Contexts**

As part of the event, additional information can be sent about things related to the event, such as user identity, region and language settings, etc. 

**N.B.** All Event Contexts must be sent with every event - the information within is validated against the schema as part of event processing. Bad values or events will be marked or discarded as necessary.

## Global Event Schema
**The Event Core and Event Contexts are covered under the Global Event Schema.**

The schema is a "contract" that must be upheld and consists of properties and their acceptable values. Each event must adhere to the schema. There are a set of event properties that are non-nullable and some that are dependent on which event was sent. 

The event schema stored in this repository covers all possible properties and acceptable values.

## Data Library Components & Events
Each component is visible under the relevant components subdirectory, e.g. /components/modal. The possible events are defined individually and have their own individual readme & JSON files (which adhere to the Global Event Schema) but offer examples and more detailed information about each event.

## Event Contexts
Each Event Context is defined within the /_contexts directory as it's own individual JSON file. 
> **N.B.**: An event will always be sent with both the Event Core as well as the Event Contexts but depending on which analytics system you are sending the information too, the syntax may need to be adapted.

# Examples & Values
Many values within the schemas can be considered as suggestions, with many more properties or values that can be added. Concrete examples of various component implementations can be found in the /_examples directory.

# More Information

## Event Core: `intent`
Many teams struggle to understand how to implement the `intent` property. Seen from a user flow perspective, there are only a limited amount of possible outcomes when interacting with an element on the page. These for values are represented in the schema through the values `camera`,`microphone`, `location`, `map`, etc. A large percentage of events will have the `navigation` value by default. This property _can_ be constructed late-join, but it is highly recommended to implement it at the time of collection.

## Event Core: `options`
Filtering (search) is possibly one of the more difficult aspects of the Data Library to implement. Since many search modals have various levels of filters and adjustments, the list of properties can become very long, it's recommended to try and standardise the search properties across all implementations of search in your product. For example, if you have a store selling lights and lightbulbs, with two different search modules (one for each), it makes sense to have a `wattage` property for both, rather than adding `max_wattage` for lights and `output_wattage` for lightbulbs.

In addition, the `options` parameter can be used to attach more specific items that don't conform to the event core model, but don't belong inside of it's own schema due to being about the event itself (rather than its meaning).

## Contexts: Entities & Business Objects
Semantically, these two contexts will be the most important for your event data. Without these 2 you might as well just be implementing counters of how often users click buttons! For data modeling further downstream, you need to understand what the button _is_, not only that it's a button. This is where these two contexts come in. You can add any additional contexts and properties necessary for your business.

### Entities
When implementing the Entities, think about how your product is built. The classification is based on your operating business model(s) and company type. For a B2C SaaS selling furniture, accessories and other services for rental apartments, the realms may be named `products` and `services`. 

### Business Objects
For the Business Objects, assess what types of information you are offering on your platform. Obviously in an e-commerce situation you will have a `products` object, but think deeper about what other types of objects you may have such as `order`, `support_case` or `faq_item`. A semantic tagging of your business objects will allow you to more easily and fully model and understand exactly what actions are being taken by your users.