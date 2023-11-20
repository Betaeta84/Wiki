# Classification
The assignment of an element to a laser operation is called classification. It determines how this element will be burnt: as a vector, as an image, with a lot of power, fast / slow etc. All these properties are set by the [operations](https://github.com/meerk40t/meerk40t/wiki/Online-Help:-OPERATIONS).

There's no limit to the amount of operations a single element can be assigned to:
- a single operation, like engrave or cut
- multiple operations, a rastered image of the element as it has a solid fill and a vector cut around it's edges
- consequence: **if an element is not assigned to any operation at all, then this element will not be burned!**

You can assign an element manually to an operation if you drag that element and drop it on top of the operation:

![dragdrop](https://github.com/meerk40t/meerk40t/assets/2670784/9661da26-0e02-4e6f-a7ef-c99928134415)

Alternatively you can let MeerK40t do the assignment for you: this is done by looking at the color of the stroke and the fill of the element.

## Automatic classification

### Details

Don't read on, if you get easily confused or bored, MeerK40t does normally a great job classifying elements.

![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/8e0c2bdd-65a3-46b0-ba09-94300bcf39c9)

The classification logic is as follows:

1. A given element will be handed over all operations in the tree (from top to bottom) who will decide if they will be able to classify it:
  1. is it the right type (eg an image op will only accept images while a cut op will refuse such a thing)
  2. if the checkmarks for stroke / color are set then it will compare it's own color to the stroke / fill of the node. If they match it will classify it   
  3. if there aren't any marks set, then it will accept such a node regardless of it's color
2. If there was no matching operation then it will look for all operations defined in the ``_default`` operations list (the one at the bottom of the screen) and will use one of them
3. If again such an op couldn't be found it will create a cut op if the elements stroke is red (with the color red and the checkmark for stroke set, so it will accept only similar colored elements), an engrave for all other colors (with the color of the element and the checkmark for stroke set, so it will only accept similar colored elements) and will look as well for a fill. If a fill is present then it will create a default raster op with the color "black" and no checkmarks set, so this raster will accept all elements with a fill.

So for a rectangle with stroke blue and fill cyan you would have two operations created:
- an engrave with a blue color
- a raster with a black color
If you don't want to have this then your shapes need to have no stroke color = transparent 

Additionally:
- Any successful classification for one op will not prevent more operations to classify the very same element too, this is very much intended (eg a pattern fill plus an outer engrave) - unless the "Stop" checkmark is set for the operation. This will prevent further classification.
- There is another checkmark: Default. If the first pass to find matching operations failed (no matching colors found), then in a second pass the operations with the default flag active will be used for classification. This step will be done before a check with the default material list would be done, so between 2. and 3. above.
