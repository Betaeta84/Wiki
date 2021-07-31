The purpose of this Wiki page is to familiarise you with MeerK40t's Graphical User Interface. If you are reading this page, we are going to assume that you have read the earlier beginners wiki pages and so are familiar with the Safety Issues, you have unpacked and checked, commissioned (and enhanced) your K40, that you understand the basics of Laser cutting and engraving and are therefore now ready to learn how to use MeerK40t. 

If you haven't read the earlier pages we recommend that you return to the [Beginners Index](./Beginners:-0.-Index) and read them before returning to read this page.

## Contents
* [Introduction](#Introduction)
* Menus
* Ribbon Bar functions
* Panes & Windows
* 

## Introduction
When you first run MeerK40t, it should look something like this:

![image](https://user-images.githubusercontent.com/3001893/127753148-361eefa5-dad7-4d75-b9d7-05b9739c6062.png)

This screenshot was taken on Windows, but it should look pretty similar on other operating systems. (Hopefully your display hardware will allow the window to be bigger than the above layout shown as an example.) The layout consists of the following elements:

1. [Menu bar](#Menus) - with File, View, Pane, Tools, Help and Languages menus.
2. The [Ribbon-bar](#Ribbon-Buttons) - giving buttons to access all the major MeerK40t functions
3. [Top, left, right and bottom pane-bars](#panes-and-windows) - these can be configured to show a variety of different panes which can provide either control of the K40 or information about what MeerK40t is doing. Panes can also be undocked, and most of them are also available as Windows. The key difference between panes and Windows is (in the opinion of the author) that panes should be used to display controls or information that you want available all the time, and Windows for controls or information that you want to see occasionally. 
4. [The "scene"](#The-Scene) i.e. a representation of the K40 laser bed
5. [The Status bar](#Status-bar)

## Menus
The Menu bar has the following menus:
1. [File](#File-Menu) - Load & save files, exit MeerK40t
2. [View](#View-Menu) - Control what is shown in the "scene"
3. [Pane](#Pane-Tools-Menus) - Control what panes are shown as standard, unlock panes so you can move them around.
4. [Tools](#Pane--Tools-Menus) - Open windows
5. [Help](#Help-Menu) - Access various help and support web sites
6. [Languages](#Languages) - Set the language you want to view the MeerK40t interface in.

### File Menu
![image](https://user-images.githubusercontent.com/3001893/127753386-5306bcd9-32ec-40d6-b646-5b275c0433f2.png)
The file menu allows you to:
* New - Clear the existing project from MeerK40t
* Open Project - load a vector, image or mixed file
* Import - import a vector, image or mixed file
* Save the elements of these files together with the details of the Operations you have set
* Exit MeerK40t

The difference between Open Project and Import is ... (*to be added*).

The following file formats are supported for loading and importing:
* SVG
* Images - PNG, JPEG, GIF, BMP, ICO, WEBP
* EPS
* Engrave EGV
* Gcode
* RDWorks RD
* DRawing Exchange Format DXF

The following file formats are supported for saving:
* SVG

### View Menu
![image](https://user-images.githubusercontent.com/3001893/127753609-40d6d818-c993-4ad7-b9f9-a1f3614ce46c.png)

The View Menu essentially controls what is shown in the "scene":
* Zoom - Zoom in or out or zoom to fit the scene to the available space
* Show or hide:
    * The grid
    * The background image (if you set it)
    * The measurement guides at top and left of the scene
    * Different types of SVG graphics - Paths, Images, Text, Fills, Strokes
    * Scaled stroke widths
    * Execution animation - Laserpath (cumulative blue lines showing historic path of the laser head), Reticle (small red circle showing current location of the laser head)
    * Selection - hide the selection box when objects are selected
    * Icons / Tree - ???
    * Image caching / Refresh / Animate - ???
    * Invert - Negative image of scene
    * Flip XY - flip Top-Left and Bottom-Right corners of the screen - effectively upside-down

### Pane & Tools Menus
### Help Menu
### Languages

---
### Authors
The MeerK40t team is grateful for the help from @Sophist-UK in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃