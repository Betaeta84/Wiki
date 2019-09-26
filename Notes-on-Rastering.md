Rastering on the K40 or any laser cutter requires cutting bands of pixels with alternative on and off values related to value of the pixels to create an image.

MeerK40t does not scale the images for you. The pixels correspond to at best resolution a 1 pixel to 1 mil conversion where each pixel is 1/1000th of an inch.

The raster step value, allows you to step a different amount down in the raster. So rather than 1 mil steps you can do 2 or 3 or 4, up to 63 are permitted but that is a bit extreme. Now, to keep the aspect ratio, when you step by 2 mils in the y-direction you must equally step by 2 mils in the x direction. This traditionally means that you'd have values of 2 pixels on or 2 pixels off. This step in the x-value equals the raster step value. 

Currently (Date: 9/26/19) the rasters use PPI interpolation 

If you go into the properties and increase the step size it will scale up in size. At issue is the fact that Raster Images are a particular size and resizing them would require inventing additional pixels. While you can scale up through the step size you can't merely apply a matrix to the image. Since we're dealing with 1:1 pixels to encoding it would lose resolution or require changing the image itself to change it's size through other means. Currently it uses the native resolution of the device which is currently 1 pixel is 1 mil. So 1000 pixels wide is an inch. You can scale this up by the step factor.

I'm making a conscious choice to not scale the rasters because they don't scale in a traditional manner and I do need to make it clear that that is what I am doing and why. If you want a larger image you are much better off editing the image in an image editor program before hand.

If I take that and make it a size for the user, it could do weird things. And so I held off on actual image manipulations until later. I could certainly do them. I have access to PIL. But, it needs to be explicit that this is the native size of the image. I feel like that's something Whisperer got wrong.