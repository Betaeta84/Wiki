# Console Commands

Starting in version 0.6.0 there are a series of console commands which are accessed in a variety of ways:
* Terminal inside MeerK40t accepts console commands.
* Batch files accepted with the -b command in the command line.
* Keymap keys run commands which are by default console commands.

In 0.7.0 this is greatly expanded.


## Commands

## Help in 0.7.0
Currently (3/7/21) the following commands are listed when requesting "help" in 0.7.0
```
help
--- Base Commands ---
abort           Abort Job
acceleration    Set Interpreter Acceleration [1-4]
alias           alias <alias> <console commands[;console command]*>
bind            bind <key> <console command>
camera\d*       camera commands and modifiers.
camwin          camwin <index>: Open camera window at index
cd              change directory
channel         channel [(open|close|save) <channel_name>]
circle          circle <x> <y> <r> or circle <r>
classify        classify elements into operations
clipboard       clipboard
consoleserver   starts a console_server on port 23 (telnet)
context         context
control         control [<executive>]
cut             group current elements into operation type
declassify      declassify selected elements
device          device [<value>]
dir             list directory
down            cmd <amount>
egv             Lhystudios Engrave Code Sender. egv <lhymicro-gl>
egv_export      Lhystudios Engrave Buffer Export. egv_export <egv_file>
egv_import      Lhystudios Engrave Buffer Import. egv_import <egv_file>
element         element, selected elements
element(\d+,?)+ element0,3,4,5: elements 0, 3, 4, 5
element*        element*, all elements
elements        list all elements in console
element~        element~, all non-selected elements
ellipse         ellipse <cx> <cy> <rx> <ry>
embroider       embroider <angle> <distance>
end             end <commmand>
engrave         group current elements into operation type
fill            fill <svg color>
flush           flush
grblserver      activate the grblserver.
grid            grid <columns> <rows> <x_distance> <y_distance>
halftone        image halftone <diameter> <scale> <angle>
hold            Hold Controller
home            home the laser
image           image <operation>
imageop         group current elements into operation type
left            cmd <amount>
line            adds line to scene
load            load file
lock            lock the rail
loop            loop <command>
ls              list directory
matrix          matrix <sx> <kx> <sy> <ky> <tx> <ty>
modifier        modifier [(open|close) <module_name>]
module          module [(open|close) <module_name>]
move            move <x> <y>: move to position.
move_absolute   move <x> <y>: move to position.
move_relative   move_relative <dx> <dy>
note            note <note>
operation       operation: selected operations.
operation(\d+,?)+ operation0,2: operation #0 and #2
operation*      operation*: all operations
operation.*     operation: selected operations
operations      operations: list operations
operation~      operation~: non selected operations.
optimize        optimize <type>
outline         outline the current selected elements
path            path <svg path>
pause           realtime pause/resume of the machine
plan            plan<?> <command>
polygon         polygon (<point>, <point>)*
polyline        polyline (<point>, <point>)*
power           Set Interpreter Power
pulse           pulse <time>: Pulse the laser in place.
quit            quits meerk40t shutting down all processes
raster          group current elements into operation type
rect            adds rectangle to scene
refresh         wxMeerK40 refresh
register        register
reify           reify affine transformations
reset           reset affine transformations
resize          resize <x-pos> <y-pos> <width> <height>
resume          Resume Controller
right           cmd <amount>
rotaryscale     Rotary Scale selected elements
rotaryview      Rotary View of Scene
rotate          rotate <angle>
ruidaserver     activate the ruidaserver.
scale           scale <scale> [<scale-y>]?
schedule        show scheduled events
set             set [<key> <value>]
shutdown        quits meerk40t shutting down all processes
speed           Set Speed in Interpreter.
start           Start Pipe to Controller
stroke          stroke <svg color>
stroke-width    stroke-width <length>
text            text <text>
theme           Theming information and assignments
thread          show threads
timer.*         timer<?> <duration> <iterations>
trace_hull      trace the convex hull of current elements
trace_quick     quick trace the bounding box of current elements
translate       translate <tx> <ty>
tree            access and alter tree elements
unlock          unlock the rail
up              cmd <amount>
usb_connect     Connect USB
usb_disconnect  Disconnect USB
webhelp         Launch a registered webhelp page
window          wxMeerK40 window information
--- camera Commands ---
background      set background image
export          export camera image
fisheye         fisheye (capture|reset)
perspective     perspective (set <#> <value>|reset)
start           Start Camera.
stop            Stop Camera
--- clipboard Commands ---
clear           clipboard clear
contents        clipboard contents
copy            clipboard copy
cut             clipboard cut
list            clipboard list
paste           clipboard paste
--- elements Commands ---
circle          circle <x> <y> <r> or circle <r>
classify        classify elements into operations
clipboard       clipboard
copy            duplicate elements
cut             group current elements into operation type
declassify      declassify selected elements
delete          delete elements
ellipse         ellipse <cx> <cy> <rx> <ry>
engrave         group current elements into operation type
fill            fill <svg color>
grid            grid <columns> <rows> <x_distance> <y_distance>
imageop         group current elements into operation type
line            adds line to scene
matrix          matrix <sx> <kx> <sy> <ky> <tx> <ty>
merge           merge elements
outline         outline the current selected elements
polygon         polygon (<point>, <point>)*
polyline        polyline (<point>, <point>)*
raster          group current elements into operation type
rect            adds rectangle to scene
reify           reify affine transformations
reset           reset affine transformations
resize          resize <x-pos> <y-pos> <width> <height>
rotate          rotate <angle>
scale           scale <scale> [<scale-y>]?
select          Set these values as the selection.
select+         Add the input to the selection
select-         Remove the input data from the selection
select^         Toggle the input data in the selection
step            step <raster-step-size>
stroke          stroke <svg color>
stroke-width    stroke-width <length>
subpath         break elements
text            text <text>
translate       translate <tx> <ty>
--- ops Commands ---
copy            duplicate elements
delete          delete elements
plan            plan<?> <command>
select          Set these values as the selection.
select+         Add the input to the selection
select-         Remove the input data from the selection
select^         Toggle the input data in the selection
step            step <raster-step-size>
--- tree Commands ---
list            view tree
move            <node> move <destination>, eg ... move 1:0
sub             sub <#>. Tree Context
```

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

This set of commands controls various internal aspects of MeerK40t's execution.

The device refers to either the main driver of the laser cutter or the Kernel itself. 
* `device [<value>]`

Device 0 always refers to the kernel.

The set command allows directly altering values persistently saved to the device. This are often toggled by a variety windows and properties etc, but can also be changed here. 

* `set [<key> <value>]`

Window controls the registered windows commands. And allows them to be launched and closed from the console. This also allows binds to open particular windows.

* `window [(open|close) <window_name>]`

```
Windows Registered:
1: MeerK40t
2: Shutdown
3: PathProperty
4: TextProperty
5: ImageProperty
6: RasterProperty
7: EngraveProperty
8: CutProperty
9: Controller
10: Preferences
11: CameraInterface
12: Terminal
13: Settings
14: Rotary
15: Alignment
16: About
17: DeviceManager
18: Keymap
19: UsbConnect
20: Navigation
21: JobSpooler
22: JobInfo
23: BufferView
24: Adjustments
```

Modules can register controls which run particular functions within those modules. 

* `control [<executive>]`

Modules are bits of code which attach and manipulate the parts of devices. These usually include things like pipes, interpreters, servers, spoolers, and basically any fully encapsulated reusable bit of code for a device. `Console` itself is a module. 

* `module [(open|close) <module_name>]`

Devices all have built in schedulers which allow recurring bits of code to be repeatedly called. These items can be viewed with the `schedule` command. 
* `schedule`

Modules register channels which are text based information flows. The responses to the console commands are themselves sent through the `console` channel and displayed in the terminal. In the batch commands in the CLI a `print` operation is attached to the console channel printing responses as such.

* `channel [(open|close) <channel_name>]`

For example `channel open usb` will allow the terminal to view any usb log items. 

### Elements

Control of the actual elements loaded in the program can be done with the these commands.

The `element` command without data lists the elements that currently loaded. With values it selects those elements. The selected elements are the ones manipulated by the majority of these commands.

* `element [<element>]*`

Note: Any newly added element is selected by default.

Path adds an SVG Path see [SVG 2.0 SVG 9.2](https://svgwg.org/svg2-draft/paths.html#PathElement)) for additional details.
* `path <svg_path>`

Circle executes a normally parsed SVG circle. [SVG 2.0 SVG 10.3](https://svgwg.org/svg2-draft/shapes.html#CircleElement) with the three values of center_x, center_y, and radius.
* `circle <cx> <cy> <r>`

Keep in mind the values are normally parsed so cx and cy and r values can be lengths like `cm`, `mm` or even `%`. `Circle 50% 50% 50%` would make a circle centered at the center of the laser bed, with 50% (min dimension) radius.

Ellipse executes a normally parsed SVG ellipse. [SVG 2.0 SVG 10.4](https://svgwg.org/svg2-draft/shapes.html#EllipseElement) with four values of center_x, center_y, radius_x, radius_y.
* `ellipse <cx> <cy> <rx> <ry>`

Rect executes a normally parsed SVG rect. [SVG 2.0 SVG 10.4](https://svgwg.org/svg2-draft/shapes.html#EllipseElement) 
* `rect <x> <y> <width> <height>`

Text runs adds text element to the scene.
* `text <text>`

Polygon adds a polygon element to the scene. As these properties are in svg units rather than length they take their values directly in mils (1 / 1000th of an inch).
* `polygon [<x> <y>]*`

Polyline is an unclosed element quite similar to polygon. This equally takes units in mils.
* `polyline [<x> <y>]*`

The group command groups the selected items.
* `group`

The ungroup command ungroups the selected groups.
* `ungroup`

The stroke command sets the stroke of the currently selected item.
* `stroke <color>`

The fill command sets the fill of the currently selected item.
* `fill <color>`

The rotate command appends a rotation to the currently selected item. This is centered at the center of the selection.
* `rotate <angle>`

The scale command appends a scale to the currently selected items. This is done relative to the upper left hand corner.
* `scale <scale> [<scale_y>]`

The translate command moves the currently selected item by appending a translation.
* `translate <translate_x> <translate_y>`

### Operations

The operations are what MeerK40t actually runs the laser on. The elements are simply the objects being used to make them.

The operation command lets you see the currently used operations and allows you to specify which should be selected.
* `operation [<element>]*`

The classification process determines which operations particular elements should be occupy. This by default engraves any blue-stroked objects, cuts any red-stroked objects, and rasters anything else or anything with a fill.
* `classify`

Cut automatically classifies the selected elements as cuts and appends that to the operation list.
* `cut`

Engrave automatically classifies the selected element as an engrave and appends that to the operation list.
* `engrave`

Raster automatically classifies the selected element as a raster and appends that to the operation list.
* `raster`

### Binds / Aliases

Binds link keystroke to particular commands. Aliases are shortcuts to commands.
Any command that is bound that starts with a `+` will execute on keydown. But, on keyup will try to execute a `-` version of that operation. There are some loop/end aliases which start the loop on the keydown and end on key-up. The only hardcoded command of this type is: `+laser` and `-laser` which directly turns the laser itself on.

* `bind [<key> <command>]`
* `alias [<alias> <command>]`

So if you enter a command `bind space +laser` then the spacebar will on press start firing the device laser without motion, and on release of the spacebar will execute `-laser` and turn the laser off. There are similar binds of numpad keys for manipulating selected elements.

### Emulator/Servers

Emulators are particular modules which allow for control of the device through a server connection which pretends to be a particular device. The two devices currently allowed here are:

* `ruidaserver`
* `grblserver`

The ruidaserver will pretend to be a ruida device. And your system can be used as if it were a ruida device connecting through the 50200 port for UDP packets. This works even as a localhost loop, where you would have RDWorks or Lightburn (the only two programs that control ruida) and connect that program to MeerK40t. Then MeerK40t pretends to be the device, and translates the the commands into generic commands that can then control an M2 Nano or any proper device MeerK40t controls. 

* Note: as of 5/22/20 Ruidaserver doesn't actually work, it just echos some stuff.

---

The grblserver works similarly and pretends to be a grbl device on port 23. Your system can then talk to MeerK40t like it's a GRBL device and it will translate the sent GCODE and realtime commands into generic operations which are then sent to whatever device is currently running and whatever backend device it controls.

* Note: as of 5/22/20 GRBLserver is kinda weird and may not catch everything. It was kinda working back in March but hasn't been much tested since then and certainly not the full suite of proper tests, with a wide variety of testing programs.

### Misc

Refresh refreshes the scene in case it needs to do that. The screen should refresh for most operations but sometimes it doesn't and quick method of forcing that is thusly provided.

* `refresh`

### Examples

```
operation* delete                  - select all operations and delete them
element* delete                    - select all elements and delete them
cut -c #ff0000 -s 20 mm/s          - create a new operation, speed 20mm/s, color set to red.
rect 0 0 100 100 stroke #ff0000    - create a rect element, stroke=red to prepare for cut op.
element* declassify                - clear any existing classification
element* classify                  - reclassify elements, matching color of elements to color of op
ellipse 100 100 20 25 rotate 45 fill #000000 stroke #ff0000   - create a modify an ellipse
```

### Help Wanted

If there's something you can't do, or something you may need to make accessible to binds, batches, or console raise an issue. Also if you'd have some nice and useful commands and helpful binds, raise an issue for that as well.


---
### Authors
The MeerK40t team is grateful for the help from @... in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃