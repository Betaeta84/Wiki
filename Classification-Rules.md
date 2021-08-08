The rules to classifications are:

1. Shapes with strokes except blackish are going to be classified into Cut / Engrave - with one of the exact color created if necessary. Reddish shapes with strokes will be created as Cuts, everything else as Engraves.

2. Shapes with blackish strokes will also be classified into Cut / Engrave if the color is an exact match. But a new cut/engrave will not be created.

3. Shapes with fills (including white but except 100% transparent) will be classified into all Raster operations regardless of fill colour or operation colour.


Each object within a project is classified in a two phase process. If an object does not classify into the current operations these will be dealt with in the fallback phase.

---

Classify Phase:
1. Objects that matching stroke colors are added any operation that exactly matches the color.
2. Objects with any fill color (including white) are added to all any Raster operation that exists.
3. Objects that are of type Text rather than Path are added to any Raster operations that exist, regardless of fill.
3. Any objects which are zero length are added added to any Dot operation.
4. Any images are added to Image operations

Fallback Phase, if object did not match any existing operations.
1. Objects with stroke colors will be added to a vector operation that exactly matches that color.
- If the stroke color is reddish. The created operation will be a Cut.
- If the stroke color is *anything else*. The created operation will be an Engrave.
2. Any zero length objects will be added to a created Dot operation.
3. Ignore.
