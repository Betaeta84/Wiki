# Preferences
## General
<img width="212" alt="image" src="https://github.com/meerk40t/meerk40t/assets/2670784/d80e2433-014a-4322-9362-82097fb390cb">

- Units: Define the standard units for MeerK40t to use (imperial/metric)
- Language: Sets the user interface language 
- SVG Pixel: Select the Pixels Per Inch to use when loading an SVG file
- SVG Viewport is bed: SVG files can be saved without real physical units. This setting uses the SVG viewport dimensions to scale the rest of the elements in the file.
- Image DPI Scaling:
  - Unset: Use the image as if it were 1000 pixels per inch.
  - Set: Use the DPI setting saved in the image to scale the image to the correct size.
- Create a file-node for imported image:
  - Unset: Attach the image directly to elements.
  - Set: Put the image under a file-node created for it.


MeerK40ts standard to load / save data is the svg-Format (supported by many tools like [Inkscape](https://inkscape.org/)). While it is supporting most of SVG functionalities, there are still some unsupported features (most notably advanced text effect, clips and gradients).
To overcome that limitation MeerK40t can automatically convert these features with the help of inkscape:
if you set the 'Unsupported feature' option you can either ask at load time how to proceed or let inkscape perform the conversion automatically. Please note that this conversion might not really needed most of the times, so the recommendation is to use the 'Ask at load time' option.

![image](https://github.com/meerk40t/meerk40t/assets/2670784/741b4d31-2169-4dc7-93cc-818ed55e3eba)




