For saving files MeerK40t uses an extended version of SVG 1,1.

The SVG 1.1 specification

# Namespace Declaration

`<svg> attribute: xmlns:meerk40t="https://github.com/meerk40t/meerk40t/wiki/Tech:-SVG-Namespace"`

This flags the SVG as being produced by Meek40t, and containing MeerK40t objects.

There are two primary MeerK40t specific objects: `operation` and `note`

The file will contain `text`, `image`, `path`, in pure svg. The SVG height and width will also be set to the bed size used so loading in other applications will give you a similar view. Images will be embedded into the SVG this will happen for all images regardless how they were initially loaded.

---

# Operations

Operations merely define a laser operation. These will have the same properties as the `LaserOperation` class in MeerK40t. The expectation is that these will have the same `type` as the default operation. Please note if additional elements are added to the `LaserOperation` class these will be used. If the operation object contains any attribute which matches the same attribute found in the `LaserOperation` class. This will be used, and an attempt will be made to type the object as the same type found within the LaserOperation default init. Currently the accepted attributes are:

* acceleration="1", type of int, defines the custom acceleration value.
* acceleration_custom="True", type of bool, defines whether the custom acceleration value is used.
* advanced="True", type of bool, defines whether this particular operation opens advanced properties.
* color="#000000", type of Color (see `svgelements` Color class), defines the color of the operation. Since this is svg color, it will permit anything valid for SVG color classes.
* dot_length="1", type of int, defines the length dots should tend to pattern into if they can be patterned.
* dot_length_custom="True", type of bool, defines whether custom dot_length is used.
* dratio="0.261", type of float, defines the diagonal ratio value used by MeerK40t.
* dratio_custom="True", type of bool, defines whether the dratio value should be used.
* emphasized="False", type of bool, defines a temporary emphasis flag within MeerK40t.
* group_pulses="True", type of bool, defines whether it should group pulses, moves single black dots in the output stream by 1 unit to try to make sure they form into groups.
* highlighted="False", type of bool, defines a temporary highlight flag within MeerK40t.
* operation="Raster", type of str, defines the type of operation we are using.
* output="True", type of bool, defines whether the operation should execute or spool
* overscan="20", type of int, defines the amount of overscan to use with rasters.
* passes="1", type of int, defines the amount of passes the operation should conduct.
* passes_custom="True", type of bool, defines whether the custom passes should be used.
* power="1000.0", type of float, defines the power in PPI to use. 1000 is max. 
* raster_direction="0", type of int, Defines the raster direction.
    * 0 is top-bottom
    * 1 is bottom-top
    * 2 is left-right
    * 3 is right-left
    * 4 is crosshatch
* raster_preference_bottom="0", type of int, defines the start position preference when starting on the bottom.
    * -1 is left
    * 0 is no preference
    * 1 is right
* raster_preference_left="0", type of int, defines the start position preference when starting on the left.
    * -1 is top
    * 0 is no preference
    * 1 is bottom
* raster_preference_right="0", type of int, defines the start position preference when starting on the right.
    * -1 is top
    * 0 is no preference
    * 1 is bottom
* raster_preference_top="0", type of int, defines the start position preference when starting on the top.
    * -1 is left
    * 0 is no preference
    * 1 is right
* raster_step="1", type of int, defines the raster step to be used with this operation.
* raster_swing="True", type of bool, defines whether this raster should be bidirectional or not
* selected="False", type of bool, internal selection property of elements within MeerK40t
* show="True", type of bool, defines whether objects of this property should show in the scene.
* speed="200.0", type of float, defines the speed in mm/s


### Operation Example

`<operation acceleration="1" acceleration_custom="True" advanced="True" color="#000000" dot_length="1"`
               `dot_length_custom="True" dratio="0.261" dratio_custom="True" emphasized="False" group_pulses="True"`
               `highlighted="False" operation="Raster" output="True" overscan="20" passes="1" passes_custom="True"`
               `power="1000.0" raster_direction="0" raster_preference_bottom="0" raster_preference_left="0"`
               `raster_preference_right="0" raster_preference_top="0" raster_step="2" raster_swing="True"`
               `selected="False" show="True" speed="200.0"/>`

---

## Note

The other non-svg element within the xml is note. This has one attribute `text` and the value of the text is the note. This is to save some information about the svg in question or any other information that you would prefer remain with a particular project file.