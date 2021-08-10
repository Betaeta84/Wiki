Linux: Debian 8 (Jessie)
---
Python3.4.2 installed by default:

`apt-get install git, build-essential, libssl-dev, libffi-dev python-gtk3.0 libgtk-3-dev`

`mkdir Build && cd Build`

OpenSSL Build
---

`wget https://www.openssl.org/source/openssl-1.0.2q.tar.gz`

`tar xvf openssl-1.0.2q.tar.gz`

`sudo cp openssl-1.0.2q /usr/local/openssl`

`cd /usr/local/openssl`

`./config -fPIC -shared`

`sudo make`

`sudo make install`

`sudo cp ./*.{so,so.1.0.0,a,pc} ./lib`

`cd ~/Build`

Python Build
---

`export LD_LIBRARY_PATH=/usr/local/openssl/lib:/usr/local/lib:$LD_LIBRARY_PATH`

`wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tgz`

`tar zxvf Python-3.9.6.tgz`

`cd Python-3.9.6`

`./configure enable-optimizations`          ----- Fails/Failed

`LDFLAGS="-L/usr/local/openssl/lib" ./configure --with-openssl=/usr/local/openssl --enable-shared`

`make`

`sudo make install`

`cd ..`

PIP Requirements (broken down)
---

`pip3 install opencv-python-headless`       ---- gets numpy

`pip3 install pyusb ezdxf`

`pip3 install wxpython`                     ---- gets Pillow + six

MeerK40t
---

`git clone https://github.com/meerk40t/meerk40t.git`

`cd meerk40t`


`python3 meerk40t.py`



Optional to build binary:
---
if not already there `cd Build/meerk40t`

`pip3 install pyinstaller`

`mv meerk40t.py mk40t.py`

`/home/XXXX/.local/bin/pyinstaller --onefile --windowed --name MeerK40t --add-data 'locale/:locale/' mk40t.py`

`mv mk40t.py meerk40t.py`


`./dist/MeerK40t`