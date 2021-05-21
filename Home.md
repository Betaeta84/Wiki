# Welcome to MeerK40t!
MeerK40t is the extendable laser software for the Stock K40. We hope that you find this **free** software useful, and hopefully not too difficult to learn to use (though we know there is scope for improvement in both the software's usability and the documentation - and we hope that you may be willing to help out with these).

## Overview
Congratulations on finding MeerK40t. Perhaps you started with the software supplied with your K40, or perhaps you tried Whisperer first, or maybe knowledge of our existence is spreading and you can here first... Or perhaps you have heard about the industry leading Lightburn software which is the standard to aspire to, but which cannot drive the K40... but regardless of which welcome.

Meerk40t is a replacement for both the software supplied with the K40 and the K40 Whisperer software. It is designed specifically to drive the popular K40 class of (chinese built) laser machines which have an Lhymicro controller board. If you have a different controller board thanks for dropping by.

So what makes MeerK40t different from the alternatives:

#### Paths and images
MeerK40t supports 4 types of burn operation:
* Vector cuts / vector engraving - essentially the same, with the laser following the vector path, and differing only on the strength of the burn
* Vector raster - burning all points inside a (closed) vector path
* Image raster - burn a raster image using pixels

MeerK40t aims to fully support the SVG 1.1 standard (including specific extensions for the major editors where that makes sense).

#### Power Control
With a K40, you can set the power of the laser when it is on from the front panel, but the controller board is only able to turn the laser on and off.

Fortunately the controller can turn the laser on and off very fast indeed. In fact it can do this 1,000x per inch (or c. 400 per cm) which is enough to turn it on and off a couple of times in the width of the laser beam itself!!

MeerK40t uses this capability to vary the strength of the laser, either:
* Constantly every 2nd or 3rd or ... every 1/1000 of an inch or
* For rasters, variably depending upon the grey-scale level of pixels

Technical details of this [Pulse Modulation feature](https://github.com/meerk40t/meerk40t/wiki/Tech:-Raster-pulse-modulation-PPI) can be found [here](https://github.com/meerk40t/meerk40t/wiki/Tech:-Raster-pulse-modulation-PPI).

#### Accuracy
MeerK40t uses a special set of [Zingl-Bresham curve-plotting algorithms](https://github.com/meerk40t/meerk40t/wiki/Tech:-Zingl-Bresenham-Curve-Plotting) to perfectly plot shapes. If your SVG has elliptical-arcs or Bezier-curves, by calculating these as curves rather than approximating them with many short straight lines, they will be plotted perfectly to the nearest 1/1000".

#### Choice of Drivers
The MeerK40t driver interface uses either the LibUsb driver or the CH341DLL default windows driver. This means that MeerK40t can run alongside other laser software that uses either of these drivers.

#### Command Line
MeerK40t has a comprehensive [command line interface](/meerk40t/meerk40t/wiki/Help:-Command-Line-Interface). If you want to integrate your K40 into an automated workflow or just prefer to use the command line, you should be able to execute most projects without using the GUI.

MeerK40t also has an internal command line where more advanced functionality can be executed than available through the GUI.

## Documentation
Our aim is to have a comprehensive set of documentation covering:
* [Beginners](/meerk40t/meerk40t/wiki/Beginners:-0.-Index) - Help to set up your hardware, install MeerK40t and achieve your first burn
* [Detailed Manual](/meerk40t/meerk40t/wiki/Doc:-0.-Index) - A detailed hierarchically-structured set of indexed pages describing all the functionality of MeerK40t in detail.
* [Further Help]() - Additional help on specialised areas
* [Installation](https://github.com/meerk40t/meerk40t/wiki/Install:-General) - Detailed instructions on installing MeerK40t on most major platforms

However we are still a long way from achieving this, so any help you can give in improving our documentation would be very welcome.

## Help Wanted
@Sophist points out that whilst @taterize (David Olsen - the author) is doing a great job in creating this useful tool, there is always more work to do than he can handle alone. So, if you benefit from using this **free** software, please, please show your appreciation by contributing something back and helping him out. 

If you are worried that you will be unable to contribute because you can't write Python code, then let me reassure you that there are many other ways you can contribute:
* Updating and enhancing these Wiki pages to pass on your experiences to others
* Translating the application strings into other languages (please only do this for languages you are fluent in)
* Raising Github issues when things don't work as you think they should or suggesting ideas for other improvements
* Testing out new beta functionality on your machine or helping research the capabilities / speeds etc. of the controller board

See [Tech: Help Wanted](https://github.com/meerk40t/meerk40t/wiki/Tech:-Help-wanted) for more details.

### Coding
If you wish to write a module either for public consumption or for private use:

* See the [Kernel API System](https://github.com/meerk40t/meerk40t/wiki/Tech:-Kernel-API-System) Documentation.

If you wish to write GUIs for your modules, you may need to add icons.

* [Adding Icons to MeerK40t](https://github.com/meerk40t/meerk40t/wiki/Tech:-Adding-Icons-to-a-MeerK40t-Module)

* [UI and UX Design Guide](https://github.com/meerk40t/meerk40t/wiki/Tech:-UI-and-UX-Design-Guide)

## Detailed Documentation for the Stock Board.

If you want detailed documentation for the stock board this is the place to be. The only place to be.
[Understanding Lhymicro-GL](https://github.com/meerk40t/meerk40t/wiki/Tech:-Lhymicro-GL)

[Lhystudios Boards Hardware specifics](https://github.com/meerk40t/meerk40t/wiki/Tech:-Lhystudios-Hardware-specifics)

This is also a place for research on the board:
[Physical Speed Measurements](https://github.com/meerk40t/meerk40t/wiki/Tech:-Physical-Speed-Measurements)

## SVG Namespace

For saving files MeerK40t uses a modified form of SVG, the [Namespace specification is here.](https://github.com/meerk40t/meerk40t/wiki/Tech:-SVG-Namespace)

---
### Authors
The MeerK40t team is grateful for the help from @taterize, @joerlane  & @Sophist-UK in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃