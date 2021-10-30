Choices are properly a `list` of `dict` object. Stored in the kernel. These are added with `add_choices()` these help provide a unified location for displaying and controlling of these options.

## Example:

```python
            {
                "attr": "opt_rapid_between",
                "object": context,
                "default": True,
                "type": bool,
                "label": _("Rapid Moves Between Objects"),
                "tip": _(
                    "Travel between objects (laser off) at the default/rapid speed rather than at the current laser-on speed"
                ),
            },
```

These have defined "attr" values and if the "object" for this "attr" is a `Context` type the kernel automatically calls the `.settings()` command for this using the "type", "attr", and "default" values to ensure these choices are initialized. So if a choices is registered at during the "boot" lifecycle (which is after the persistent settings have been loaded), any value registered within a choices will be declared.

The definitions here declare all the different parts that are used, we should declare `attr`, `object`, `default`, and `type` and `label`. Though there are additional values such as `submenu` and `tip`, these declare a submenu of this choice and a tooltip respectively.

## Properties Panel

The magic here is to define a more abstract definition of various options used in meerk40t. This allows the options to be called up dynamically. So, for example, PropertiesPanel in `meerk40t/gui/propertiespanel` serves as a generic displayer of these choices. This is done by giving the registered "choice" value it was defined with or by passing the list of dictionary object. This class creates a vertical list of properties in a panel, and displays the registered editor for each of these.

This will also, in the future give us the ability to add in listeners for properties changes and update the state of the value. So if you change the state of the value in one location we can have the dirty work of having the correct value updated for us.

### Bool

Booleans data are given a checkbox.

### Int, String, Float

Integers Strings and Floats are given a textbox stored within a Labeled `StaticBoxSizer` with the value's label.

### Color

Color is rendered as a button in a labeled `StaticBoxSizer` that when pressed launches a `wx.ColorDialog` to edit that color.

### Other

If there is a need to add in additional handlers here they should be added to Properties panel.

## Menubar

The Menubar options are also dynamic methods for displaying choices. This is done in `meerk40t/gui/wxutils` `create_menu_for_choices` and takes the `list[dict]` values and gives us a wx.Menu object. If the choices values have a `separate_before` or `submenu` or `action` this is used within the menu in addition to the regular `attr`, `object`, `type`, `label`, `tip` values.

### Bool

Boolean values get a wx.ITEM_CHECK type menu bar item.

### Action

Action types consist of the string "action" for the type property, and a defined `action` parameter. This simply executes the code given to it, when selected from the list. It's possible to define a menubar dynamically with this value.

