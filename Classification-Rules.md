Each object within a project is classified in a two phase process. If an object does not classify into the current operations these will be dealt with in the fallback phase. Classification color is based on stroke-color information *only*. The color of the fill does not matter, only whether a fill exists or does not exist. This is because stroke color has no effect on vector operations (Cut/Engrave). Fill has a direct impact on the rastering of that object.

### Prohibitions
1. Vector objects with a length greater than zero are never classified into Dots, even if the stroke-color matches.
2. Text objects are never added to any non-raster operation, even if the stroke-color matches.

### Classify Phase
1. Shapes that have stroke colors are added to all operations that exactly match that stroke color, except where prohibited.
2. Shapes with any fill color are added to all Raster operations.
3. Text objects (as opposed to Path in the shape of letters) are added to all raster operations.
4. Dots (Paths consisting of Move, with or without a Close) are classified into stroke-color matching Dots operations.
5. Any images are added into all ImageOp operation.
6. Only objects that did not get classified by the above are sent to the Fallback Phase.

### Fallback Phase
The fallback phase is designed to handle any classifications above, when an existing Operation to match does not exist - the objective being to ensure that all elements with either stroke or fill are added to at least one operation. Added operations are added immediately. Subsequent objects in the Classify Phase may get classified into the newly created operations.

1. Shapes with stroke colors will be added to a new operation that exactly matches that color.
    - If the path is of zero length and contains a position the created operation will be a Dots operation.
    - If the stroke color is reddish, the created operation will be a Cut.
    - If the stroke color is black, the created operation will be a Raster.
    - If the stroke color is white, the created Engrave operation will be disabled.
    - If the stroke color is *anything else*, the created operation will be an Engrave.
2. Shapes with fill colors and text objects will be added to a new black-stroke Raster operation.
3. Images will be added to a new Image Operation.

### Priority 
New Operations will be added immediately after the last equal or higher priority operation:

The order of existing operations will be preserved. New operations will be inserted at the an appropriate position to keep the grouped operations together as much possible.

1. Dot
2. Image
3. Raster
4. Engrave
5. Cut

Cutcode, Lasercode, CommandOperations and other non-typical Operations are unranked and ignored. If no ranked operations exist, the newly created operation is appended to the end of the operation list.
