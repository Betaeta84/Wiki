This is a work in progress.
Instructions from December 2019.

* wget http://github.com/meerk40t/meerk40t/archive/master.zip
* unzip master.zip
* cd meerk40t-master
* python3 MeerK40t.py

Errors that might occur:

> ModuleNotFoundError: No module named 'wx'

We install wxPython with pip.

```
sudo pip install -U \
      -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-18.04/wxPython-4.0.7-cp36-cp36m-linux_x86_64.whl \
      wxPython
```
This might fail saying pip isn't a thing.

You need pip.

* wget https://bootstrap.pypa.io/get-pip.py
* sudo python3 get-pip.py

> ModuleNotFoundError: No module named 'distutils.util'

sudo apt-get install python3-setuptools

> Still not working. Trying random thing. I got this working before.

* sudo apt-get install python3-*


