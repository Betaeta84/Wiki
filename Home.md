# Welcome to MeerK40t!
MeerK40t is the extendable laser software for the Stock K40. We hope that you find this **free** software useful, and hopefully not too difficult to learn to use (although we know there is scope for improvement in both the software's usability and the documentation - and we hope that you may be willing to help out with these).

## Overview
**Congratulations on finding MeerK40t.** Perhaps you started with the software supplied with your K40, or perhaps you tried K40 Whisperer first, or maybe knowledge of our existence is spreading and you came here first... Or perhaps you have heard about the industry leading Lightburn software which is the standard to aspire to, but which cannot drive the K40... but regardless of how you got here, a BIG welcome.

Meerk40t is a replacement for both the software supplied with the K40 and the K40 Whisperer software. It is designed specifically to drive the popular K40 class of (chinese built) laser machines which have an Lhymicro controller board. Whilst Meerk40 aspires to be able to drive other controller boards (e.g. Gerbil based boards), this is still to be written, so if you have a different controller board thanks for dropping by now, but please check in occasionally to see if your board is now supported.

We hope that you will find Meerl40t to be better than the alternatives for the stock K40, and hope that it allows you to achieve more without having to spend your time and effort **and** a significant proportion of the original cost of your K40 both upgrading your controller and paying for Lightburn.

So what makes MeerK40t different from the alternatives:

### Paths and images
MeerK40t supports 4 types of burn operation:
* Vector cuts / vector engraving - essentially the same, with the laser following the vector path, and differing only on the strength of the burn
* Vector raster - burning all points inside a (closed) vector path
* Image raster - burn a raster image using pixels

MeerK40t aims to fully support the SVG 1.1 standard (including specific extensions for the major editors where that makes sense).

### Power Control
With a K40, you can set the power of the laser from the front panel, but the controller board is only able to turn the laser on and off.

Fortunately the controller can turn the laser on and off very fast indeed. In fact it can do this 1,000x per inch (or c. 400 per cm) which is enough to turn it on and off several times in the width of the laser beam itself!!

MeerK40t uses this capability to vary the strength of the laser, either:
* Constantly every 2nd or 3rd or ... 1/1000 of an inch; or
* For rasters, variably depending upon the grey-scale level of pixels

The technical details of this [Pulse Modulation feature](./Tech:-Raster-pulse-modulation-PPI) can be found [here](./Tech:-Raster-pulse-modulation-PPI).

### Accuracy
MeerK40t uses a special set of [Zingl-Bresham curve-plotting algorithms](./Tech:-Zingl-Bresenham-Curve-Plotting) to perfectly plot shapes. If your SVG has curved paths (which technically are either elliptical-arcs or Bezier-curves), then by lasering these as precise curves rather than approximating them with many short straight lines, they will be plotted perfectly to the nearest 1/1000".

### Choice of Drivers
The MeerK40t driver interface uses either the LibUsb driver or the CH341DLL default windows driver. This means that MeerK40t can run alongside other laser software that uses either of these drivers.

### Command Line
MeerK40t has a comprehensive [command line interface](./Help:-Command-Line-Interface). If you want to integrate your K40 into an automated workflow or just prefer to use the command line, you should be able to execute most projects without using the GUI.

MeerK40t also has an internal [Console command line](./Help:-Console-Commands) where more advanced functionality can be executed than available through the GUI.

## Documentation
Our aim is to have a comprehensive set of documentation covering:
* [Beginners](./Beginners:-0.-Index) - Help to set up your hardware, install MeerK40t and achieve your first burn
* [Detailed Manual](./Doc:-0.-Index) - A detailed hierarchically-structured set of indexed pages describing all the functionality of MeerK40t in detail.
* [Further Help]() - Additional help on specialised areas
* [Installation](./Beginners:-2.-Installing-MeerK40t) - A page giving links to detailed instructions on installing MeerK40t on most major platforms

However we are still a long way from achieving this, so any help you can give in improving our documentation would be very welcome.

## Help Wanted
Code is a huge part of a computer program, but a small part of a community.

If you are worried that you won't be able to contribute because you can't write Python code, then you shouldn't because there are many other ways you can contribute:
* [Updating and enhancing these Wiki pages](./Tech:-Creating-a-wiki-page) to pass on your experiences to others.
* Writing your own or enhancing other documentation to help others at all levels of understanding.
* Thinking about and organizing longer term priorities.
* Learning and sharing new information.
* Raising, filing, discussing [Github issues](/meerk40t/meerk40t/issues), either when things don't work as you think they should or if you have ideas for improvements. Test, address and amplify the issues of others. 
* Upskilling new contributors
* Helping users, not just here but in the broader community. Bigger communities are more robust and higher achieving.
* Automating workflows and defining workflows. 
* [Translating the application strings into other languages](./Tech:-Foreign-Language-Translations) (this tends to require not just fluency but being fluent in laser jargon)

* Testing out new experimental functionality on your machine or helping research the capabilities / speeds etc. of the controller board, or other controller boards we might want to support

See [Tech: Help Wanted](https://github.com/meerk40t/meerk40t/wiki/Tech:-Help-wanted) for more details.

Finally, if you are using MeerK40t for commercial gain (and Meerk40t is provided free regardless of whether you are a home hobbyist or using it to make stuff to sell), then if you are unable to give something back by giving your time to any of the above activities, please consider [sponsoring @tatarize](/sponsors/tatarize) as a way of saying thank you to him for all his efforts and to help pay for the small number of things that the project actually has to buy (like code signing certificates or research hardware). Note: This software is free to use and there are no licensing or legal terms which compel you to reward @tararize in this way, and the choice is yours to make freely - but it just seems ethical to share a small proportion of your profits with him when you benefit financially from his efforts.

### Coding
If you wish to write a module either for public consumption or for private use:

* See the [Kernel API System](https://github.com/meerk40t/meerk40t/wiki/Tech:-Kernel-API-System) Documentation.

If you wish to write GUIs for your modules, you may need to add icons.

* [Adding Icons to MeerK40t](https://github.com/meerk40t/meerk40t/wiki/Tech:-Adding-Icons-to-a-MeerK40t-Module)

* [UI and UX Design Guide](https://github.com/meerk40t/meerk40t/wiki/Tech:-UI-and-UX-Design-Guide)

## Detailed Documentation for the Stock Board.

If you want detailed documentation for the stock board, then the research we have undertaken to understand the hardware can be found in this wiki.

There are specific pages for the [Lhymicro M2-Nano Hardware](./Tech:-Lhymicro-M2-Nano-Hardware) and the [Lhymicro Control Protocols](./Tech:-Lhymicro-Control-Protocols) used by software like MeerK40t to control the K40.

The Tech section of the Wiki also has other research on the K40 hardware, for example [Physical Speed Measurements](./Tech:-Physical-Speed-Measurements)

## SVG Namespace
For saving files MeerK40t uses a modified form of SVG, for which the [Namespace specification is here](./Tech:-SVG-Namespace).

# Acknowledgements

* Primary author: Tatarize
* Additional coding: JKRamarz, MartonMiklos, Jaredly, FrogMaster, Sophist-UK, GSBoylan, 
* Translations: Peter9811 (Spanish), 
* Wiki: Tatarize, JoerLane (M2-Nano Guru), Tiger12506, Olivier33m, Sophist-UK

If you have contributed, please edit this page and add your name to the above list.

---
### Authors
The MeerK40t team is grateful for the help from @taterize, @joerlane  & @Sophist-UK in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃