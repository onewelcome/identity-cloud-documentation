# Example palette.json for DABP

DABP uses Material UI themes for its styling. Check out the [Material UI palette docs] (https://mui.com/customization/palette/) to learn more. The only customizations allowed are changing colours.

The example below shows the most common customization options. A comment is added to explain where the colour is used.
```json
{
  "common": {
    //the color of the separator line between the headers and the data of a table
    "black": "#eeeee",
    //the background color of the top bar with the logo and the profile buttons.
    "appBarBackground": "#000000",
    // the color of the profile icon on the top bar
    "profileIcon": "#004312",
    //the background color of when hover over profile icon on the top bar
    "proleIconHover": "#fffff"
  },
  "background": {
    //the background color for 'paper' type components: menu, tables, modal dialogs
    "paper": "rgba(255, 255, 255, 1)",
    //the background color for all other components
    "default": "rgba(250, 250, 250, 1)"
  },
  "primary": {
    //the color of the clickable elements, which are considered primary or `call-to-action`(buttons, links etc)
    "main": "#32912f"
  },
  "secondary": {
    //the color of the clickable elements which are considered secondary (delete buttons on lists, toggles etc)
    "main": "rgba(235, 95, 2, 1)"
  },
  "error": {
    //the color of the inline errors in forms when validation fails
    "main": "rgba(0, 0, 0, 1)"
  },
  "text": {
    //the primary color of text
    "primary": "rgba(0, 0, 0, 1)",
    //the secondary color of text, used in form placeholders, tooltips, table sorting etc
    "secondary": "rgba(0, 0, 0, 0.75)",
    //color used for hint texts for empty tables
    "hint": "rgba(0, 0, 0, 0.5)"
  }
}
```
