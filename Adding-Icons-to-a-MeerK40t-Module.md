Giving my methodology for making an icon for MeerK40t.

The icons are from Icon8 and typically `IOS Glyph`, `IOS` or `Windows Metro` in style.

https://icons8.com/icons

Find the desired icon and download in 50x50. We use the free license.

* Put the icon file in the Pycharm working directory.
* Click Python Console.
* type: from wx.tools.img2py import img2py
* type: img2py('icons8-icon-name.png', 'icon.py')
* Open icon.py
* Copy the icon8_icon_name PyEmbeddedImage() block.
* Open icons.py
* Paste the icon8_icon_name PyEmbeddedImage() block into icons.py