# Optimisation-Options
![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/d84ac55d-6399-41bc-8eb9-f232dd0cbc11)

### Burn-Sequence
- Burn Inner First: Ensure that inside burns are done before an outside cut in order to ensure that burns inside a cut-out piece are done before the cut piece shifts or drops out of the material.
If you find that using this option significantly increases the optimisation time, alternatives are: 
  - Deselecting _Burn Inner First_ if you are not cutting fully through your material 
  - Putting the inner paths into a separate earlier operation(s) and not using _Merge Operations_ or _Burn Inner First_ 
  - If you are using multiple passes, check _Merge Passes_
- Tolerance: Tolerance to decide if a shape is truly inside another one.
- Group Inner Burns: Try to complete a set of inner burns and the associated outer cut before moving onto other elements. This option only does something if _Burn Inner First_ is also selected. If your design has multiple separate pieces on it, this should mostly cause each piece to be burned in entirety before moving on to burn another piece. This can reduce the risk of e.g. a shift ruining an entire piece of material by trying to ensure that one piece is finished before starting on another.
This optimization works best with _Merge Operations_ also checked though this is not a requirement. 
Because this optimisation is done once rasters have been turned into images, inner elements may span multiple design pieces, in which case they may be optimised together.

### Reducing Movements
- Reduce Travel Time:Using this option can significantly reduce the time spent moving between elements with the laser off by optimizing the sequence that elements are burned. 
When this option is NOT checked, elements are burned strictly in the order they appear in the Operation tree, and the Merge options will have no effect on the burn time. 
When this option IS checked, Meerk40t will burn each subpath and then move to the nearest remaining subpath instead, reducing the time taken moving between burn items.
- Burn complete subpaths: By default _Reduce Travel Time_ optimises using SVG individual path segments, which means that non-closed subpaths can be burned in several shorter separate burns rather than one continuous burn from start to end. 
This option only affects non-closed paths. When this option is checked, non-closed subpaths are always burned in one continuous burn from start to end rather than having burns start in the middle. Whilst this may create a longer travel move to the end of the subpath,it also avoids later travel moves to or from the intermediate point. The total travel time may therefore be shorter or longer. It may also avoid minor differences in total burn depth at the point the burns join. 
- Merge passes: Combine multiple passes into the same optimization. This will typically result in each subpath being burned multiple times in succession, burning closed paths round-and-round and non-closed paths back-and-forth, before Meerk40t moves to the next path. 
If you have a complex design with many paths and are burning with multiple passes, using this option can significantly **REDUCE** the optimisation time. 
NOTE: Where you burn very short paths multiple times in quick succession, this does not allow time for the material to cool down between burns, and this can result in greater charring or even an increased risk of the material catching fire.
- Merge Operations: Combine multiple consecutive burn operations and optimise across them globally. Operations of different types will be optimised together to reduce travel time, so vector and raster burns will be mixed. 
If _Merge Passes_ is not checked, Operations with >1 passes will only have the same passes merged. If Merge Passes is also checked, then all burns will be optimised globally.
If you have a complex design with many paths across multiple consecutive burn operations, using this option can significantly **INCREASE** the optimisation time. 

### Splitting rasters
By definition any object with a non-transparent fill will be assigned to a ratser operation. This raster operation will create a bitmap image out of these elements and will burn this image. To avoid long laserhead sweeps to move from burnable area to the next, MeerK40t can try to cluster adjacent elements together effectively creating smaller subimages that are less prone to contain these big gaps. This can significantly **REDUCE** useless laser head moves.
- Cluster raster objects: Separate non-overlapping raster objects.
Active: this will raster close (ie overlapping) objects as one, but will separately process objects lying apart from each other.
Inactive: all objects will be lasered as one single unit.
- Margin: Allowed gap between rasterable objects, to still be counted as one.