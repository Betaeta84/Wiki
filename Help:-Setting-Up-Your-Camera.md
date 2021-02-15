1. Correct for camera fisheye. Use the 6x9 checkerboard image for printing, it can be printed at a reduced size: about 3"x5" works fine. Perform Distortion correction at all four corners, and bed center; be sure it accepted the checkerboard each time. Toggle the Correct Fisheye Checkbox to see the difference, and ensure all corners are have an adjustment applied. Then leave the Correct Fisheye Checkbox checked. This is necessarily.

2. Get the bed size accurate. A couple ways around this: 
     A. You used an object of set size. Say an A4 sheet of paper, and set that as your bed size, that would do it. 
     B. Also you could just put something in the cutter larger than your bed size, and burn it to the size of the bed. Even Post-It notes can be used for this.
     C. Terminal command rect 0 0 100% 100% will create a rectangle from point 0,0 to 100% width and 100% height of your set bed size. 

3. Mark or laser engrave specific marker locations for all four corners. Set perspective markers at the exact corners (limits), which would match up corrected size vs. expected size. Those markers placed are there to solve correctly for the perspective based on camera position. For this to function properly, one needs to have a fine tuned bed size already. Check the Correct Perspective Checkbox.

This would correct most of the snag points which is largely; that the bed size must match the physical size between the perspective points, which is notably kind of mysterious.

Camera Placement: Mounting, aiming, 
Ensure Camera View is Reproducible: Use kickstand, spirit level, lid stop, 

Questions about Camera Support:

> Locally connected USB OTG UVC compliant type camera? Examples of known to work?

Also, webcam rtsp and http connections work.

> I have the following on order and will be mounting it on the Lid somehow....

There's no documentation recommending any mounting location or how to mount it.

> Where do you find the 6x9 Grid Document page?

![pattern](https://user-images.githubusercontent.com/3302478/95668169-14777280-0b25-11eb-9342-044f683f3b96.png)

> How do you "calibrate" the camera (steps, please)

* Load if your camera has fisheye, then print the checkerboard pattern.
* Use checkerboard pattern firmly glued to a flat surface. The paper can't be bent or curved.
* Use the detect checkerboard button to teach it to get rid of the fisheye.
* Use a standard piece of A4 or any standard sized object in your bed.
* Set your bed dimensions to the size of that object.
* In CameraInterface, move the perspective marks, zooming and panning in the camera view as needed, to mark the corners of the standard sized object you have in there.
* When you've marked the corners you have correct perspective and correct fish eye checked. You're done. It will be pretty close.


> How do you connect to a camera if it says "Camera not found" ( 'Windows>Camera' seems to work MUCH better than 'Camera' for startup but is still not 100%)

Try the beta. 0.6.15 beta-4 or so. This will try hammering the camera. Some windows cameras don't produce a frame at the very start and are wrongly truncated.

> "Update Image" does what? Copies image to the workspace,

It pastes the currently marked image to be the marked bed location.

> How do you CLEAR the image?

You can't.

> NEW doesn't.

New doesn't.

> The image copied onto the workspace doesn't show up on anything.

It's supposed to be below everything and is drawn as such. So you can position things on to whatever material you have placed in your bed.

> Is this typically the Image of the laser Workspace that will be used for alignment? If so, please say that. I still need a way to "remove" it... (Hide Background?? if so, please say "to remove...")

This is actually kind of typical. But, it's not used for alignment this is used for positioning an already aligned object.

> "Export Snapshot" Takes the currently selected camera and copies the snapshot onto the workspace.

That is correct.

> "Correct Fisheye" How, what does it need to do that (the 6x9 image placed on the Laser Bed?)

In several different spots, without any the paper being curved or bent. It trains the screen to undo the fisheye perspective.

> "Correct Perspective" How, what does it need to do that (the 6x9 image placed on the Laser Bed?)

It doesn't care about the checkboard at all. If you zoom out there are perspective markers. These are set at the corners of the bed. Though a standard candle would be preferrable. Though you could also laser holes in a postit note in the corner and mark those positions.

> "Slider" What does it do? What does it need to do that?

The slider sets the frames per second. It does that because sometimes you might not care if the camera updates every few second or is realtime.

> "Detect Distortions/Calibrations" What does it do? What does it need to do that (the 6x9 image placed on the Laser Bed?)

Yes. That requires the 6x9 checkerboard.  In view of the camera, and flat. It doesn't really need to be a particular size and it's for undoing fisheye perspective.

> Reset Perspective - Why would you use this?

Because you moved your perspective markers somewhere weird and figure starting over is better.

> Reset Fisheye - Why would you use this?

Because the fisheye did something weird and is giving you a garbage view and starting over is the only thing you can do to save yourself from whatever phantom math realm of wrongness your view is stuck in.

> NEED " Remove Workspace Image"

Yes. I'll add that, I'm about to address camera for 0.7.0 which was supposed to have a camera upgrade circa 0.6.3 but it kept getting pushed, and some of the kernel api changes were perfect for fixing that.

> NEED "Flip Image V, Flip Image H"

I suppose. But, you can do that with the perspective markers. Put the right ones on the left and left ones on the right or top ones on the bottom etc and even an upside down camera will be fine.

> Set IP camera 1 - Why would I use this. Needs to be what type of camera. What about camera logins?

Well, an IP camera. You would use it if you have a RTSP or HTTP camera. Login cameras should be included in the url with the username and password context as part of standard urls. `protocol://username:password@website.com`

> Set Camera 0 - Why would I use this. Needs to be what type of camera (locally connected USB??)

Locally connected USB camera, or built in laptop camera.

> (Why does one start at 0 and the other at 1?)

Because the 0 at the camera is the local OpenCV code whereas I chose the indexing for the different RTSP/HTTP stream IP cameras and started counting it at 1.

> (Would it be better to Say Network Camera #1 and Local Camera #2, etc...)

Yes. That would make better sense with regard to UI and interfacing. Though I will be likely shifting these slightly in the 0.7.x camera upgrade but those are much more cogent and better methods of addressing that.

> Is "Calibrate" another word for "align"

No. Calibrate refers to adjusting the fisheye correction whereas align means actually getting all aspects correct, bed width, perspective, and fisheye.

> Are there other "tricks" that would be helpful in calibrating/aligning the camera?

Yes. You can use a standard sized object and set your bed size to the size of that object. Then set the corners to that object. Or if you want your entire bed working and in view. You would place post it notes in your corners. Then add tiny circles or a full rectangle to outline your bed. In console these commands would be `circle 100% 0% 1mm` to make a circle at the upper right hand corner. Then go ahead and cut that in a postit note. Or `move 100% 0%` would take you to the same corner and just hit your test button with a post-it note in that corner. Then mark each of your corners with perspective markers. Then you'll have it correctly lined up. 

> If the answers to these questions are put on a "Help" page it would -help- a LOT...

Hm. I suppose.

https://github.com/meerk40t/meerk40t/wiki/HELP:-Setting-Up-Your-Camera.