
## Dependencies for MeerK40t must be installed.

Pyinstaller is weird with OpenCV support the last known version to work and thus the recommended version is:
* `pip install opencv-python-headless==3.4.9.33`

Requires `wxPython`, `pyusb`, `ezdxf`, and `Pillow`. While it often will run without these, it will lack particular capabilities if they are absent.

## Installing pyinstaller.

Running the latest version of Python 3.7.7

Using pyinstaller

* pip install pyinstaller

Circa 4/2020 this should be pyinstaller 3.6

Download the icon:
* https://github.com/meerk40t/gui-files-meerk40t/blob/master/meerk40t.ico

Once installed:
* pyinstaller -wFi meerk40t.ico MeerK40t.py

---

# .spec Alteration.
This will generate a MeerK40t.exe in the dist directory (there's some way to not compile everything but I forget it). However, due to the inclusion of libusb as needed, we actually need to include the proper driver, and translation files etc.

Edit MeerK40t.spec and replace the three lines giving binaries, datas, hiddenimports with:

>              binaries = [
>                 ('C:\\Windows\\System32\\libusb0.dll', '.'),
>              ],
>              datas=[],
>              hiddenimports=['usb'],

# Language Support

Language support. Language support needs to added starting with the first languages that provide solid translations.

```a.datas += [('locale\\it\\LC_MESSAGES\\meerk40t.mo', 'C:\\Users\\Tat\\PycharmProjects\\meerk40t\\locale\\it\\LC_MESSAGES\\meerk40t.mo', 'DATA')]```

# Pyinstaller on the spec.

Run pyinstaller again on the spec.

* pyinstaller -wFi meerk40t.ico MeerK40t.spec


# LibUSB test.

Test the .exe file for the libusb driver. Make sure the laser either works or it says `Usb Not Found` and does not say `No Driver`.

If it says No Driver, it lacks the libusb.dll file.

# False Positive Virus Prevention

Test the .exe against virus definitions with VirusTotal:

https://www.virustotal.com/gui/home/upload

False positives are fine, but not for Microsoft or AVG or any other major program.

If this false positive happens, recompiling the bootloader and rebuilding the pyinstaller exec several times can help correct this.

https://stackoverflow.com/questions/53584395/how-to-recompile-the-bootloader-of-pyinstaller
