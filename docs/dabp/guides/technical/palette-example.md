# Example palette.json for DABP

DABP uses Material UI themes for its styling. Check out the [Material UI palette docs](https://mui.com/customization/palette/) to learn
more. The only customisations allowed are changing colours.

The example below shows the most common customisation options. Comments are added to explain where the colour is used.

```json
{
  "common": {
    //the colour of the separator line between the headers and the data of a table
    "black": "#eeeee",
    //the background colour of the top bar with the logo and the profile buttons.
    "appBarBackground": "#000000",
    // the colour of the profile icon on the top bar
    "profileIcon": "#004312",
    //the background colour of when hover over profile icon on the top bar
    "proleIconHover": "#fffff"
  },
  "background": {
    //the background colour for 'paper' type components: menu, tables, modal dialogs
    "paper": "rgba(255, 255, 255, 1)",
    //the background colour for all other components
    "default": "rgba(250, 250, 250, 1)"
  },
  "primary": {
    //the colour of the clickable elements, which are considered primary or `call-to-action`(buttons, links etc)
    "main": "#32912f"
  },
  "secondary": {
    //the colour of the clickable elements which are considered secondary (delete buttons on lists, toggles etc)
    "main": "rgba(235, 95, 2, 1)"
  },
  "error": {
    //the colour of the inline errors in forms when validation fails
    "main": "rgba(0, 0, 0, 1)"
  },
  "text": {
    //the primary colour of text
    "primary": "rgba(0, 0, 0, 1)",
    //the secondary colour of text, used in form placeholders, tooltips, table sorting etc
    "secondary": "rgba(0, 0, 0, 0.75)",
    //colour used for hint texts for empty tables
    "hint": "rgba(0, 0, 0, 0.5)"
  }
}
```
