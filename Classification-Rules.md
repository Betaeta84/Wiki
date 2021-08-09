Each object within a project is classified in a two phase process. If an object does not classify into the current operations these will be dealt with in the fallback phase.

### Classify Phase
1. Shapes that have strokes are added any Cut/Engrave operations that exactly match the color. If no Cut/Engrave exists of exact matching colour, one is created during the Fallback Phase.
2. Shapes with any fill color (including white) and with opacity > 0 are added to all Raster operations with exactly matching color if at least one exists, otherwise are added to all raster operations that exist. If no Raster operations exists, then a black Raster operation is created in the Fallback Phase.
3. Shapes with pure black strokes and no fill are treated as if they have a Black fill as above.
4. Text objects (rather than Paths in the shape of letters) that have either stroke or fill or both are treated as above (using the stroke colour if there is no fill).
5. Dots (Paths consisting of M followed by Z or a zero length segment) are classified to the first Dot operation.
6. Any images are added to the first Image operation.

### Fallback Phase
The fallback phase is designed to handle any classifications above when an existing Operation to match does not exist - the objective being to ensure that all elements with either stroke or fill are added to at least one operation.
1. Shapes with stroke colors will be added to a new Cut/Engrave operation that exactly matches that color.
    - If the stroke color is reddish, the created operation will be a Cut.
    - If the stroke color is *anything else*, the created operation will be an Engrave. If the stroke color is white, the created Engrave operation will be disabled.
2. Shapes with fill (with opacity > 0) and shapes with a black stroke and no fill and text objects will be added to a new Black Raster operation.
3. Dot objects will be added to a newly created Black Dot operation.
4. Images will be added to a new Image Operation.

New Operations will be added immediately after the last Operation of the same type (if it exists), otherwise after the last Operation of a lower priority  Operation if it exists otherwise at the beginning of the list:

1. Dot
2. Image
3. Raster
4. Engrave
5. Cut

In other words, to avoid operations being created in the sequence that elements are processed resulting in a random sequence of operation types, the order of existing operations will be preserved but new operations will be inserted at the most appropriate point in order to try to keep the operations as much as possible grouped together in the above sequence.