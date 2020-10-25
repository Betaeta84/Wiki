
## Easy Windows

Everything is prebundled. You can just run the file.

Download MeerK40t.exe
https://github.com/meerk40t/meerk40t/releases

Run: MeerK40t.exe

MeerK40t is compiled as a portable application. It doesn't need to install or do anything.


## Run Batch File to Python.

You will need python:
* Download and install from: https://www.python.org/

Download the source and run `MeerK40t.py` with python. 

* Download click: "Clone or Download".
* Click "Download Zip"
* Unzip on your desktop, this should be the meerk40t-master directory.
* Find `install_run.bat` in the `meerk40t-master` directory and run it.


The batch file checks and installs requirements and runs MeerK40t.py

```
pip install -r requirements.txt
python MeerK40t.py
pause
```

The batch file avoids the command prompt.

* Right click and "Run as Administrator" if you have permissions errors.

## Command Prompt Fallback

You will need python:
* Download and install from: https://www.python.org/

You need to use `python` and the requirements of `wxPython` `pyusb` and `Pillow` to run `MeerK40t.py`.

Windows Instructions:

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

## Troubleshooting Install

### Windows:
* `ModuleNotFoundError: No module named 'wx'` means you need wxPython.
  * Type: `pip install wxPython`
* `ModuleNotFoundError: No module named 'usb'` means you need pyusb.
  * Type: `pip install pyusb`
* `ModuleNotFoundError: No module named 'PIL'` means you need Pillow.
  * Type: `pip install Pillow`

### Linux:
* `PermissionError: [Errno 13] Permission denied:...`
  * Type: `sudo <previous command>`
* `ImportError: libpng12.so.0: cannot open shared object file: No such file or directory .` libpng is not installed.
  * Type: `sudo apt-get install libpng`

