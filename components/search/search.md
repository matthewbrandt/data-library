# Data Library Component: Search

A search component is used when information can be filtered or found using a search bar or filters.

**Interactions within Search only triggers events from the Search component and not from other components.**

# Component Events
## Search Initiate
When the triggers a search with any or no filters applied. 
In a text field with instant search, it is recommended to only trigger this event for words with >= 3 letters (for Latin languages).

## Search Filter
When the user adjusts any element from the filter options default values provided (e.g. moves the distance slider from default 10km to 30km).

## Search Result Select
When the user selects any element of the search results list.