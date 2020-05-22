# Console Commands

Start in version 0.6.0 there are a series of console commands which are accessed in a variety of ways:
* Terminal inside MeerK40t accepts console commands.
* Batch files accepted with the -b command in the command line.
* Keymap keys run commands which are by default console commands.

## Commands

### Jogs

* right <length>
* left <length>
* up <length>
* down <length>

These commands accept a distance and travel that distance. For example:
* `right 1mm` travels 1 mm to the right.
* `down 1in` travels 1 inch down  (+y direction, where +y is towards the bottom)

The distance is evaluated consistent to the SVG Length parsing. Which makes valid: `%`, `px`, `cm`, `mm`, `in`, `pt`, and `pc`.

The percent is percent across the bed set on the device. So right 10% moves 10% of your bed's total width to the right.

`move <x> <y>` goes to an absolute location.

* `move 1in 1in` goes to the location 1in 1in off the upper lefthand corner.
* `move 50% 50%` goes to the location exactly in the middle of your bed.
* `move 20cm 5in` goes to the location 20cm in the x and 5 inches down in the y.

### Misc Lasercutter

* `home` performs a homing operation 

---

* `unlock` performs an unlock rail operation.

---

* `speed [<value>]`

Speed sets the speed in mm/s. This speed is used for any particular cuts done at speed.


* `power [<value>]`

Power sets the power in ppi. 0 - 1000. This power setting in used for any cuts performed.

### Loop / End

The loop and end commands set a recurrent operation. These are performed repeatedly while `loop` is called but `end` is not.


* `loop <command>`
* `end <command>`

`loop right 1mm` shall call `right 1mm` 20 times a second until it's stopped this could be done with either `end right 1mm` or just `end` which would stop all commands. This also serves as the basis for several of the `+` prefixed commands which are prebuilt aliases.

* `+scale_up` = "loop scale 1.02"
* `-scale_up` = "end scale 1.02"
* `+scale_down` = "loop scale 0.98"
* `-scale_down` = "end scale 0.98"
* `+rotate_cw` = "loop rotate 2"
* `-rotate_cw` = "end rotate 2"
* `+rotate_ccw` = "loop rotate -2"
* `-rotate_ccw` = "end rotate -2"
* `+translate_right` = "loop translate 1mm 0"
* `-translate_right` = "end translate 1mm 0"
* `+translate_left` = "loop translate -1mm 0"
* `-translate_left` = "end translate -1mm 0"
* `+translate_down` = "loop translate 0 1mm"
* `-translate_down` = "end translate 0 1mm"
* `+translate_up` = "loop translate 0 -1mm"
* `-translate_up` = "end translate 0 -1mm"
* `+right` = "loop right 1mm"
* `-right` = "end right 1mm"
* `+left` = "loop left 1mm"
* `-left` = "end left 1mm"
* `+up` = "loop up 1mm"
* `-up` = "end up 1mm"
* `+down` = "loop down 1mm"
* `-down` = "end down 1mm"

### Device and Settings

* `device [<value>]`
* `set [<key> <value>]`
* `window [(open|close) <window_name>]`
* `control [<executive>]`
* `module [(open|close) <module_name>]`
* `schedule`
* `channel [(open|close) <channel_name>]`

### Elements

* `element [<element>]*`
* `path <svg_path>`
* `circle <cx> <cy> <r>`
* `ellipse <cx> <cy> <rx> <ry>`
* `rect <x> <y> <width> <height>`
* `text <text>`
* `polygon [<x> <y>]*`
* `polyline [<x> <y>]*`
* `group`
* `ungroup`
* `stroke <color>`
* `fill <color>`
* `rotate <angle>`
* `scale <scale> [<scale_y>]`
* `translate <translate_x> <translate_y>`

### Operations

* `operation [<element>]*`
* `classify`
* `cut`
* `engrave`
* `raster`

### Binds / Aliases

* `bind [<key> <command>]`
* `alias [<alias> <command>]`

### Servers

* `ruidaserver`
* `grblserver`

### Misc

* `refresh`