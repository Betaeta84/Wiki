# Overview
MeerK40t is laser cutting software. It is intended to control a laser and provide cutting and control capabilities for lasers. Currently only Lhystudios devices are supported. This is the M2 Nano and previous revisions in the same board line. These are ubiquitous with regard to the K40 laser cutters.

# Positioning

MeerK40t focuses on a WISWYG manipulations of projects. The Main Window scene provides a view that is supposed to represent the bed of your laser. This can be set in Properties and is used in different ways. First the size of the view is strictly tied to your bed view. The camera alignment expects that the area in the camera perspective is equal to the size of your bed. If you have a different home location such as in your upper-right, then the origin point is `0, <bed_width>` and projects will position themselves accordingly.

The positioning of your project is relative to your home location. If you position two projects in the scene that is where they will be lasered relative to your bed position. Your laser-head position has no effect on where these projects are located. MeerK40t constantly tracks your laser head location and does will travel from one location to another location within the scene-space. You do not need to home between projects or move the laser head manually. 

The one exception to this is the alternative home location. If set in properties that location is home and will be located in the upper left hand corner of your scene. If you use a jig or other method of setting an position to the same location, this will become position 0,0 and jobs should be run in the upper left hand corner of the scene directly. When homing the device will perform a normal home location, then change position to the alternative home location, it will then call that position (0,0) so tell the laser to move to a set location this will move relative to the set alternative home location.

# Elements

There are three major elements MeerK40t deals with.

## Paths

Paths are outline specific shapes within the space. These are vector paths and they are stored in native formatting. So arcs and bezier curves and lines will all be kept in their respective shapes without linearization or modification. MeerK40t has a point-perfect native shape plotting routine which allows the perfect native rendering of such curves. This element largely defines vectors which are processed in two different operations: `Engrave` and `Cut`. Paths with fills or merely outlines can also be done in the `Raster` operation. (See operations for more information). This requires that the post-processing of the Paths turns them into images of their paths.

## Images

Images are rasters. This means we are dealing with a clump of actual pixels. A rectangle based structure of different color values. This does not provide any information of the directionality of things. Just the colors in order in a rectangle. We cannot therefore do any vector operations on an Image except to draw the rectangle. This is actually what MeerK40t will do if you try to `Engrave` an image. Images are therefore highly suited for the operations `Image` and `Raster` (See operations for more information). Since we can only use the pixel information to perform laser operations.

## Text

Text is a message and a font. This means we are dealing with actual text and the means to turn that text into paths. Currently MeerK40t lacks this capabilities. Text can be turned into a raster and rastered directly with MeerK40t. MeerK40t can alter what the text says and the fonts. There are currently some positioning issues within MeerK40t that provides inconsistent results here. If Text is set within a vector operation such as `Engrave` or `Cut` it will preprocess to strip that from the operations. These should be text-to-path in whatever designing software is being used. 

# Operations.

## Classification

Operations are used to classify elements. The data we are working with are Path, Image, and Text objects (some native shapes may be added later), and the operations themselves are the processes the laser will do with that information. You could `Raster` or `Cut` a path, and this ambiguity means we need to know what operations should be done with what elements. The same element can also exist in several different operations. The primary method of classification is through stroke color and existence of fill. The Engrave operations have colors, this is only used during classification and simply allows you to automatically have an element classified into your particularly desired operation type. `Raster` operations catch anything matching their respective color and anything that is a closed and filled shape.

Classification occurs from top to bottom. Where the first matching operation will contain that element, except if there are more than one Engrave operations that match the same color. Then all of these operations will get all of those paths. If a path is not classified before running out of operations. A new operation will be created and will possess that path and all other paths with that same stroke color.

## Engrave

The default vector operation is engrave. This tends to imply the intent is to laser a line into the material according to some set path information. This requires directional information so it must be a path object (or a text, but that doesn't work). This operation can be set in the OperationProperty window which can be accessed by double-clicking an operation.

## Cut

A cut is vector operation that is intended to cut through the material. Codewise this is almost identical to an Engrave operation except that post processing ordering requires that the inner cuts must occur before the outercuts. If this isn't a problem for you, you can just run very slow engrave operations with the intention to cut through the material.

## Raster

A raster is done in a back and forth (or up and down (or both)) motion going over a series of pixels. This is done in a series of scan-lines. The raster-step changes the stepping between different scanlines. So the default is 1 mil of movement, however for most images 2-4 steps is likely preferred. The increase in step size tends to increase the width, since 2x step size means 2x the height, to maintain aspect ratio this means becoming smaller. Rasters natively have a step-size setting that can set this step size. Images generated will very often reflect resample themselves to be the correct size for the set step-size. Any raster operation is done in parallel. All images, shapes, text, is turned into a single internal image and rastered.

## Image

An image operation is a raster but rather than relying on the internal step-size within the operation we rely on step-size within an image. This is often set by RasterWizard or by the user manipulating the set-size. Any images in an Image Operation are does in series. This means each image is processed in order each image gets its own rastering at its own step-size. The stepsize is set to Passthrough which means that the images step-size is used. This is to make sure nothing requires images to resize or change after they've been processed. This is because resampling an image that has already been dithered and converted is a serious problem with regard to quality. We seek to avoid this issue.

# Cutcode.

Cutcode should be introduced in 0.7.x and is a modified rendered operation performed on elements. This means that it is encoded with speed and property information already and is laser-ready. This object works as both an Operation and Element since it contains both already mostly rendered together. This sort of intermediate object is needed for various interpretations and emulations of other laser formats. For example running Ruida-code for a Lhystudios device requires we set our speed and other settings based on Ruida's data. Currently the `ruidaserver` emulation takes the shapes only since there was no method of taking other objects.

# Windows

## Main Window
The main window has two sections, the tree which allows various project manipulations and the scene which allows direct visualization and modification of the project. 

![wxmeerk40t](https://user-images.githubusercontent.com/3302478/102618581-e9466c00-40ef-11eb-90d3-cd60a4b25970.png)

The second tab in the main window provides Position capabilities where you can directly set the position of a selected element.

![wxmeerk40t-2](https://user-images.githubusercontent.com/3302478/102618567-e21f5e00-40ef-11eb-9e28-d7bcd7f97656.png)

## Job

Starting a particular job. This window has two main sections the things being run and the preprocessing required to run them. For example, if you have rotary enabled then the job may need to be properly transformed to run. If you have a raster operation it will need to be converted into an image. If you have an image whose size is the merely stretched and transformed rather than made of actual pixels, this will need to be actualized. If you have a cut operation, the inner cuts must be processed first so the operation requires the optimization be run.

![Job](https://user-images.githubusercontent.com/3302478/102619045-b2bd2100-40f0-11eb-8afe-2dcb2081f52f.png)

## Navigation
Controlling and aligning projects. There are three main batches of command in the dialog. The first is device position alone. This is the location of the device and controls related to the device without regard for any other aspects. The middle is the project and its relationship with the laser. So you can position the laser position to the various corners do a convex hull outline, move the project and laser together in sync, and other project alignment features. The final control is project-only. You can scale, rotate, move, directly view the matrices of the selected element.

![Navigation](https://user-images.githubusercontent.com/3302478/102618586-eb102f80-40ef-11eb-9e64-a052fadaa66a.png)

## UsbLog

Viewing what's going on with the USB connection for troubleshooting. The USB connections troubleshooting is usually different and problematic, so every step of the detection and connection process is documented and logged. This should provide some considerable help in determining exactly where the problem is with the USB connection. This window tends to display the `usb` channel. In console you can view the channel with `channel open usb` or you could do `channel save usb` to save any usb log to disk.

![Usblog](https://user-images.githubusercontent.com/3302478/102618589-ec415c80-40ef-11eb-81af-a4748cb0ad1a.png)

## Controller

Viewing detailed information about the controller state. Allowing pausing and aborting. This gives real-time highly detailed information about the controller and the controller state. What data is being sent, how much is currently in the buffer, how many packets have sent, how many packets are being rejected. And lets you send realtime commands to change the state of the device. Like pausing the device and aborting the current job.

![Controller](https://user-images.githubusercontent.com/3302478/102618599-f06d7a00-40ef-11eb-8408-e56898ce3385.png)

## Spooler
Job Spooler. Shows data currently in the process of being sent to the laser for cutting. This includes time estimates and some current work. Generally this isn't very helpful.

![spooler](https://user-images.githubusercontent.com/3302478/102618592-ee0b2000-40ef-11eb-90ae-81324953f506.png)

## Preferences

Device Specific settings and adjustments. This is stuff like the USB connection to the device and certain behaviors with regard to the device. These would be likely switched out for a different dialog if there were a different backend than `LhystudiosDevices`.

![Preferences](https://user-images.githubusercontent.com/3302478/102618603-f19ea700-40ef-11eb-8003-455b0eb569db.png)

## Settings

Program specific settings. Optional behaviors. These are some behaviors and characteristics that exist program wide and should not require much alternation or changing. They tend to define certain things that cannot be ascertained in other ways, like what program do these SVGs come from and what language should be used.

![Settings](https://user-images.githubusercontent.com/3302478/102619747-c7e67f80-40f1-11eb-95f9-9a1ac155abcb.png)


## RasterWizard

Preprocessing raster settings for image preprocessing. This comes with a few different pre-established scripts which were the sum of the recipes that could be determined. The general consensus is that `gravy` is likely the best. There are some arguments to be made for `newsy`. If you have your own recipe that you'd like included into MeerK40t you may raise an issue.

![RasterWizard](https://user-images.githubusercontent.com/3302478/102619740-c61cbc00-40f1-11eb-9f7a-fd99e263d091.png)


## OperationsProperties

Properties for the selected operation. You may change the various operation properties here and likely should use this often. The most important factors here are the type of operation being conducted and the speed. For rasters, the directionality of the raster is highly important.

![OperationProperties](https://user-images.githubusercontent.com/3302478/102618646-02e7b380-40f0-11eb-9398-361620c41db4.png)

## PathProperties

Properties for the selected path. This is largely the fill and stroke values for the paths. Changing the stroke value or fill value should reclassify the element. This could also be done in console with the `fill` or `stroke` commands. Also `alt+s` and `alt+f` on the keyboard should load up the internal color changing dialog and allow colors changes. This corresponds to `control fill` and `control stroke`

![PathProperties](https://user-images.githubusercontent.com/3302478/102618649-0418e080-40f0-11eb-9d5a-1e6dabe976b3.png)

## Image Properties

Properties for the selected image. This is largely just a bit of position info and the actual size information for the image itself.

![ImageProperties](https://user-images.githubusercontent.com/3302478/102618652-05e2a400-40f0-11eb-9739-ce122b9016eb.png)

## Text Properties

Properties for the selected text. Change the text, size, font, stroke, and fill color of particular text properties. Currently this works for rastering only since text objects cannot have their vector paths determined. If this is the desire the document should be text-to-path converted. This will then not be a text object.

![TextProperties](https://user-images.githubusercontent.com/3302478/102618656-0713d100-40f0-11eb-9dad-e3a4cf9a846c.png)

## Devices

Device Manager to control what devices load up. (Mostly don't mess with this). There's some desire to have alternative backends for the program which could be used with other controllers. The boot factor is switched with a right mouse button press and if you have two devices in here to boot, then both devices will boot and start two main windows. This functionality should be changed in 0.7.x

![Devices](https://user-images.githubusercontent.com/3302478/102618612-f5cac480-40ef-11eb-8541-992453de0baa.png)

## Camera

CameraInterface window is for viewing k40 camera feed, and controlling various camera related aspects. The interface has a number of primary functions. Fisheye distortion correction, which is used to straighten edges rounded by camera lens aberrations. Send frame as background to the scene, or an image in elements. The frame to background works, but without the fisheye correction this isn't wildly helpful. Also, this function will require the bed size be set accurately, as well as perspective (cut area) be adjusted/cropped.

![Camera](https://user-images.githubusercontent.com/3302478/102618615-f7948800-40ef-11eb-950b-3ae6689bd383.png)

Camera window contains a number of controls, as well as a camera selection menu when open (the Camera selection menu is in the menu bar on a Mac when the camera window is open and in the foreground). 

A. Camera Selection Menu
Reset Perspective: Resets the perspective handle settings to default.
Reset Fisheye: Resets the transformation matrix generated by the Detect Distortion/Calibrate which is used to correct fisheye.
Set IP Camera 1-3: Enter a URL like rtsp://username:PASSWORD@IPADDRESS/path or http://username:PASSWORD@IPADDRESS/path 
Set Camera 0-4: Select USB camera index 0-4. This may require allowing permission to access each camera in Mac OS.

B. Update Image Button
Sends current camera view to the bed background. This can be used after all initial setup steps have been completed. 

C. Export Snapshot Button
Sends the current camera view as an image into MeerK40t's Elements. That can be added to an operation, or saved as an image inside .svg file by saving the resulting file with File->Save as.

D. Correct Fisheye Checkbox
Will apply the matrix generated from the Detect Distortion/Calibration setup. 

E. Correct Perspective Checkbox
Crops and skews the camera view to fit the perspective handles. This also changes the aspect ratio to match the bed size set in MeerK40t's Preferences.

F. Camera FPS Slider
Changes the frames per second from the camera that are shown in the Camera window. This can be useful on slower systems; especially if camera is also used to monitor a running laser job.

G. Detect Distortions/Calibration Button
Looks for a 6x9 grid to allow the openCV backend to do remove lens aberrations from the image. That allows the overhead camera to be more accurate; and the edges of the bed to appear straight instead of bowed out from the lens view. This can help significantly improve position accuracy at the extreme edges.

H. Perspective Handles 
Click and drag in the middle of the handle to move them. Use the mouse wheel to zoom in and out on the image, and hold down middle mouse button (wheel) to move around image. You may need to zoom or resize the window to see the perspective handles depending on resolution of the camera and computer display. 

## Keymap

Key binding. Bind a specific key to a specific console command. This is also accessed with `bind` in console. There may also be a need for an alias command which binds multiple commands together and have hooks for the keydown and keyup, if the alias starts with `+` for the keybound alias then automatically the `-` alias will be attempted on keyup. These commands are all console commands which can control most aspects of the program.

![Keymap](https://user-images.githubusercontent.com/3302478/102618620-f95e4b80-40ef-11eb-8924-bd9592d0e3dd.png)

## Notes

View and edit notes on the current project. This saves out to file and starts on loading of file. These could also be altered with the command `note` in console.

![Notes](https://user-images.githubusercontent.com/3302478/102618625-fbc0a580-40ef-11eb-8808-262d0acf20fc.png)

## Rotary Settings

Rotary settings. The current state of Rotary Settings is that if the rotary button is checked it will add into the pre-processing for jobs a stretching command. Also the scale_x and scale_y factors are used for the `rotaryview` and `rotaryscale` functions which can be accessed with that command in console or `alt+F3` and `control+F3`. The rest of the settings are disabled and don't do anything. Their intent was to transform the design space into a non-Euclidean geometry, and to help do the math calculations for rotaries.

![RotarySettings](https://user-images.githubusercontent.com/3302478/102618632-fe22ff80-40ef-11eb-9320-aa2ca316a8b7.png)

## About

About Page.

![About](https://user-images.githubusercontent.com/3302478/102618638-ffecc300-40ef-11eb-865d-beaa503d304d.png)

## Alignment Ally

Alignment Ally is intended to help with the process of aligning the laser. It has a series of tests that can be run. It's not however very helpful. And may be removed in future versions.

![Alignment](https://user-images.githubusercontent.com/3302478/102691817-3136c300-41c4-11eb-9eb7-a5d0861f7912.png)

## Adjustments (Deprecated)
Adjustments. I don't think any of this stuff does anything anymore.

![Adjustments](https://user-images.githubusercontent.com/3302478/102618641-01b68680-40f0-11eb-86cc-619e4550313f.png)


# Tree Menu Items.

![operation-menu](https://user-images.githubusercontent.com/3302478/107894279-10e56500-6ee4-11eb-9a67-153a5f35e1f3.png)
* Clear All: Removes all Image, Raster, Cut and Engrave operations from the list. The items will remain in Elements, as well as File.
* Reverse Layer Order: Will reorder the Operations in the tree; to effectively run the job backwards.
* Refresh Classification: 
* Set Other/Blue/Red Classify: Uses the standard Red color for cut, Blue color for Engrave, and Black/Any other colors for Raster engrave. 
* Set Basic Classification: Uses a rainbow of colors for discreet operations which run in a specified order. Along with Black/Any other color classing as Raster engrave. 
* Add Operation: Adds a new unclassed operation to the bottom of the list of operations in the tree. Double clicking it will default classify as Engrave, and allow setting the operation type, color, settings, etc. If untouched, it remains a Unclassified (Unknown operation classification), and unused.
![Image-Op-Menu](https://user-images.githubusercontent.com/3302478/107894282-12af2880-6ee4-11eb-97ba-3370d05e70c1.png)
* Execute Job: Opens the Job window with only the chosen Image operation loaded; along with any pre and post actions selected in the Job window already. Other operations are excluded from this laser job; even if they remain enabled in the tree.
* Clear All: Clears all of the image entries in the selected operation. They will remain in the Elements and Files entries of the tree.
* Remove Image(Operation): Removes the currently selected Image operation from the tree. Images classified inside remain in Elements and Files entries in the tree.
* Convert Operation: Coverts the selected image operation into a different type; Raster, Cut, or Engrave. Images aren't 
* Duplicate Operation: Makes a copy of the Image operation in the tree. The items contained remain linked; as a single reference of the same image element. The new duplicated Image operation will show up at the bottom of the list of operations in the tree, and can contain settings different than the operation it was cloned from.
* Passes: Adds selected number of extra references to the image inside selected operation.
* Step: Sets the number of steps in Y for each horizontal cuttings pass of the laser head (raster). A higher number will run faster by rastering less lines. 
* Make Raster Image: Makes a single image by combining all images in the selected operation. The individual images remain separated in the Elements and Files.
![engrave-operation-menu](https://user-images.githubusercontent.com/3302478/107894284-13e05580-6ee4-11eb-8cb0-1054956746d7.png)
* Execute Job: Opens the Job window with only the chosen Engrave or Cut operation loaded; along with any pre and post actions selected in the Job window already. Other operations are excluded from this laser job; even if they remain enabled in the tree.
* Clear All: Clears all of the Engrave or Cut entries in the selected operation. They will remain in the Elements and Files entries of the tree.
* Remove (Operation): Removes the Engrave or Cut operation.
* Convert Operation: Converts the operation to Raster, Cut, or Engrave.
* Duplicate Operation: Creates a new operation of the same type at the bottom of the operations section of the tree. This new operations has references to all the same objects; but can contain discreet settings from the operation it was duplicated from. It will run in the order it is placed in the tree. 
* Passes: Duplicates references to all objects in the operations according to number of passes. The extra objects will run in the same order by default. 
![engrave-element-menu](https://user-images.githubusercontent.com/3302478/107894287-15118280-6ee4-11eb-8c9f-227e538d03d2.png)
* Remove: path#####:
* Remove # objects:
* Clone Reference:
* Duplicate:
![elements-menu](https://user-images.githubusercontent.com/3302478/107894288-1642af80-6ee4-11eb-9efb-d96a0ac03bc7.png)
* Clear All:
* Reverse Layer Order:
* Reclassify Operations:
* Reset User Changes:
![element-specific](https://user-images.githubusercontent.com/3302478/107894291-1773dc80-6ee4-11eb-8462-615e96441b2d.png)
* Remove path#####:
* Duplicate:
* Reset User Changes:
* Scale:
* Rotate:
* Reify User Changes:
* Break Subpaths:
![image-specific](https://user-images.githubusercontent.com/3302478/107894294-1b076380-6ee4-11eb-87d2-560b797cda40.png)
* Remove (image):
* Duplicate:
* Reset User Changes: Reverts back to the version of the file currently stored on disk.
* Scale:
* Rotate:
* Reify User Changes:
* Step:
* Actualize Pizels:
* ZDepth Divide:
* Image:
* RasterWizard:
* Apply Raster Script:
![image-specific-post](https://user-images.githubusercontent.com/3302478/107894296-1d69bd80-6ee4-11eb-9ff2-127fb603ab0e.png)
* Remove (image): Removes image from
* Duplicate: Creates distinct copies of the image which can be manipulated individually. 
* Step: Change in Y steps for each next line of laser raster.
* Actualize Pizels: Writes changes based on previous manipulation.
* ZDepth Divide: Divide image into multiples; each with a range of tone based on the number requested. Cutting these images in order results in a Z-Depth Raster.
* Image: 
* RasterWizard: Open RasterWizard
* Apply Raster Script: Apply Raster Script from defaults. 
![file-menu](https://user-images.githubusercontent.com/3302478/107894298-1e9aea80-6ee4-11eb-93b1-779bcdb38aa5.png)
* Remove: Remove the file from MeerK40t project.
* Reload: Reload the file from disk. 