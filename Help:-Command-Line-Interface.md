The MeerK40t CLI:
```
usage: MeerK40t.py [-h] [-z] [-c] [-a] [-p PATH] [-t TRANSFORM] [-o OUTPUT]
                   [-v] [-m] [-s [key=value]] [-H] [-O] [-b BATCH] [-S SPEED]
                   [-gs GRBL] [-gy] [-gx] [-ga ADJUST_X] [-gb ADJUST_Y] [-rs]
                   [input]

positional arguments:
  input                 input file

optional arguments:
  -h, --help            show this help message and exit
  -z, --no_gui          run without gui
  -c, --console         start as console
  -a, --auto            start running laser
  -p PATH, --path PATH  add SVG Path command
  -t TRANSFORM, --transform TRANSFORM
                        adds SVG Transform command
  -o OUTPUT, --output OUTPUT
                        output file name
  -v, --verbose         display verbose debugging
  -m, --mock            uses mock usb device
  -s [key=value], --set [key=value]
                        set a device variable
  -H, --home            prehome the device
  -O, --origin          return back to 0,0 on finish
  -b BATCH, --batch BATCH
                        console batch file
  -S SPEED, --speed SPEED
                        set the speed of all operations
  -gs GRBL, --grbl GRBL
                        run grbl-emulator on given port.
  -gy, --flip_y         grbl y-flip
  -gx, --flip_x         grbl x-flip
  -ga ADJUST_X, --adjust_x ADJUST_X
                        adjust grbl home_x position
  -gb ADJUST_Y, --adjust_y ADJUST_Y
                        adjust grbl home_y position
  -rs, --ruida          run ruida-emulator
```

---
### Authors
The MeerK40t team is grateful for the help from @tatarize in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃