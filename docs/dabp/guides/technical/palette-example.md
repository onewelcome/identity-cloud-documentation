Please take a look at the example palette file with comments about elements certain entries influence.
This file shows just most common customization options. DABP uses Material UI themes, please check out [Material UI palette docs](https://mui.com/customization/palette/) to learn more.
```json
{
  "common": {
    //color of the separator line between headers and data of within tables
    "black": "#eeeee",
    //color of the top bar where logo and profile buttons are placed
    "appBarBackground": "#000000",
    //color of the profile icon on the top bar
    "profileIcon": "#004312",
    //color of the shadow when hover over profile icon on the top bar
    "proleIconHover": "#fffff"
  },
  "background": {
    //background color for 'paper' type components: menu, tables, modal dialogs
    "paper": "rgba(255, 255, 255, 1)",
    //background color for all other components
    "default": "rgba(250, 250, 250, 1)"
  },
  "primary": {
    //color of the clickable elemnts, which are considered primary or `call-to-action`(buttons, links etc)
    "main": "#32912f"
  },
  "secondary": {
    //color of the clickable elemnts which are considered secondary (delete buttons on lists, toggles etc)
    "main": "rgba(235, 95, 2, 1)"
  },
  "error": {
    //color of the inline errors in forms when validation fails
    "main": "rgba(0, 0, 0, 1)"
  },
  "text": {
    //primary color of text
    "primary": "rgba(0, 0, 0, 1)",
    //secondary color of text, used in form placeholders, tooltips, table sorting etc
    "secondary": "rgba(0, 0, 0, 0.75)",
    //color used for hint texts for empty tables
    "hint": "rgba(0, 0, 0, 0.5)"
  }
}
```
