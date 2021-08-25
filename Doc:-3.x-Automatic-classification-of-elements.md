## Overview
When you open a file in Meerk40t, the SVG elements in that file are automatically classified into one or more operations.

The objectives of this automated classification are:
1. To classify elements consistently every time given the same SVG file and the same pre-existing operations.
1. To allow the user to create an operations set and save the file so that elements can be reclassified into the same operations when the file is reloaded.
1. For the classification rules to be reasonably understandable to the user, particularly for simple SVG files using the de-facto standard of Red = Cut, Blue = vector Engrave, Black or Filled = Raster.
1. To allow more advanced users to separate Cuts and Engraves into as many separate operations as they want, based on stroke colour, whilst also providing an option to limit this (using Default operations) to avoid excessive numbers of separate operations if the file has many colors.
1. To allow overlays of different infill darknesses to combine additively as you would expect - which means putting them into the same Raster operation(s) - whilst still allowing advanced users to create more flexible operation schemes.

As you might imagine, there are a wide variety of different combinations of elements that need to be catered for, and this will never be perfect for all of these. However the developers have strived to achieve a classification algorithm that is reasonably intuitive and which gives results that are expected in as many cases as possible.

## Simplified explanation
1. Fill = Raster
1. Black strokes = Raster
1. Same color stroke & fill = Raster only
1. Red strokes = Cut (and raster if fill as well)
1. Every other stroke = Engrave (and raster if fill as well)

**To-Do:** A simple example should be included here.

## Full algorithm
This section is provided to give a definitive explanation of how the classification algorithm works. Hopefully you will only need to study this if you are an advanced user and want to understand the nuances of why your file is classified the way it is.

### Pre-classification rules
1. Images are classified into a single (first listed) Image operation. If an Image operation does not exist, one will be created if necessary.
1. Paths and Straight lines which have zero or one segment and zero length are classified into a special "Dots" operation which has a different way of burning. If a Dots operation does not exist, one will be created if necessary.
1. White (#ffffff) fill is effectively an anti-raster - used to blank out areas that would otherwise be burned - these are treated as a special case added to every non-empty Raster operation at the end of classification.
1. For the purposes of classification, the color of any element is the color of the stroke if it has a stroke otherwise the color of fill.
1. Elements are added to at least one Raster operation if they have fill or if they have a Black (#000000) stroke.
1. Elements are added to at least one vector (Cut or Engrave) operation if they have a stroke except if:
    1. The stroke color is Black (#000000).
    1. The stroke color is exactly the same as the fill color.
    **Note:** An element with black stroke or stroke same color as fill will still be classified to an existing Cut or Engrave operation of the exact same color regardless of these exceptions.

### Automated Classification
Automated classification is a 4 phase process:

1. Elements are added to any operation (regardless of stroke or fill) that has the exact same color as the element (as defined above).
    **Example:** An element with a stroke color of #abcdef will be matched to **any** Cut, Engrave or Raster operation with the same color of #abcdef.
1. For elements which need to be added to a vector (Cut or Engrave) operation (as defined above) but without an existing operation of the exact same color, then:
    1. Elements which are red-ish in colour (i.e. Red (#ff0000) itself or sufficiently close in colour to Red) will be allocated to a Cut operation.
    1. Elements of any other color (except Black which is a special case above) will be allocated to an Engrave operation.
    1. If one or more Default operations of the required type exists, then the elements is added to these operations.
    1. Otherwise a new operation of the required type is created with the exact color of the element. All other elements with this same color which are not matched in step one will also be added to this new operation. 
    If there are no Default Cut or Engrave operations pre-defined, then you will get one or more operations for every element with a stroke that is not Black.
    **Summary:** With the two exceptions defined above, every element with a stroke will be allocated to at least one Cut or Engrave Operation, matched on colour, an existing Default operation, or into a new operation with matching colour.
1. If elements have not been added to a Raster operation when they need to be (as defined above), then:
    1. If one or more Default Raster operations exists, then the element is added to these operations.
    1. Otherwise if one or more Raster operations exist which have no elements added after step 1, then elements are added to these operations.
    1. Otherwise a new Default Raster operation is created and all elements which still need to be rastered will be added to it.
    **Summary:** With the exception of White fill, every element with a fill or Black stroke will be allocated to at least one Raster Operation, matched on colour, into an existing Default operation, into empty Raster operations or into a new Default operation.
1. Finally, any White-filled (anti-fill) elements will be added to any non-empty Raster operations in order to mask out these areas for every Raster operation.