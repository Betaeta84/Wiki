This is a work in progress.
Instructions from December 2019.
Trying a bunch of stuff on a clean install to see what works, even if it's lost in the woods nonsense.

* wget http://github.com/meerk40t/meerk40t/archive/master.zip
* unzip master.zip
* cd meerk40t-master
* python3 MeerK40t.py

Should load up out of the box.

It didn't.
---


ModuleNotFoundError: No module named 'wx'
---

We install wxPython with pip.

* wget https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-18.04/wxPython-4.0.7-cp36-cp36m-linux_x86_64.whl
* sudo pip install wxPython-4.0.7-cp36-cp36m-linux_x86_64.whl

sudo: pip: command not found
---

You need pip.

* wget https://bootstrap.pypa.io/get-pip.py
* sudo python3 get-pip.py

ModuleNotFoundError: No module named 'distutils.util'
---

You need setuptools

* sudo apt-get install python3-setuptools

Other commands I tried that might not be for the best:
---
 
Some other random things I tried.

> Still not working. Trying random thing. I got this working before.

* sudo apt-get install python3-*

