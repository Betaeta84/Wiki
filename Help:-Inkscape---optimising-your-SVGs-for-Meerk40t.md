This is a stub page. If it doesn't have the help you need, please add your own experiences to the page to help others.

## Helping Meerk40t work well
The performance of Meerk40t (at least in v 0.6.x) is highly dependent on the number of objects in the SVG file. If you have a large number of very small paths, the object tree in Meerk40t is very large and takes a long time to display, and if you have to drag objects in the tree, the length of the list of leaves makes it very tedious.

For vector operations, the way the laser moves from path to path is also dependent upon how they paths are created in Inkscape. Have you ever seen your laser cutting what should be a continuous closed path by actually cutting it in random pieces, perhaps cutting some segments in what would be a clockwise direction and others in an anticlockwise direction? Or perhaps it is cutting a lot of small repeated paths (like sewing holes in leather patterns) in random order?

Do you ever see a vector path get burned twice? Sometimes you may have duplicate paths, even if you cannot see them.

These can be a particularly the case if you have received your SVG from a 3rd party and is especially true if e.g. you have used an online service to convert a PDF pattern into an SVG. (There are several such services, and some are better than others - but they all produce SVG files which IMO need clean-up.)

You can help Meerk40t to be more efficient by cleaning up your SVG file in Inkscape before you load it into Meerk40t.

## Reducing the number of vector path objects

There are two ways you can combine individual line segment paths in Inkscape into fewer path objects:

a. You can join consecutive paths into a single path;
b. You can combine multiple separate paths into a single path object.

These are entirely different actions, done in completely different parts of Inkscape and producing completely different results. 

### Creating a single closed path

### Combining multiple paths into a single path object

## Eliminating duplicate paths

## Using colours to identify Raster / Vector paths

