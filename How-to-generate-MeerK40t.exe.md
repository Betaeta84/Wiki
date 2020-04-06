Requires installed wxPython and pyusb.
Running the latest version of Python 3.7.7

Using pyinstaller

* pip install pyinstaller

Circa 4/2020 this should be pyinstaller 3.6

Once installed:

* pyinstaller -wFi meerk40t.ico MeerK40t.py

https://github.com/meerk40t/gui-files-meerk40t/blob/master/meerk40t.ico

---

This will generate a MeerK40t.exe in the dist directory. However, due to the inclusion of libusb as needed, we actually need to include the proper driver.

Edit MeerK40t.spec and replace the three lines giving binaries, datas, hiddenimports with:

>              binaries = [
>                 ('C:\\Windows\\System32\\libusb0.dll', '.'),
>              ],
>              datas=[],
>              hiddenimports=['usb'],

Run pyinstaller again on the spec.

* pyinstaller -wFi meerk40t.ico MeerK40t.spec

---

Test the .exe file for the libusb driver. Make sure the laser either works or it says `Usb Not Found` and does not say `No Driver`.

Test the .exe against virus definitions with VirusTotal:
https://www.virustotal.com/gui/home/upload

False positives are fine, but not for Microsoft or AVG or any other major program.

## OpenCV support

Pyinstaller is weird with OpenCV support the last known version to work and thus the recommended version is:
* `pip install opencv-python-headless==3.4.9.33`