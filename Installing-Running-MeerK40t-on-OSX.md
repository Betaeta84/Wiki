MeerK40t uses wxPython for the GUI interface and requires transformation matrices to do zooming and panning of elements of the scene and within the viewbox and those features were added to wxPython in 4.1. So the wxPython being installed needs to be version 4.1+ earlier versions will default to code that does not render the scene.

These issues were discussed in:
https://github.com/meerk40t/meerk40t/issues/95

To successfully run MeerK40t on the Mac, requires a snapshot build of wxPython 4.1
https://wxpython.org/Phoenix/snapshot-builds/

Install from the .whl file and download the latest released version of the master branch. Or a particular released version:
https://github.com/meerk40t/meerk40t/releases
