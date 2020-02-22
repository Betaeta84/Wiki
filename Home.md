# Welcome to the MeerK40t Wiki!

MeerK40t is the Hackable Laser Software for the Stock K40.

## Running MeerK40t

### Requirements
* `wxPython`: wxPython for linux and OSX should be above 4.1 for full scene functionality. If you wish to use the command line interface, using the --no_gui flag, does not require `wxPython`
* `pyusb`: pyusb is used for LibUSB in windows, linux, and OSX. If using the Windows CH341 driver, `pyusb` can be omitted. Doing so will simply fail the LibUSB driver and use the CH341 driver.
* `Pillow`: Pillow does image loading, saving and manipulations. Without Pillow installed MeerK40t will not have access to these functions.

### Optional Requirements
MeerK40t has a modular API, and can use external functionality without strictly requiring these. These optional requirements are not required in the precompiled binaries. They often also include small useful functionality that may not be broadly applicable.

* `OpenCV`: OpenCV is required for the CameraInterface. `pip install opencv-python-headless`

If you are writing a module for MeerK40t is is entirely reasonable that you can include outside requirements as you see fit, however these will be optional requirements.


### Precompiled Binaries

If you want to run MeerK40t, try some one of the precompiled binaries:

https://github.com/meerk40t/meerk40t/releases

Notes:
Precompiled Binaries are not required to support optional functions. It's entirely optional whether they support non-core features.
Precompiled Binaries do not have the translations working very effectively. Do not fret though, there are no actual translations. Yet. 


### Running Python MeerK40t.

If you want to run MeerK40t in pure python:

* Run Batch File to Python.

Download the source and run `MeerK40t.py` with python. 

* Download click: "Clone or Download".
* Click "Download Zip"
* Unzip on your desktop, this should be the meerk40t-master directory.
* Find `install_run.bat` in the `meerk40t-master` directory and run it.

The batch file checks and installs requirements and runs MeerK40t.py

You can run the same set of commands yourself:

```
pip install -r requirements.txt
python MeerK40t.py
pause
```

You need to use `python` and the python requirements of `wxPython` `pyusb` and `Pillow` to run `MeerK40t.py`.

Windows Instructions:
You will need python:
* Download and install from: https://www.python.org/

You will need Meerk40t:
* Download click: "Clone or Download".
* Click "Download Zip"
* Unzip on your desktop, this should be the meerk40t-master directory.
* Press Windows + R (to load run).
* Type "cmd" 
  * This should be a dos prompt at `C:\Users\(Name)>` location.
* Type "cd Desktop"
* Type "cd meerk40t-master
* Type "python MeerK40t.py"
  * At this point it could fail in a couple ways. It could lack: `wxPython`. `pyusb`. `Pillow`. See Troubleshooting:

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

### Expert Install
For more involved expert install instructions see the specific pages on installing for your OS.

Raspberry Pi: [Installing MeerK40t on a Raspberry Pi](https://github.com/meerk40t/meerk40t/wiki/Installing-MeerK40t-on-a-Raspberry-Pi)

Ubuntu 19.04.3: [Installing MeerK40t on Ubuntu 18.04.3](github.com/meerk40t/meerk40t/wiki/Installing-MeerK40t-on-Ubuntu-18.04.3)

OSX: [Installing Running MeerK40t on OSX](github.com/meerk40t/meerk40t/wiki/Installing-Running-MeerK40t-on-OSX)

## Detailed Documentation for the Stock Board.

If you want detailed documentation for the stock board this is the place to be. The only place to be.
[Understanding Lhymicro-GL](github.com/meerk40t/meerk40t/wiki/Lhymicro-GL)

[Lhystudios Boards Hardware specifics](github.com/meerk40t/meerk40t/wiki/Lhystudios-Boards-Hardware-specifics)

This is also a place for research on the board:
[Physical Speed Measurements](github.com/meerk40t/meerk40t/wiki/Physical-Speed-Measurements)

## Help Wanted.

You can contribute, even if you can't code. Raising issues and suggesting ideas is certainly making contributions. Also, things like translating the strings into other languages is strongly desired and helpful. Running test your machine and helping with the research on the board or into particular subjects closely related.

If you wish to help translate, you may need to regenerate the messages.po file:

* [Regenerating the message.po file](github.com/meerk40t/meerk40t/wiki/Regenerating-the-message.po-file)

If you wish to write a module either for public consumption or for private use:

* See the [Kernel API System](https://github.com/meerk40t/meerk40t/wiki/Kernel-API-System) Documentation.

If you wish to write GUIs for your modules, you may need to add icons.

* [Adding Icons to MeerK40t](https://github.com/meerk40t/meerk40t/wiki/Adding-Icons-to-a-MeerK40t-Module)

* [UI and UX Design Guide](https://github.com/meerk40t/meerk40t/wiki/UI-and-UX-Design-Guide)

## SVG Namespace

For saving files MeerK40t uses a modified form of SVG, the [Namespace specification is here.](https://github.com/meerk40t/meerk40t/wiki/Namespace)

