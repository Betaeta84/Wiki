# Rasterization

MeerK40t includes a RasterBuilder. It uses a highly debuggable methodology to build rasters. This gives MeerK40t the ability to overscan, perform bottom-to-top right-to-left rasters, start from any corner, skip blank edge pixels, skip blank edge lines and makes the entire process extremely easy to troubleshoot or extend.


# Scaling
MeerK40t does not scale the images for you. The pixels correspond to at best resolution a 1 pixel to 1 mil conversion where each pixel is 1/1000th of an inch.

# Background
Rastering on the K40 or any laser cutter requires cutting bands of pixels with alternative on and off values related to value of the pixels to create an image.

# Raster Step
The raster step value, allows you to step a different amount down in the raster. So rather than 1 mil steps you can do 2 or 3 or 4, up to 63 are permitted (by hardware) but that is a bit extreme. Now, to keep the aspect ratio, when you step by 2 mils in the y-direction you must equally step by 2 mils in the x direction. This traditionally means that you'd have values of 2 pixels on or 2 pixels off. This step in the x-value equals the raster step value. 

Currently (Date: 9/26/19) the rasters use PPI interpolation by default. So if your image even stepped up to be larger, it has additional information within the pixels. Like they are not 100% black or 100% white they will do PPI rendering. So 50% pixels black will produce black pixels that are 50% on and 50% off. See [[How does MeerK40t's pulse modulation works?]] for more information. The important point here is that PPI will cause the scaled up pixels to modulate. So if you have a step size of 4. You will not have 2 types of pixels. You will have 2^4 (16) pixels that can exist, and the rest of the data will be propagated forward in a sort of dithering that underlies the pulse-modulation. 

If you have pure black and pure white pixels, then pulse modulation will carry forward no additional information and the image will be identical to the image submitted.

In fact, if we submit a dithered image or non-dithered image we could simply solve the non-dithered image in terms of the dither. Since a black and white image will fire or not fire the laser. And the PPI raster propagated image will either fire or not fire the laser. We can actively show that PPI power modulation is going to be identical to submitting a purely 1 bit image that is pre-dithered.

# Scanline difference
There is a going to be a difference between a 2000x2000 pixel image and a 1000x1000 pixel image with a raster step of 2. The 2000x2000 image will perform 2000 scanlines of 2 inches. Whereas the 1000x1000 image will perform 1000 scanlines of 2 inches. While PPI can feed can modulate extra information into the longer x-movements. It will do nothing for the scanlines which may actively qualify as information loss. This may also reduce the heat input by half (making the image lighter). It would be entirely possible to perform this operation without changing the lightness of the image if we ensure the heat input is equalized. Basically by counting up the number of black pixels and making sure they are equal.

If use image editing software to make an image 2x the size. You will have on average the same number of black pixels. Since 1 black pixel becomes 4 black pixels (since you increased length and width). If you increase the size by a increasing the raster-step you will make 1 black pixel become 2 black pixels. On average this means the average amount of laser reaching the medium (the heat input) is exactly half.

That is the reason why changing the raster step to 1 makes images seem darker, because they are. But, they are also more accurate.

# Good/Fast results?

Just drag and drop an image into the scene. Use raster step to scale the image typically 5 or below. And run it.

* You do not need to convert it to grayscale.
* You do not need to dither.
* It will apply PPI power modulation based on the extra information and will result in what basically amounts to dithering anyway. Time to raster will be basically none. And it give you better results than some people ever achieve using other methods.
* Control darkness with raster speed.


# Best results.

If time is no concern and you want the absolute best results your better off scaling the image you want to use to be exactly the size you want. Using 1000 px = 1 inch (the native K40 units given the 1 mil step distance of the stepper motors). Then you need to make sure that the amount of pixels you are putting in isn't too dark. And do the dithering yourself with a good image editing program. With only black and white pixels there is PPI becomes moot. But, the kind of dithering PPI is merely a carry forward of the current scanline and could be easily inferior to the types that diffuse the error in multiple directions.

Darkness is best controlled here with the speed of the raster. So what would be a darker image is better off being an image done faster. Since the speed of the laser head will also provide you with means to control the overall heat input to the material.

* 1 raster step.
* Scale the image beforehand to the size you wish it to be on the physical medium.
* Perform the dithering yourself in a graphics program.
* Use the raster speed of the laser to control darkness of the image.
     * This should be true between different mediums as well. Other greyscale conversions etc, will change the pattern of the dots. It won't actually have other effects.

*Note*: This is the advice from math and physics From science, your results speak for themselves.

# Need a larger image?

If you need to scale your image. You can increase the step size. But, see Best Results and other information about what that larger image actually means.

Raster Images are a particular size and resizing in any other manner them would require inventing additional pixels. While you can scale up through the step size you can't merely apply a matrix to the image. Since we're dealing with 1:1 pixels:mils changing it would data and require changing the image itself to change its size through other means.

MeerK40t currently only uses the native resolution of the device which is currently 1 pixel is 1 mil. So 1000 pixels wide is an inch. You can scale this up by the step factor (with the heat input caveat, 2x step factor will become 1/2 as light, whereas scaling the image by 200% will leave it the same darkness).

So if you want your image to be 10 inches across. You can have a 1000 pixel image with a step size of 10 (I wouldn't recommend that),  or 10000 pixels across.

Note: MeerK40t does not preprocess your image. It reads the image while it's creating the commands for the controller. This is done in a single process. So larger images create no additional overhead. See: [[On the Fly Processing]].

# Why doesn't my image scale?

MeerK40t is making a conscious choice to not scale the rasters because they don't scale in a traditional manner.

If you want a larger image you are much better off editing the image in an image editor program before hand. You are better off giving MeerK40t all the pixels you can. You can make choices for the step size and the scale factors that results in and the ultimate size of the image. But, you *must* be deliberate about that. 

If you have an image that is 1000 pixels wide, MeerK40t will not be able to make that 1.5 inches wide.
You will get:
1 step: 1 inch
2 step: 2 inches
3 step: 3 inches.
Etc. 

If you want to scale that up to 1500 pixels with image editor software, you can. And it will be 1.5 inches wide. Or you could make it 750 pixels wide and you can run it with a step value of 2 and it will be 1.5 inches wide. But, you must make these choices deliberately. In the future, MeerK40t may help you there with some image manipulations but they will still require deliberate choices.

# Isn't that restricting?

Yes. I'm restricting the user to making some deliberate choices that will keep and maintain the best results.

I spent quite a while doing image to Lhymicro-gl coding and this gives the best results. But, it might be really annoying if you try to scale up the image and it only scales up that stupid rectangle I put around the image. (9/26/19). Mea culpa.

PPI rastering lets you basically end up with good results out of the box. See the Good/Fast Result section.

---

If you have better ideas for how to do this, or disagree raise an issue.


# Stuttering

Rasters sometimes stutter. This is cause by the M2 controller being able to execute all the commands in its very small buffer before it can get new commands. There's an absolute speed at which data can be sent. And this is nearly always enough that controller takes longer to execute them than receive them. This is sometimes not true for rasters. Especially when the typical draw size of the pixels is 1. The command required to do 1 pixel is `DBa` the command to do 15 is `DBo` (and takes 15 times longer) and when it's doing a lot of these pixels and the laser head is going fast enough it can constantly run out of commands. Before MeerK40t information feedback what was happening was not clear. But, when it bottoms out of the buffer the device stutters and sending a BUSY signal for a second. Even though clearly the buffer in the program has enough data to send.

This may be sort of scary for some people as this sudden busy signal in the data is not caused by the sending of packets. And *could* have resulted in K40 Whisperer losing a packet prior to version 0.33 and completely destroying a rendering. But this was due to a protocol bug was fixed after I pointed it out in an email to Scorch.

These are actually fine and will cause no major issue, but may harm overall speed. You could ironically slow down the rastering speed and the commands will take longer to execute and so it can refill its buffer before this bottoms out. But, these were only scary due to a bug, they are accounted for and corrected in the M2 firmware.

# Scan Lines

If you use raster-step/scangap alone to scale up an image in addition to decreasing in overall darkness (since each additional rasterstep you can also get scanlines in deeper rasters. Since the scanlines are farther apart, the laser cuts are farther apart and there's room to create laser cuts that do not overlap. 

---
### Authors
The MeerK40t team is grateful for the help from @tatarize in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃