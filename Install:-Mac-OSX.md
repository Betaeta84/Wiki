# Download
Regular Mac binary builds are published with releases as .dmg files containing a standard Mac native application. Find the latest version in the [Releases](https://github.com/meerk40t/meerk40t/releases) section. More adventurous users are welcome to clone the repo.      

# Build
MeerK40t requires macOS Command Line Tools to be installed. Those can be installed with terminal command "xcode-select --install" then "sudo xcodebuild -license"; or with a package downloaded from https://developer.apple.com/ 

Python >= 3.6 is required. To check if a version of Python 3 already installed, use the terminal command "python3 -V". Otherwise: download from https://www.python.org/downloads/mac-osx/ and install. 

After install, the new python command will contain the version you installed. So if you installed Python version 3.9.2, the command is "python3.9 -m pip list" and the like.
### Non-pip Requirements
libjpeg is required to install Pillow.

libusb is required to use pyusb.

## MacPorts
sudo port install libjpeg libusb
## Homebrew
brew install libjpeg libusb
## Or; Manually Compile or Install Requirements
### LibJpeg
http://ethan.tira-thompson.com/Mac_OS_X_Ports.html LibJpeg has been made available as a macOS install package. 
[libjpeg Package Download](http://ethan.tira-thompson.com/Mac_OS_X_Ports_files/libjpeg%20%28universal%29.dmg)

OR

Download source with terminal command: "curl --remote-name http://www.ijg.org/files/jpegsrc.v9c.tar.gz"

### LibUSB
https://github.com/libusb/libusb

To build; "cd" into cloned repo and issue terminal command: "./autogen.sh && ./bootstrap.sh && make && sudo make install"

# Install MeerK40t and Requirements available from pip
Once a version of Python 3, Command Line Tools, libjpeg, and libusb are installed; pip can be used to install the rest of the requirements, as well as MeerK40t itself. From the list below; ezdxf is an optional install which enables DXF file support. OpenCV (opencv-python-headless) is also optional, and only necessary for camera support. Without wxPython, a CLI only version of MeerK40t can still function (no GUI).

terminal command: "python3 -m pip install --upgrade meerk40t[all]"

OR (more explicitly)

terminal command: "python3 -m pip install --upgrade numpy scipy six wheel altgraph pyparsing ezdxf pyusb opencv-python-headless pillow wxpython meerk40t[all]"

To launch MeerK40t ###terminal command: "meerk40t"


### Advanced:
openssl from github is required if you want to compile python yourself: https://github.com/openssl/openssl Python compile may require ./configure --enable_framework for some use cases on MacOS.

On macOS 11+ Big Sur; numpy and wxPython would need to be installed from source as well. 

### Legacy Support:

MeerK40t has been compiled as far back as Mac OS X 10.8 but it is not regularly requested or tested.

MeerK40t uses wxPython for the GUI interface and requires transformation matrices to do zooming and panning of elements of the scene and within the viewbox and those features were added to wxPython in 4.1. So the wxPython being installed needs to be version 4.1+ earlier versions will default to code that does not render the scene.

These issues were discussed in:
https://github.com/meerk40t/meerk40t/issues/95