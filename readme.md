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
- non_interaction
- result
- version

**Nullable Properties**
If one of the properties below is not required for an event it must still be sent with a null value (not as a string!)

| | start_state | end_state | search_properties | media_type | media_interaction_type |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| screen_view |  |  |  |  |  |
| hyperlink_click |  |  |  |  |  |
| button_click |  |  |  |  |  |
| radio_select | x | x |  |  |  |
| checkbox_select | x | x |  |  |  |
| switch_select | x | x |  |  |  |
| slider_adjust |  |  |  |  |  |
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
| search_filter | x | x |  |  |  |
| search_result_select |  |  |  |  |  |
| media_interaction |  |  |  | x | x |
| input_exit | x | x |  |  |  |

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

# Examples & Values
Many values within the schemas can be considered as suggestions, with many more properties or values that can be added. Concrete examples of various component implementations can be found in the /_examples directory.

# More Information

## Event Core: `result`
Many teams struggle to understand how to implement the `result` property. Seen from a user flow perspective, there are only four possible outcomes when interacting with an element on the page: the user continues their journey, their journey is interrupted, they complete or fail a significant milestone. These for values are represented in the schema as `continue`, `exit`, `success` and `failure`. It is up to the teams to decide _how_ to implement this property but the general recommendation is: determine your significant milestones of your product and for those, define what success looks like (this is a necessary step for data modeling later anyhow). A large percentage of events will have the `continue` value by default.

## Event Core: `search_properties`
Search is possibly one of the more difficult aspects of the Data Library to implement. Since many search modals have various levels of filters and adjustments, the list of properties can become very long, it's recommended to try and standardise the search properties across all implementations of search in your product. For example, if you have a store selling lights and lightbulbs and two different search modules (one for each), it makes sense to have a `wattage` property for both, rather than adding `max_wattage` for lights and `output_wattage` for lightbulbs.

## Contexts: Entities & Business Objects
Semantically, these two contexts will be the most important for your event data. Without these 2 you might as well just be implementing counters of how often users click buttons! For data modeling further downstream, you need to understand what the button _is_, not only that it's a button. This is where these two contexts come in. 

When implementing the Entities, think about how your product is built. Realms are domains of interest, which can be considered a high-level categorisation of your business sectors. For a B2C SaaS selling furniture, accessories and other services for rental apartments, the main realms may be considered as `products` and `services`. 

For the Business Objects, assess what types of information you are offering on your platform. Obviously in an e-commerce situation you will have a `products` object, but think deeper about what other types of objects you may have such as `order`, `support_case` or `faq_item`. A semantic tagging of your business objects will allow you to more easily and fully model and understand exactly what actions are being taken by your users.