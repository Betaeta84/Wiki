MeerK40t uses wxPython for the GUI interface and requires transformation matrices to do zooming and panning of elements of the scene and within the viewbox and those features were added to wxPython in 4.1. So the wxPython being installed needs to be version 4.1+ earlier versions will default to code that does not render the scene.

These issues were discussed in:
https://github.com/meerk40t/meerk40t/issues/95

To successfully run MeerK40t on the Mac, requires a snapshot build of wxPython 4.1
https://wxpython.org/Phoenix/snapshot-builds/

Install from the .whl file and download the latest released version of the master branch. Or a particular released version:
https://github.com/meerk40t/meerk40t/releases

Python >= 3.6 required. Check to see ifyou have a version already installed with terminal command "python3 -V". Otherwise: download and install https://www.python.org/downloads/mac-osx/

After install, the new python command will contain the version you installed. So if you install Python version 3.9, the command is "python3.9 -m pip list" and the like.

libjpeg is required for Pillow. 
Website: http://ethan.tira-thompson.com/Mac_OS_X_Ports.html 

Direct .pkg Download: http://ethan.tira-thompson.com/Mac_OS_X_Ports_files/libjpeg%20%28universal%29.dmg
OR
curl --remote-name http://www.ijg.org/files/jpegsrc.v9c.tar.gz

libusb is required for pyusb. https://github.com/libusb/libusb

"python3.X -m pip install --upgrade ezdxf pyusb numpy six wheel altgraph pillow opencv-python-headless wxpython"
OR
"python3.X -m pip install --upgrade meerk40t[all]"

Advanced:
openssl from github is required if you want to compile python yourself: https://github.com/openssl/openssl Python compile may require ./configure --enable_framework for some use cases on MacOS.

On Big Sur; numpy and wxPython would need to be installed from source as well. 