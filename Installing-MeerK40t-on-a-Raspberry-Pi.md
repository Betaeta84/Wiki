With regard to issue #84 ( https://github.com/meerk40t/meerk40t/issues/84 )

6 steps to running MeerK40t on the Raspberry Pi (OS Buster+).

1. Make sure you have python supporting the latest version of wxPython.
2. Downloads the master branch of meerk40t.
3. Unzips the master.zip
4. Enters the meerk40t-master directory.
5. install requirements.
6. runs MeerK40t

The commands for these steps are as follows done in the home directory (~):

1. ``sudo apt-get install python3-wxgtk4.0``
2. ``wget http://github.com/meerk40t/meerk40t/archive/master.zip``
3. ``unzip master.zip``
4. ``cd meerk40t-master/``
5. ``pip3 install -r requirements.txt``
6. ``python3 MeerK40t``