* [New questions](#new-questions)
* [New proposal](#new-proposal)
* [Existing proposal](#existing-proposal)

# New questions
Before we can finalise this, a couple of new (sets of) questions need answering.

### Transparency 

* The colour picker in Operation Properties does not allow you to create an Operation colour with anything other than 0% transparency.
* Colours in elements can have non-zero transparency.

Questions:
1. Should we (programmatically) create Operations with non-zero transparency? I think we should always set transparency to 0.
2. How should we handle transparency in colour comparisons between elements and operations? I think we should only compare RGB and not transparency.
3. What does transparency in fill do during Raster? I would hope that it adjusts the PPI in addition to the fill colour.
4. What does transparency in stroke do during Cut / Engrave? My vote is to use this for PPI (enabled by default but with an Operation Properties Advanced option to disable it).

### Alternative non-fill / fill rastering special cases
1. Non-fill - I assume we raster a black Shape with no fill as if it has black fill. Does this make sense, or should we use this as a way of rastering a path rather than engraving it or creating two paths with a fill between?
2. Fill - If we have a stroke and fill of the same colour, the proposed algorithm below classifies the element to both Cut/Engrave and Raster operations. I propose that if the stroke and colour fills are the same then we only classify it to a Raster operation, and if the user wants it classified to both Cut/Engrave and Raster, then they need to change the stroke colour.

# New proposal
Each element within a project (with the exception of Shapes with no stroke and no fill) is classified into at least one Operation. We iterate through each element in the project and we ensure that all elements are classified into at least one operation, creating new operations as needed as we iterate through these elements.

### Rules
1. If the element is a Dot (a path with either M followed by Z or M followed by a zero length segment of any type) we treat it as a special case and not as a normal Shape, regardless of stroke and fill. Dots will be added to the first Dot operation, and one will be created if necessary.
2. If the element is a Shape (but not a Dot) with no stroke and no fill, it is **not** classified.
3. If the element is a Shape with no stroke but fill, we use the fill colour as the stroke colour.
4. Shapes and Text elements are allocated first to Cut/Engrave/Raster Operations of the exact same colour that exist at the start of the classify. If this happens, then the following rules do not apply. The intent here is to respect change from one operation type to another for a specific colour previously made by the user.
5. Shapes which have Black strokes and not classified under 4. will be treated as Raster objects and allocated to all existing Raster operations or to a new single Raster operation.
6. Shapes which have Reddish strokes and not classified under 4. will be created as new Cut operations.
7. Shapes which are neither Reddish nor Black and not classified under 4. will be created as Engrave operations.
8. Shapes with fill which are not Black and not classified under 4. will be additionally be allocated to all Raster operations or to a new single Raster operation.
9. Text elements not classified under 4. will be allocated to all existing Raster operations or to a new single Raster operation.
9. A new Dot, Raster or Image operation will only be created if no operation of this type already exists.

## Algorithm
1. We iterate through the operations and look for the following matches:
    1. Object is an SVGImage and the operation is *the first* Image operation - we add the object to the operation. We do not consider any further operations.
    2. Object is a Dot and the operation is a Dot - we add the object to the operation. We do not consider any further operations.
    3. Object is a Shape and the operation is an *existing* Cut/Engrave which exactly matches the element colour - we add the object to the operation.
    4. Object is a Shape and the operation is an *existing* Raster operation which exactly matches the element colour - we add the object to the operation. This is done regardless of whether the Shape has a fill or not.
    5. Object is a Text object and the operation is an *existing* Raster operation which exactly matches the element colour - we add the object to the operation.

    Note 1: We test for whether the operation was existing at the start of the classification by maintaining a running list of new Cut/Engrave and Raster operations we have created during the classification, and an operation is only an *existing* one if it is not in these lists.

    Note 2: As we iterate we create a list of Raster operations for use in step 3. below.

    If the element was added to any operation in the above, we move to the next element.

2. If element is Dot or Image, then create a Dot or Image operation, add the element to it and move to the next element.
   1. If the object is a Dot, we create a Dot operation as the first operation in the list.
   2. If the object is an Image, we add an Image operation after the last Dot operation in the list or at the beginning of the list if there is not a Dot operation.

3. Element is a Shape with stroke except Black, iterate through the new Cut/Engrave operations. If the operation is a Cut/Engrave which exactly matches the element colour, add the element to the operation (there should only be one of these). If there are no matching operations, add a new operation as follows:
   1. If the object is a Shape with Reddish stroke, we add a Cut operation of this colour.
   2. If the object is a Shape with stroke except Redish or Black, we add an Engrave operation of this colour. If the colour is white we set this operation to Disabled.
   Note: We maintain a list of these new Cut/Engrave operations for use in step 1. and this here.

4. Element is a Shape with fill, or if the object is a Shape with a Black stroke, or if the object is a Text object, then:
   1. If there are no Raster operations, add a Raster operation of colour Black and add the element to it.
   2. Otherwise, iterate through the existing Raster operations and add the object to each Raster operation.
   Note: We maintain a list of this new Raster operation for use in this step 1.

### Priority
The ideal priority of operations is as follows:

1. Dot
2. Image
3. Raster
4. Engrave
5. Cut

However:
* the order of existing operations will be preserved (in case the user has manually sequenced the operations)
* new operations of the same type as an existing operation will be inserted immediately after the last existing operation of that type in order to group similar operations together
* new Dot, Image and Raster operations (which are only created by classify if an existing operation of this type doesn't already exist) will be inserted at the beginning of the list but respecting the above sequence

Note: It is proposed that a first PR addresses the classification into operations and that sequencing these operations as described here will be added as a second PR once the first one has been tested, accepted and merged.

Cutcode, Lasercode, CommandOperations and other non-typical Operations are unranked and ignored. If no ranked operations exist, the newly created operation is appended to the end of the operation list.

# Previous proposal
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
