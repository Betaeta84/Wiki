# Operations
Operations are fundamental concept MeerK40t is using to translate geometric shapes on the scene into laser burn instructions.
In essence MeerK40t distinguishes two classes of operations:
- Burn operations - that will take shapes and burn these with some given parameters like speed and power
- Special operations - that will issue certain commands to the laser or the infrastructure around it, interacting with the user etc.

The assignment which element will 'belong' to which operation, ie will be burnt how, is called Classification and deserves a chapter of its own.

## Vectors and Bitmaps

Before we enter the realms of the individual operations, let's make sure we understand the two fundamental burn types: Vectors and Bitmaps (or Rasters):

- A vector is essentially a very thin line - it can be straight or it can be curved, but it is VERY thin - in theory it has infinitely small width, but in the case of lasering it is the width of the laser dot, which is a tiny faction of an inch or centimetre. For example, you would use a vector to describe the edge of the leather design you are going to cut out, or perhaps the edges of the letters you are going to burn onto the material. In the case of letters, the vectors would normally be one or more closed loops, and in this case the vectors might have a fill colour or gradient of colours.

- A bitmap is a 2-dimensional matrix of equal sized dots - and each dot can be any colour (or for lasering grey dots of varying darkness / brightness). Sometimes we constrain these dots to be either black or white rather than shades of grey. Photographs are held in a computer as bitmaps (in a variety of file formats), and sometimes diagrams which could be described using vectors are provided in image form as well.

## Burn Operations
### Engrave

The default vector operation is engrave. This tends to imply the intent is to laser a line into the material according to some set path information. This requires directional information so it must be a path object (or a text, but that doesn't work). This operation can be set in the Operation-property window which can be accessed by double-clicking an operation.

### Cut

A cut is vector operation that is intended to cut through the material. Codewise this is almost identical to an Engrave operation except that post processing ordering requires that the inner cuts must occur before the outercuts. If this isn't a problem for you, you can just run very slow engrave operations with the intention to cut through the material.

### Raster

A raster is done in a back and forth (or up and down (or both)) motion going over a series of pixels. This is done in a series of scan-lines. The raster-step changes the stepping between different scanlines. So the default is 1 mil of movement, however for most images 2-4 steps is likely preferred. The increase in step size tends to increase the width, since 2x step size means 2x the height, to maintain aspect ratio this means becoming smaller. Rasters natively have a step-size setting that can set this step size. Images generated will very often reflect resample themselves to be the correct size for the set step-size. Any raster operation is done in parallel. All images, shapes, text, is turned into a single internal image and rastered.

### Image

An image operation is a raster but rather than relying on the internal step-size within the operation we rely on step-size within an image. This is often set by RasterWizard or by the user manipulating the set-size. Any images in an Image Operation are does in series. This means each image is processed in order each image gets its own rastering at its own step-size. The stepsize is set to Passthrough which means that the images step-size is used. This is to make sure nothing requires images to resize or change after they've been processed. This is because resampling an image that has already been dithered and converted is a serious problem with regard to quality. We seek to avoid this issue.

### Dots

A dot operation burns a single point at a given location for a given duration.

## Special Operations
![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/46bd31bb-4b0e-4c59-83ea-353d7de4bcda)
