The MeerK40t console uses pipelines of commands where command output is used as the input of other commands. This permits context relevant usage as well as the linking different commands between different contexts. This also allows commands like `list` to have a dozen different and distinct contexts, being able to list spoolers, drivers, outputs, channels, inputs, ops, elements, clipboard, plans, etc. 

Typing `help` or `?` will display the command list. Typing `help <command>` will provide extended help for that particular command.

## Extended Help

```
 help halftone
 	Returns half-tone image for image.  The maximum output dot diameter is given by sample * scale (which is also the number of possible dot sizes). So sample=1 will preserve the original image resolution, but scale must be >1 to allow variation in dot size.
 
 
 	halftone <oversample:int> <sample:int> <angle:parse>
 	(image) -> halftone -> (image)
 	Argument: int 'oversample':
 		pixel oversample amount
 	Argument: int 'sample':
 		pixel sample size
 	Argument: parse 'angle':
 		half-tone angle
 	Option: int ('--scale', '-s'):
 		process scaling
```

This is the help for the halftone command. This is part of the `image` context. You would need to use an image context in order to use this command, for example `image halftone 2 10 22`. There is extended help that gives information about the command. The expected syntax: `halftone <oversample:int> <sample:int> <angle:parse>` which says oversample is an int sample is a int and angle is an Angle. Angles allow for the CSS angle syntax. So `90deg` `100grad` `0.25turn` `1.57079rad` would all refer to the same quarter circle angle.

### Arguments

If Arguments are omitted it depends on the command as to whether it executes or not. Some will, others will not. However, without all the arguments the next item in the command pipeline cannot be executed. So if you wanted save this image to disk after performing the halftoning. You would not be permitted to without giving all the arguments:

```
 	save <filename:str>
 	(image) -> save -> (image)
 	Argument: str 'filename':
 		filename
```

### Options

So `image halftone 2 10 22deg save myhalftonedimage.png` would work but `image halftone 2 10 save myhalftonedimage.png` would not because it lacks one of the required arguments. Options, however, can be inserted where-ever and are optional. So you could add in a `--scale 2` to the halftone command or neglect to do so, without any issues. 

### Contexts

``` 	(image) -> halftone -> (image)```

In the Extended Help we see input context `image` and the output context `image` this means that we can apply the halftone command in a string of different commands which will execute in series.

Some commands use all the data you give it, for example the `polygon` command:

```
 	polygon 
 	(elements) -> polygon -> (None)
```

This command results in a output context of `None` this is because the requirements for the polygon is that it takes any number of points. `polygon 0 0 1000 1000 0 1000` makes a polygon of a triangle with 1 inch sides.


# Plugins - Internal Use

If you have the console open, you can see that many gui events actually use the internal console to perform many of the actions. Most windows are loaded through the console. When you click a button to open a window MeerK40t calls a console command to open the window rather than opening the window in code. If you click the RasterWizard button you can see the console issues a command `window toggle RasterWizard` this toggles the RasterWizard window. You can type this yourself in console and it will open the same window. This is extremely useful if you are using a plugin or need want to use this internally. If we use the internal `timer` command to perform this command

```
 help timer
 	timer.* <times:str> <duration:float>
 	(None) -> timer.* -> (None)
 	Argument: str 'times':
 		Number of times this timer should execute.
 	Argument: float 'duration':
 		How long in seconds between/before should this be run.
 	Option: bool ('--off', '-o'):
 		Turn this timer off
 	Option: bool ('--gui', '-g'):
 		Run this timer in the gui-thread
```

We require two arguments here, the times to perform the action and the duration between the actions. If we set the times to 0 we can perform this action forever. So if you were using a camera and you wanted to update the background of the camera to the MeerK40t scene background `timer 0 5 camera0 background` where camera0 specifies the first camera. If we wanted to disable the echo on this we would prefix the command with a `.` so `timer 0 5 .camera0 background` would perform the same command but suppress the local echo. If we type `timers` we get a list of the timers and what commands the timer is performing. So:

```
 timers
 ----------
 Timers:
 1: timer1 "camera0 background" forever, each 5.000000 seconds
 ----------
```

We can then disable the timer by typing `timer1 off` or disable all timers with `timer* off`

Going back to the window toggle:

`timerwt 0 1 .window toggle RasterWizard` would flicker RasterWizard window open and close every second. (It also steals focus making killing the timer a bit harder and quite annoying).

## Binds

The most useful application for these console commands are binds. You can see this by typing `bind` in console. We see a list of key binds which specifically link to particular commands. This is also accessible in the keymap window. `window toggle Keymap` will load that window. We can see that the keybinds in meerK40t are actually console commands. In fact they directly call these commands. So if we press control+1 it triggers the command `bind 1 move $x $y` the $x and $y commands get replaced with the current laser head position at the moment of the bind is called. So if you press `1` on your keyboard it MeerK40t will perform a `move` command to this specific location. This creates a hotkey location linked to the number 1.

In the default binds we can see that control+shift+h is bound to `scale -1 1` This performs a scaling operation on the current selected objects which does nothing in the x but inverts in the Y direction. This is a flip command horizontal command. The same applies to control+shift+v in the vertical. Keybinds also link to aliases with the special effect that if they call an alias that begins with `+` it automatically performs the `-` form of the command. So if we do the command `bind space +laser` the use of the spacebar will now bind to the `+laser` command but when the spacebar is released it will call the `-laser` command ***Warning: Do not type +laser outside of a bind it will just start firing your laser in place forever, this is bad.***. This allows you to fire the laser by holding the spacebar. ***Warning: Do not allow a cat to burn down your house.*** These also apply to the alias commands. If you define an alias for +myalias and bind it to a key. It will automatically try to call -myalias when you release the key. This allows you to trigger commands OnKeyDown as well as OnKeyUp. This is how many bind work. For example there are default binds for `numpad_add`, `numpad_subtract` `numpad_divide`, `numpad_multiply`, `numpad_down`, `numpad_up`, `numpad_right`, `numpad_left`, these call aliases like `+rotate_ccw` typing `alias` will allow you to see what these call:

```
3: +rotate_ccw     loop rotate -2
15: -rotate_ccw     end rotate -2
```

The `loop` commands basically trigger this a given command every 50ms or so until stopped with the `end` command. With the note that you cannot cause more than two copies of the same command to exist. This will keep triggering the `rotate -2` command until the key is released. This is done entirely through console commands to bind a key to rotate a object while that key is being held.


# Command List

## Base Commands
Base commands are permitted to begin a new command. Some commands like `solarize` or `preopt` can't be called from a default context. They could only be called from a `image` or `plan` context respectively. Base commands are accepted as the first command in a pipeline. For example, to run the `lhyemulator` you would just type `lhyemulator` without any preceding commands. 

```
--- Base Commands ---
alias           alias <alias> <console commands[;console command]*>
align           align elements
bind            bind <key> <console command>
camera\d*       camera commands and modifiers.
camwin          camwin <index>: Open camera window at index
cd              change directory
channel         channel (open|close|save|list|print) <channel_name>
circle          circle <x> <y> <r> or circle <r>
classify        classify elements into operations
clipboard       clipboard
consoleserver   starts a console_server on port 23 (telnet)
context
control         control [<executive>]
cut             <cut/engrave/raster/imageop> - group the elements into this operation
declassify      declassify selected elements
device          device
dir             list directory
dots            <cut/engrave/raster/imageop> - group the elements into this operation
down            cmd <amount>
driver          driver<?> <command>
element         element, selected elements
element([0-9]+,?)+ element0,3,4,5: chain a list of specific elements
element*        element*, all elements
elements        show information about elements
element~        element~, all non-selected elements
ellipse         ellipse <cx> <cy> <rx> <ry>
embroider       embroider <angle> <distance>
end             end <commmand>
engrave         <cut/engrave/raster/imageop> - group the elements into this operation
fill            fill <svg color>
flush           flush current settings to disk
grblserver      activate the grblserver.
grid            grid <columns> <rows> <x_distance> <y_distance>
home            home the laser
image           image <operation>*
imageop         <cut/engrave/raster/imageop> - group the elements into this operation
inkscape        perform a special inkscape function
input           input<?> <command>
left            cmd <amount>
lhyemulator     activate the lhyemulator.
lhyserver       activate the lhyserver.
line            adds line to scene
load            loads file from working directory
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
operation([0-9]+,?)+ operation0,2: operation #0 and #2
operation*      operation*: all operations
operation.*     operation: selected operations
operations      show information about operations
operation~      operation~: non selected operations.
optimize        optimize <type>
outfile         outfile filename
outline         outline the current selected elements
output          output<?> <command>
path            path <svg path>
plan            plan<?> <command>
plan-alias      Define a spoolable console command
plugin
polygon         polygon (float float)*
polyline        polyline (float float)*
quit            quits meerk40t shutting down all processes
raster          <cut/engrave/raster/imageop> - group the elements into this operation
rect            adds rectangle to scene
refresh         Refresh the main wxMeerK40 window
register
reify           reify affine transformations
reset           reset affine transformations
resize          resize <x-pos> <y-pos> <width> <height>
right           cmd <amount>
rotaryscale     Rotary Scale selected elements
rotaryview      Rotary View of Scene
rotate          rotate <angle>
ruidacontrol    activate the ruidaserver.
ruidadesign     activate the ruidaserver.
scale           scale <scale> [<scale-y>]?
schedule        show scheduled events
set             set [<key> <value>]
shutdown        quits meerk40t shutting down all processes
spool           spool<?> <command>
stroke          stroke <svg color>
stroke-width    stroke-width <length>
tcp             network <address> <port>
text            text <text>
theme           Theming information and assignments
thread          show threads
timer.*         run the command a given number of times with a given duration between.
tool            sets a particular tool for the scene
trace_hull      trace the convex hull of current elements
trace_quick     quick trace the bounding box of current elements
translate       translate <tx> <ty>
tree            access and alter tree elements
unlock          unlock the rail
up              cmd <amount>
webhelp         Launch a registered webhelp page
window          Base window command
```

## Camera Commands
Camera is for the camera add-on and are used to control the camera and set various settings. This includes starting and stopping the camera. Exporting an image to the scene, or setting the image to the background image, as well as setting the perspective and fisheye values. Additional commands like #385 suggestion would be added first into the camera context here.
```
--- camera Commands ---
background      set background image
export          export camera image
fisheye         fisheye subcommand
perspective     perspective (set <#> <value>|reset)
start           Start Camera.
stop            Stop Camera
```

## Channel Commands
The channel commands manipulate the channels within the kernel. These provide extra information about the state of the various channels. This includes watching the channel and saving the channel to disk, printing to the stdout, etc.

```
--- channel Commands ---
close           stop watching this channel in the console
list            list the channels open in the kernel
open            watch this channel in the console
print           print this channel to the standard out
save            save this channel to disk
```

## Clipboard Commands

Clipboard commands can be used to save, cut, and paste data. The command `clipboard` is used to introduce all of these commands. The `clipboard` command also accepts name for specifically referring another clipboard. `clipboard -n 2 cut` creates a clipboard called 2 and performs a cut.
```
--- clipboard Commands ---
clear           clipboard clear
contents        clipboard contents
copy            clipboard copy
cut             clipboard cut
list            clipboard list
paste           clipboard paste
```

## Device Commands
Devices are accessed and manipulated with the device manager. These commands tend to list the devices as a whole. The devices are usually defined by chaining together different spoolers, drivers, and outputs. For example, the primary device is added with the command `spool0 -r driver -n lhystudios output -n lhystudios` and can be seen with `device list`

```
--- device Commands ---
activate        delegate commands to currently selected device
delete          delete <index>
list            list devices
spool           spool<?> <command>
```

## Driver Commands
The driver command is used as part of the devices string linking a particular driver with additional parts. Driver usually accepts a `--new` option which accepts a driver type: `... driver --new lhystudios` would append a driver to a given spooler. This hooks together the spooler -> driver -> output information. See spooler and output for more information.
```
--- driver Commands ---
list            driver<?> list
outfile         outfile filename
output          output<?> <command>
reset           driver<?> reset
tcp             network <address> <port>
type            list driver types
```

## Elements Commands
The elements context refers to the shapes and objects within the scenes. Specifically things like paths, rect, images, etc. These commands tend to either add additional elements or modify existing elements.
```
--- elements Commands ---
align           align elements
circle          circle <x> <y> <r> or circle <r>
classify        classify elements into operations
clipboard       clipboard
copy            duplicate elements
cut             <cut/engrave/raster/imageop> - group the elements into this operation
declassify      declassify selected elements
delete          delete elements
dots            <cut/engrave/raster/imageop> - group the elements into this operation
ellipse         ellipse <cx> <cy> <rx> <ry>
engrave         <cut/engrave/raster/imageop> - group the elements into this operation
fill            fill <svg color>
grid            grid <columns> <rows> <x_distance> <y_distance>
imageop         <cut/engrave/raster/imageop> - group the elements into this operation
line            adds line to scene
list            Show information about the chained data
matrix          matrix <sx> <kx> <sy> <ky> <tx> <ty>
merge           merge elements
outline         outline the current selected elements
path            convert any shapes to paths
polygon         polygon (float float)*
polyline        polyline (float float)*
raster          <cut/engrave/raster/imageop> - group the elements into this operation
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
```

## Image-Array Commands
The Image-array command only permits the image command to wrap the context. This context is the exported from the `camera export` command. This is needed if you needed to export a camera snapshop and manipulate it. For example `camera export image grayscale` would export an image from the camera and convert it into a grayscale image. (Testing did not produce the correct result though 8/12/21)
```
--- image-array Commands ---
image           image <operation>*
```

## Image Commands
```
--- image Commands ---
add             
autocontrast    autocontrast image
blur            blur image
brightness      brighten image
ccw             rotate image ccw
color           color enhance
contour         contour image
contrast        increase image contrast
crop            Crop image
cw              rotate image cw
detail          detail image
dewhite         
dither          Dither to 1-bit
edge_enhance    enhance edges
emboss          emboss image
equalize        equalize image
find_edges      find edges
flatrotary      apply flatrotary bilinear mesh
flip            flip image
grayscale       convert image to grayscale
halftone        halftone the provided image
invert          invert the image
mirror          mirror image
path            return paths around image
quantize        image quantize <colors>
remove          Remove color from image
resample        Resample image
rgba            
save            save image to disk
sharpen         sharpen image
sharpness       shapen image
smooth          smooth image
solarize        
threshold       
unlock          unlock manipulations
vectrace        return paths around image
wizard          apply image wizard
```

## Inkscape Commands

Inkscape commands are found in extra/inkscape.py and these tends to rely on inkscape. They do some limited loading and processing with inkscape. For example if you wanted to use inkscape to load a file and correct the text internally, you would do `inkscape locate input <filename> text2path load` which would use inkscape to process the file you gave it with the command line then load it with text2path.
```
--- inkscape Commands ---
image           image <operation>*
input           input filename
load            load processed file
locate          Locate the inkscape program on your computer
makepng         make png
simplify        simplify path
text2path       text to path
version         determine inkscape version
```

## Input Commands
Input is like driver a part of the devices which serves as the data source. I am unsure if these actually work correctly currently.

```
--- input Commands ---
list            input<?> list, list current inputs
outfile         outfile filename
output          output<?> <command>
tcp             network <address> <port>
type            list input types
```

## Lhystudios Commands
The Lhystudios commands are specifically for M2 Nano and other related devices. Many of these commands are not used or require other functionality for other controller boards. For example the command: `egv IPP$` would very specifically send the commands IPP to the laser controller which is the command for home the device. The `rapid_override` command would override the rapid movements to use alternative slower movements (useful for rotary) but would not be needed for a grbl device etc. These commands often are virtually delegated based on the currently selected device. They can be explicitly delegated with the `dev` command.
```
--- lhystudios Commands ---
abort           Abort Job
acceleration    Set Driver Acceleration [1-4]
code_update     update movement codes for the drivers
continue        abort waiting process on the controller.
egv             Lhystudios Engrave Code Sender. egv <lhymicro-gl>
egv_export      Lhystudios Engrave Buffer Export. egv_export <egv_file>
egv_import      Lhystudios Engrave Buffer Import. egv_import <egv_file>
estop           Abort Job
hold            Hold Controller
pause           realtime pause/resume of the machine
power           Set Driver Power
pulse           pulse <time>: Pulse the laser in place.
rapid_override  limit speed of typical rapid moves.
resume          Resume Controller
speed           Set current speed of driver.
start           Start Pipe to Controller
status          abort waiting process on the controller.
usb_abort       Stops USB retries
usb_connect     Connects USB
usb_disconnect  Disconnects USB
```

## Moshi Commands
Moshi commands are moshi specific commands that only apply when a Moshiboard is currently connected.
```
--- moshi Commands ---
abort           Abort Job
continue        abort waiting process on the controller.
hold            Hold Controller
resume          Resume Controller
start           Start Pipe to Controller
status          abort waiting process on the controller.
usb_connect     Connect USB
usb_disconnect  Disconnect USB
```

## Ops Commands
Ops commands refer to the operations located in the element tree. These commands are used to manipulate those values or send them to a spooler etc.
```
--- ops Commands ---
copy            duplicate elements
delete          delete elements
list            Show information about the chained data
load            Load operations from persistent settings
plan            plan<?> <command>
save            Save current operations to persistent settings
select          Set these values as the selection.
select+         Add the input to the selection
select-         Remove the input data from the selection
select^         Toggle the input data in the selection
step            step <raster-step-size>
```

## Output Commands
```
Output is part of the devices and usually `output -n lhystudios` is used as part of the device string. This results in creating a lhystudios controller for the selected device. 
--- output Commands ---
list            output<?> list, list current outputs
type            list output types
```

## Plan Commands
Plan commands refer to the job planner. This is what the ExecuteJob dialog does internally. This plans out the various stages and permits the conversion from operations into cutcode, and from cutcodes into cutcommands which are sent to a laser. These are the processing and preprocessing for the job being executed. If you run the command `plan copy preprocess validate blob preopt optimize spool` this would take the current job and send it directly to the laser without any GUI interactions. If you bound a command to this `bind control+g plan copy preprocess validate blob preopt optimize spool` you would have a hot key to simply run the current work.
```
--- plan Commands ---
append          plan<?> append <op>
blob            plan<?> blob
classify        plan<?> classify
clear           plan<?> clear
command         plan<?> command
copy            plan<?> copy
copy-selected   plan<?> copy-selected
list            plan<?> list
optimize        plan<?> optimize
preopt          plan<?> preopt
prepend         plan<?> prepend <op>
preprocess      plan<?> preprocess
return          plan<?> return
spool           spool<?> <command>
step_repeat     plan<?> step_repeat
validate        plan<?> validate
```

## Spooler Commands
The spooler commands refer to the spooler which is a list of would-be executed objects. This is mostly going to cutcode but could include special commands like unlock the rail and or even execute some console commands. These are lists of elements which should be executed in order. Whether a driver or output exist is not relevant for a particular spooler. It's a location that data is sent. These can usually be selected in the JobSpooler window or in the ExecuteJob or Simulate windows. 
```
--- spooler Commands ---
clear           spooler<?> clear
down            cmd <amount>
driver          driver<?> <command>
home            home the laser
left            cmd <amount>
list            spool<?> list
lock            lock the rail
move            move <x> <y>: move to position.
move_absolute   move <x> <y>: move to position.
move_relative   move_relative <dx> <dy>
right           cmd <amount>
send            send a plan-command to the spooler
unlock          unlock the rail
up              cmd <amount>
```

## Tcpout Commands
The TCPout commands specifically reference the tcp device that should exist locally, there is only a single test command to set the current port.
```
--- tcpout Commands ---
port            change the port of the tcpdevice
```

## Window Commands
Window commands are gui relevant to load, open, close, and toggle windows. The `open` and `toggle` command allows for three different options: `--driver` `--output` and `--input` these load the source specific windows. For example the Controller window you need to load depends on the type of output you have, so the command to load the window is `window toggle -o Controller` which toggles the Controller window but does so specific to the output of the current selected device. If you have a TCPout for your output it loads a TCPController. If you have a Moshi controller it will load the Moshi specific controller window. This also allows for some controller specific windows that do not exist for other devices for example: `window open --output AccelerationChart` loads a specific settings window (does not work) for Lhystudios boards but does nothing for any other output windows. There are specific controllers for tcp, file, lhystudios, and moshi board. If full ruida support was added these sort of context dependent windows would be needed for the board specific information.

```
--- window Commands ---
close           close the supplied window
list            List available windows.
open            open/toggle the supplied window
reset           reset the supplied window, or '*' for all windows
toggle          open/toggle the supplied window
```

# Example

## High Automation

If you wanted to do something extreme with very high automation of the lasering processing, for example:

https://twitter.com/giladrom/status/1394797667251724290

We would start from very basic primitives statements and chain them heavily together.

Let's say you want to use to the console to create a blue-filled circle.

`circle 3in 3in 1in fill blue` the first command is the `circle` command this takes 3 arguments.

```
- help circle
circle <x_pos:Length> <y_pos:Length> <r_pos:Length>
(elements) -> circle -> (elements)
Argument: Length 'x_pos'
Argument: Length 'y_pos'
Argument: Length 'r_pos'

circle <x_pos:Length> <y_pos:Length> <r_pos:Length>
(None) -> circle -> (elements)
Argument: Length 'x_pos'
Argument: Length 'y_pos'
Argument: Length 'r_pos'
```

Note that in help this information is listed twice because circle activates from two different contexts these are `elements` and `None` with `None` being the base context. Since all arguments were filled in the circle command was completed and our command was in an elements context. We follow this with the fill command.

```
help fill
	fill <color:Color>
	(None) -> fill -> (elements)
	Argument: Color 'color':
		color to color the given fill


	fill <color:Color>
	(elements) -> fill -> (elements)
	Argument: Color 'color':
		color to color the given fill
```
The fill command takes in a color. This is any CSS-color value color, since it uses the same parsing routine. So any predefined css color (https://www.w3.org/wiki/CSS/Properties/color/keywords)  or hex code (eg: #ff00ff), will be accepted.

We also get an elements context as output. This means we can string to the end any additional elements commands to the end of the fill command.
* These are other element creation commands, `circle`, `ellipse`, `rect`, `polygon`, `polyline`, or `text` commands.
* Affine transformation commands such as `scale`, `rotate, `reset`, `reify`, or `matrix`.
* Operations commands such as `cut`, `engrave`, `imageop`, `dots`, or `raster`.
* Color commands like `stroke` and `fill`.

But, let's say we want want to apply another circle inside the current circle and add it to a 3x5 grid:
`circle 3in 3in 1in fill red circle 3in 3in 0.5in fill -f 1 #ff00ff grid 3 5 1in 1in` Should create two circles with centers located at 3in 3in in the scene with two a red fill and another with half the radius and a magenta fill, these items should then be added to a grid. Our grid command `grid 3 5 1in 1in` would duplicate the current item 15 times and put those duplications 1 inch apart.

Let's say we wanted to do something even more intricate and put these elements into a raster operation that we create.

```
raster 
	(elements) -> raster -> (ops)
	Option: float ('--speed', '-s')
	Option: float ('--power', '-p')
	Option: int ('--step', '-S')
	Option: Length ('--overscan', '-o')
	Option: Color ('--color', '-c')
	Option: int ('--passes', '-x')
```

We see that raster accepts options rather than arguments. This means the `raster` command itself accept elements contexts, but and requires no arguments. But, we can set the various speeds and power amounts during the creation. If we wanted this with a step of 2 and speed of 99 mm/s. Our command would be `raster --step 2 --speed 99` but we could also do `raster -S 2 -s 99` or even `raster -Ss 2 99`. We then automatically put the newly created grid of circles within the newly created raster object.

`circle 3in 3in 1in fill red circle 3in 3in 0.5in fill -f 1 #ff00ff grid 3 5 2in 2in raster -Ss 2 99` (this doesn't work because grid fails and raster fails 8/12/21).

The raster command however changes our context. We were doing `elements` contexts strings but putting this into a raster gave us an operations context `ops`. 

In help, we can see the list the given context, it accepts `copy`, `delete`, `list`, `load`, `plan`, `save`, `select`, `select+`, `select-`, `select^` and a `step` command that sets the step property for the operations. The load and save are the values found under right-click menu of operation. If you wanted to make your own set of save operations you could type `operation save My-stuff` and get a my-stuff valeu there. The most useful context for us here is going to be plan which moves us from the `ops` context into a `plan` context. Specifically this works to start planning the data for running on our device.

The plan command itself is a transition command. It mostly packages our `ops` data into the planner. It doesn't take any arguments or options. But, gives us access to plan context commands. These are the different stages, `copy`, `preprocess`, `validate`, `blob`, `preopt`, `optimize`, and `spool`. Because we transitioned from a `ops` context we basically already performed copy. The copy command moves data from the operations in the tree into the planner. The copy-selected does this only for the selected operations. We perform the other parts of the planning stages, doing `... raster -Ss 2 99 plan preprocess validate blob spool` we are skipping the pre-optimize and optimize stages.

This gives us the command:
`circle 3in 3in 1in fill red circle 3in 3in 0.5in fill -f 1 #ff00ff raster -Ss 2 99 plan preprocess validate blob spool`

Which creates two circles of different colors, puts them in a grid, puts that grid of commands into the planner and converts it to an image and sends it to the default spooler.

Since we can call console commands from the commandline:

`meerk40t -ze "circle 3in 3in 1in fill red circle 3in 3in 0.5in fill -f 1 #ff00ff raster -Ss 2 99 plan preprocess validate blob spool"`

Could be used to perform this entire sequence and send it directly to the laser. This might not correctly quit the program after it's done. So we could add additional commands in the spooler to shutdown Meerk40t. We would do this with the `plan` context command of `append` specifically we would append a `shutdown` command.

`meerk40t -ze "circle 3in 3in 1in fill red circle 3in 3in 0.5in fill #ff00ff raster -Ss 2 99 plan preprocess validate blob append shutdown spool"`

The reason this called -z rather than -Z is the need for the meerK40t gui to semi-launch. WxPython is used to convert a filled shape into a real image. the --gui-suppress flag in the CLI `completely suppress gui` and we simply need to run `--no-gui` and `run without gui`. If we drop the -z command it would launch as normal, send the data, then shutdown the gui when finished.

```
  -z, --no-gui          run without gui
  -Z, --gui-suppress    completely suppress gui
```
