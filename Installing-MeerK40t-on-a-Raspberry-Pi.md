The easy way to run MeerK40t on the Raspberry Pi is to download a pre-compiled binary release for the Raspberry Pi. They are marked MeerK40t_Pi in the release area.

Building MeerK40t is also possible; albeit time consuming. This is because a snapshot build of wxPython 4.1 is required (available at https://wxpython.org/Phoenix/snapshot-builds/ ). On a Pi 3 B+ this process will take about 24 hours. On an original Pi B+ it will take about 96 hours. Both need virtual memory to be temporarily increased to 1,024 MEG for the build process to complete successfully. To do so; edit /etc/dphys-swapfile by setting CONF_SWAPSIZE=1024 (typical default is  CONF_SWAPSIZE=100); then reboot.

Download the appropriate wxPython-4.1.tar.gz. (For example; at the time of this writing the current latest version is named wxPython-4.1.0a1.dev4561+9407f765.tar.gz)

The commands for these steps are as follows done in the home directory (~), or whatever nested directory you prefer:
1. tar -zxvf wxPython-4.1.0a1.dev4561+9407f765.tar.gz
2. cd wxPython-4.1.0a1.dev4561+9407f765
3. python3 build.py build
4. python3 build.py install
5. cd ..
6. pip3 install Pillow pyusb
7. git clone https://github.com/meerk40t/meerk40t.git
8. cd meerk40t
9. python3 MeerK40t.py

Self contained installations can then be created by adding pyinstaller. 
1. pip3 install pyinstaller
2. cd meerk40t (if you aren't already in the directory)
3. pyinstaller --windowed --onefile MeerK40t.py
The compiled application will be placed in a new directory named dist.