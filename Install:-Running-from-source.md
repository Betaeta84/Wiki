If you want to use MeerK40t to drive your K40 Laser on one of the standard supported platforms (Windows, MacOS, OSX, Linux, Raspberry Pi) we recommend that you use the executable versions.

If you want to run MeerK40t on a different platform or you want to develop a plugin or you want to assist with the development of MeerK40t itself, then you will need to run MeerK40t from source, and will need to:

1. Install prerequisite software
2. Download the MeerK40t source
3. Set up a shortcut to run it.

If you use these instructions to run MeerK40t from source and find that they are incomplete or wrong, please help the next person who will try to use them by updating them (using the Edit Button top right) using your own experiences, or raise an [issue](https://github.com/meerk40t/meerk40t/issues/new/choose) describing your difficulties so someone else can fix it

### Essential Prerequisites
* `Python`: MeerK40t will (apparently) run using Python2 however the latest version of Python3 is recommended. 
* Python's `pip`: pip is the Python package manager and will be used to install other packages you need.
* Python's `virtualenv`: We recommend that you create a Python virtual environment to run MeerK40t, in which case you will need this. 
* Python's `setuptools`: A pre-requisite for installing pyusb
* `wxPython`: MeerK40t uses wxPython to provide the GUI environment. If you **only** wish to use the command line interface using the --no_gui flag, then you do not require `wxPython`, however we expect only a **very** small number of expert users will want to do this. For full functionality, wxPython version >= 4.1 should be installed. 
* `pyusb`: MeerK40t can use either of two sets of USB drivers. If you are running MeerK40t on Windows and have already installed the CH341 drivers (which come with the software provided with your K40), then you don't need pyusb. Otherwise pyusb is used on all platforms to provide the USB connectivity to your K40. If you want to run MeerK40t on a platform which does not have either CH341 or pyusb, then you are out of luck. If using the Windows CH341 driver, `pyusb` is optional - the LibUSB driver will be used if pyusb is installed, otherwise CH341 will be used.
* `Pillow`: Pillow is a package that provide image loading, saving and manipulation. If you do not install Pillow, MeerK40t will not have access to **any** image functions and will only be able to handle SVG paths.

Even if you do not intend to use the functionality of the above packages, we still recommend that you install them. The majority of users will run MeerK40t with these packages installed, and if you omit them then (in our opinion) you may be more likely to hit a bug.

### Optional Prerequisites
* `OpenCV`: OpenCV is required for the CameraInterface. Install it after the essential prerequisites using `pip install opencv-python-headless`

MeerK40t has a modular API, and modules can be added that require additional external dependencies to provide functionality that may be useful in specific circumstances but not broadly applicable. These modules and their optional requirements will not be included in the executable images, but can be added as needed.

If you are writing a module for MeerK40t you should feel free to include external requirements that you need to provide the functionality you are aiming for, however these will be Optional Prerequisites.

### Installing Essential Prerequisites

1. Download the version for your O/S from the [Python web site](https://www.python.org/downloads/) and run the executable to install.

2. PIP should come pre-installed with Python 2.7.9+ and Python 3.4+. Check whether it is installed by running `pip help`. If it is not installed, then install PIP as follows:

   * Windows - Download `get-pip.py` from [pypa.io](https://bootstrap.pypa.io/get-pip.py) and run it from an **administrator** command prompt with `python getpip.py`.
   * Linux - Debian, Ubuntu and related distributions which use `apt` - `sudo apt install python3-pip`

   Make sure that you have the latest version of pip by running `pip3 install --upgrade pip` from an administrator command prompt.

3. Set up a virtual environment for MeerK40t.

4. Install Python's setup tools with:

   * Windows - 
   * Linux - Debian, Ubuntu and related distributions which use `apt` - `sudo apt install python3-setuptools`

4. Install wxpython with `pip3 install wxPython` from an administrator/sudo command prompt.

5. Install pysub with `pip3 install pyusb` from an administrator/sudo command prompt.

6. Install pillow with `pip3 install pillow` from an administrator/sudo command prompt.

### Running Python MeerK40t

If you simply want to use MeerK40t running from source, we recommend that you download a release version (not a beta) of source from the [github release page](https://github.com/meerk40t/meerk40t/releases) and unpack it into the virtual environment you have just created.

If you want to help develop MeerK40t itself, then create a local git clone from the GitHub repository so that you can keep the source code in-step with Github and can submit any changes yo develop back to Github. If you are going to develop changes to MeerK40t itself, we assume that you will be technical enough to know how to do this without detailed instructions from us.

To run MeerK40t use `python3 MeerK40t.py`

### Troubleshooting Install

#### Windows:
* `ModuleNotFoundError: No module named 'wx'` means you need wxPython.
  * Type: `pip install wxPython`
* `ModuleNotFoundError: No module named 'usb'` means you need pyusb.
  * Type: `pip install pyusb`
* `ModuleNotFoundError: No module named 'PIL'` means you need Pillow.
  * Type: `pip install Pillow`

Windows may not tell you pyusb is a requirement. But, if you are running it along side Whisperer it would be a requirement. Having pyusb and running the stock driver is not an issue.

#### Linux:
* `ImportError: libpng12.so.0: cannot open shared object file: No such file or directory .` libpng is not installed.
  * Type: `sudo apt-get install libpng`