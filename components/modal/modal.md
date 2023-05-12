# Data Library Component: Modal

Modals are elements that visually display over other screens and (in some cases) prevent the user from continuing until an action is successful. 

**N.B.** Unlike Cards, interactions within modals themselves use the Modal Select event and will additionally also trigger the individual component’s type. 

*e.g. Clicking a checkbox within a modal will trigger BOTH a Modal Select & Checkbox Select event.*

# Component Events
## Modal Display
When the modal is shown (within view).

## Modal Dismiss
When the modal is hidden from the user, with or without user input.

## Modal Select
When the modal is interacted with by the user.