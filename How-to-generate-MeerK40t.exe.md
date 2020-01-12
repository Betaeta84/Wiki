This is done with the proper requirements installed wxPython and pyusb. And running the latest version of Python 3.

Using pyinstaller

* pip install pyinstaller

Once installed:

* pyinstaller --noconsole --onefile MeerK40t.py

This will generate a MeerK40t.exe in the dist directory. However, due to the inclusion of libusb as needed, we actually need to include the proper driver.

Edit MeerK40t.spec and replace the three lines giving binaries, datas, hiddenimports with:

>              binaries = [
>                 ('C:\\Windows\\System32\\libusb0.dll', '.'),
>              ],
>              datas=[],
>              hiddenimports=['usb'],

Run pyinstaller again on the spec.

* pyinstaller --noconsole --onefile MeerK40t.spec

And test the .exe file for having the driver. Make sure the laser either works or it says `Usb Not Found` and does not say `No Driver`.