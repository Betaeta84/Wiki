**WARNING: This page is currently work in progress.**

The purpose of this Wiki page is to familiarise you with MeerK40t's Graphical User Interface. If you are reading this page, we are going to assume that you have read the earlier beginners wiki pages and so are familiar with the Safety Issues, you have unpacked and checked, commissioned (and enhanced) your K40, that you understand the basics of Laser cutting and engraving and are therefore now ready to learn how to use MeerK40t. 

If you haven't read the earlier pages we recommend that you return to the [Beginners Index](./Beginners:-0.-Index) and read them before returning to read this page.

## Contents
* [Introduction](#Introduction)
* [Menus](#Menus)
* [Panes & Windows](#Panes--Tools-Menus--Ribbon)
* 

## Introduction
When you first run MeerK40t, it should look something like this:

![image](https://user-images.githubusercontent.com/3001893/127753148-361eefa5-dad7-4d75-b9d7-05b9739c6062.png)

This screenshot was taken on Windows, but it should look pretty similar on other operating systems. (Hopefully your display hardware will allow the window to be bigger than the above layout shown as an example.) The layout consists of the following elements:

1. [Menu bar](#Menus) - with File, View, Pane, Tools, Help and Languages menus.
2. The [Ribbon-bar](#Ribbon-Buttons) - giving buttons to access all the major MeerK40t functions
3. [Top, left, right and bottom pane-bars](#panes-and-windows) - these can be configured to show a variety of different panes which can provide either control of the K40 or information about what MeerK40t is doing. Panes can also be undocked, and most of them are also available as Windows. The key difference between panes and Windows is (in the opinion of the author) that panes should be used to display controls or information that you want available all the time, and Windows for controls or information that you want to see occasionally. The number of panes you will want showing will likely depend on the size / resolution of your display.
4. [The "scene"](#The-Scene) i.e. a representation of the K40 laser bed
5. [The Status bar](#Status-bar)

## Menus
The Menu bar has the following menus:
1. [File](#File-Menu) - Load & save files, exit MeerK40t
2. [View](#View-Menu) - Control what is shown in the "scene"
3. [Pane](#Panes--Tools-Menus--Ribbon) - Control what panes are shown as standard, unlock panes so you can move them around.
4. [Tools](#Panes--Tools-Menus--Ribbon) - Open windows
5. [Help](#Help-Menu) - Access various help and support web sites
6. [Languages](#Languages-Menu) - Set the language you want to view the MeerK40t interface in.

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

### Panes / Tools Menus & Ribbon
![image](https://user-images.githubusercontent.com/3001893/127766719-ae6a6a57-57e4-4f5b-ab09-f3a67ec09aa8.png) & ![image](https://user-images.githubusercontent.com/3001893/127766818-71c47047-9592-40f1-a9db-9e790b60b52d.png)

The Panes & Tools menus allow you to control the visibility of the Panes and Windows that provide laser controls and information display. Most of these are available in both Panes and Windows (Panes for permanent visibility, Windows for temporary visibility). The number of panes you will want showing will likely depend on the size / resolution of your display.

Panes can be placed in the left, right, top or bottom Pane Bars, or be floating. You can lock and unlock the movements of Panes. Windows are always floating and moveable, and their position is persistent.

The Ribbon is a specialised Pane, but there are also individual Toolbar Panes equivalent to each of the Toolbars in the Ribbon.

The list of Panes & Windows is as follows:
|Content|Pane|Window|Ribbon||Content|Pane|Window|Ribbon|
|-|-|-|-|-|-|-|-|-|
|[Tree](#Tree)|x| | ||[Devices](#Devices)|x|x|x|
|[Navigation](#Navigation)|x|x|x||[Camera](#Camera)|x|x|x|
|[Position](#Position)|x| | ||[Execute Job](#Execute-Job)| |x|x|
|[Ribbon](#Ribbon)|x| | ||[Simulate](#Simulate)| |x|x|
|[Toolbars](#Ribbon-Toolbars)|x| |x||[RasterWizard](#Raster-Wizard)| |x|x|
|[Stop](#Stop-Pause-Home)|x| |x||[Controller](#Controller)||x|x|
|[Pause](#Stop-Pause-Home)|x| |x||[Config](#Config)||x|x|
|[Home](#Stop-Pause-Home)|x| | ||[Settings](#Settings)||x|x|
|[Notes](#Notes)|x|x|x||[Keymap](#Keymap)||x|x|
|[Spooler](#Spooler)|x|x|x||[Rotary](#Rotary)||x|x|
|[Console](#Console)|x|x|x||[USB](#USB)||x||

Each of the above functions is depicted and **very** briefly described in the [Functions](#Functions) sections below, the intention being to give you just sufficient information to get you started. Full, exhaustive detail is provided (or will be once it is written) in a Detailed Manual page.

### Help Menu
![image](https://user-images.githubusercontent.com/3001893/127768528-49451473-472b-4ba5-8151-8c427af0ca1a.png)

The Help menu mainly provides browser links to a variety of different web resources that can provide you with help or where you can ask questions related to MeerK40t. My experience (as a user) has been that the developer of MeerK40t (@Taterize) and other long-time users of MeerK40t (of which I (@Sophist-UK) am **not** one) are both extremely knowledgeable (not only about MeerK40t software, but also about the K40 Controller) and very helpful - and that any bugs reported are taken seriously and fixed quickly.

* Help - A link to [this Wiki](./)
* Github - The [repository](/) for the source code for this (open source) project, and where you can log a [Github issue](/meerk40t/meerk40t/issues) for any bugs or enhancements you would like. **Please note:** Github issues are for confirmed bugs and feature requests which have been already been discussed with experts and are **NOT** a place for asking "how do I" questions. You will need to be registered with Github to create a new issue or comment on an existing one.
* Releases - A link to the [Github Releases page](/meerk40t/meerk40t/releases) where you can download the latest full release or Beta version.
* Facebook - A link to the [K40 Meerk40t Facebook group](https://www.facebook.com/groups/716000085655097) where you can ask questions of other MeerK40t users.
* Makers Forum - A link to the [MeerK40t category on the Makers Forum](https://forum.makerforums.info/c/k40/meerk40t/120), a bulletin board for various types of craft technology including lasers, K40 lasers and software for K40 lasers. Another place to ask questions of other MeerK40t users.
* IRC - A link to a web-based IRC client which will take you to the [#MeerK40t channel on the Libera.Chat IRC network](irc://irc.libera.chat:6665/#meerk40t), and where the MeerK40t developers and expert users hang out. If you cannot find help elsewhere, please drop by and ask for help here.

### Languages Menu
![image](https://user-images.githubusercontent.com/3001893/127769144-e74c5205-51d6-43d3-ab87-4acbd0ccc930.png)

This menu allows you to select a language that MeerK40t menus, etc. will be displayed in. MeerK40t is developed in English, but is fully enabled for other languages as and when translation files are provided for it.

The developers do **NOT** speak languages other than English, so it is up to users of MeerK40t to provide the text for other languages - but if you do so, then the developers will certainly include the translations in the next release.

If you would like to have MeerK40t in your native language or to contribute back as a thank you for the benefits you get from this free software, [instructions can be found here](./Tech:-Foreign-Language-Translations).

## Functions
### Tree
![image](https://user-images.githubusercontent.com/3001893/127768117-9b382747-85dd-47f1-833b-8318712d35fd.png)

The **Tree** Pane is a vital pane when using MeerK40t interactively using the GUI - many of the burn controls you need are only available using the GUI from here, so it is unlikely that you would want to close this pane.

As you can see from the above screenshot without a project loaded, the Tree has two main branches:
* Elements - When you load a file, the file, its groups and the elements contained (paths, images etc.) are displayed in a hierarchy here.
* Operations - Elements are classified into burn Operations, and the Operations and the elements they contain are shown here. Elements can be contained in zero, one or many Operations.

When you have a file loaded, the tree (when fully expanded) looks more like this:

![image](https://user-images.githubusercontent.com/3001893/127768240-6317b2b9-6094-46e7-8604-c6e1478420ac.png)

When you click on items in the Tree, the equivalent elements will be selected in the "scene".

There are many actions you can take by right clicking on either an item in the tree or on the selected item in the "scene" - the actions available to you will depend upon the type of the item you are right clicking against.

### Navigation

### Position
### Ribbon
### Toolbars
### Stop
### Pause
### Home
### Notes
### Spooler
### Console
### Devices
### Camera
### Execute Job
### Simulate
### RasterWizard
### Controller
### Config
### Settings
### Keymap
### Rotary
### USB

---
### Authors
The MeerK40t team is grateful for the help from @Sophist-UK in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃