# Installing from source

Download Python if you do not already have it installed: https://python.org
Be sure to check the box to Add to Path when it comes up during install.

In the command line console: `pip install meerk40t[all]` then `meerk40t` should launch meerk40t.

### Ubuntu 22.04
On version 22.04 of Ubuntu and distributions that rely on their repositories, the version of wxPython is too old.  Additionally, pip is missing a proper wheel to compile and install it.  You need to either compile wxPython yourself or use a premade wheel to install through pip.

The following is an example that will install meerk40t from source:
```
sudo apt install git python3-serial python3-usb python3-ezdxf python3-opencv python3-pip libgtk-3-dev python3-numba python3-minieigen cmake cython3

pip3 install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-22.04/wxPython-4.2.0-cp310-cp310-linux_x86_64.whl wxPython

pip3 install meerk40t[all]

# start Meerk40t with the following:
~/.local/bin/meerk40t

```
### Archman based distributions (e.g. Manjero) [Draft]
You need to use ``pamac`` to install the gtk libraries that are required to build wxpython:
```
sudo pamac install gtk3 base-devel
```
Additionally you need to create a dedicated virtual environment for python:
```
python -m venv ~/mk
~/mk/bin/pip3 install meerk40t[all]
~/mk/bin/python3 meerk40t.py
```

# Notes
* Installing numpy on windows 7 requires installing numpy==1.23.5. `pip install numpy==1.23.5` if you're using 32 bit python.

### Compatibility
We currently support Mac, Linux, and Windows back to Windows 7. To use Meerk40t in windows 7, you may need to install python 3.8 and, if using the 32-bit version, use `numpy==1.23.5`.
