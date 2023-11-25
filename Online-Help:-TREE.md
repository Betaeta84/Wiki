# Work-Tree


## Operations 

### The Operations-parent node
The top node of operations has a special context menu, that does allow to access/set generic properties of operations

![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/275504e8-24a6-4d96-a8a7-74730f0a4c72)
- Global Settings: this opens a window that contains parameters how often the totality of all operations should be performed. This can even set to be continuous, so the operations will start over and over again. This is useful e.g. if you don't know for sure how many passes you may need to cut through a material.

![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/75cf67db-d329-4fcc-9a5a-e9635d9aa4e1)
- Clear all: clears all operations
- Clear unused: clears only those operations, that do not have assigned elements, ie are not used
- Scale speed settings: scale the speeds of all (all selected) operations up or down
- Scale powersettings: scale the power of all (all selected) operations up or down
- Enable all operations: well, this enables all operations, so they __will be used__ for burning
- Disable all operations: this disables all operations, so they __will not be used__ for burning
- Toggle all operations: toggles the enabled state, so all previously enabled operations will become disabled, all previously disabled operations will become enabled
- Classification - Refresh classification for all: one option to do that and create new operations if no matching ops could be found, the other will just use the existing set
- Load operations: Loads a previously saved set of operations
- Save operations: Saves the set of defined operations for later retrieval and reuse
- Append operation: Will add a basic operation to the operation set (Image, Raster, Engrave, Cut...)
- Append special operation: appends a control operation to the operation set, so one that does not take elements and determines how to burn them, but ops that do control the job behaviour.

## Elements

## Regmarks