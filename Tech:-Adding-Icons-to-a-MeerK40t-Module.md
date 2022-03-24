Giving my methodology for making an icon for MeerK40t.

The icons are from Icon8 and typically `IOS Glyph`, `IOS` or `Windows Metro` in style.

https://icons8.com/icons

Find the desired icon and download in 50x50. We use the free license.

* Put the icon file in the Pycharm working directory.
* Using Local Terminal, with wxPython installed.
* `img2py -a icons8-icon-name-50.png icons.py`
* Paste the icon8_icon_name PyEmbeddedImage() block into icons.py


----

Other graphic assets are also fine and should be placed in an assets.py file. This would be non-icons, mouse cursors, something drawn on screen, or in the scene rather than for buttons where icons are typically and currently used. These should not have dark mode issues.