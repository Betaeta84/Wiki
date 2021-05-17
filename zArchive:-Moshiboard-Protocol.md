Note: This Wiki page is provided for historical note and does not appear relevant for MeerK40t technicalities.

Moshiboards were a popular type of laser controller circa 2013. These ran a proprietary bit of software called Moshidraw which required an orange SafeNet MicroDog dongle to run. 

# USB Protocol
The USB plug on the board is connected directly to the CH341A communications chip and configured in parallel port (EPP19) mode. This gives it the same Vendor id and product id as other things like the M2 Nano.

The usb device is located at vendor 0x1a86 with a PID of 0x5512

The CH341 is able to return status codes. The relevant values within Moshiboards are 205 and 207. 205 means it's in lasering mode, and is able to take additional laser data. Whereas 207 means either busy because of an epilogue request, where the laser cutter is still doing work. Or because of a read request, where data is being read from the board. 

Moshiboards use two different data send protocols these are CH341EppWriteData and CH341EppWriteAddr. Which get translated by the driver to A6 and A7 respectively. And a read protocol called CH341EppWriteRead which sends AC though within moshi this always sends `AC01`.

## USB Protocol Startup

(This is not needed to control the board)

The first thing done upon connection is to request the chip version number. This is a control request:
```
bmRequestType 0xC0
bRequest: 95
wValue: 0x0000
wIndex: 0
wLength: 8
```
This returns the chip version which is 2 bytes little endian and often something like 0x3000 which is 48. 

---

(This is *needed* to control the board)

When connecting it then switches the chip into EPP 1.9 Parallel Port Mode with a USB control transfer. This is intended to switch the CH341 chip itself into the correct mode. This sends a CH341SetParaMode=1. 
```
control_transfer:
bmRequestType: 0x40
bRequest: 154
wValue: 0x2525
wIndex: 257
wLength: 0
```

## USB Read.

(This is not needed to control the board)

Moshidraw early on requests data from the controller it does this through a series of actions it sends A7-read commands. When in lasering mode (205) which it should be in at the start unless it's currently lasering something from a previous execution. The A7-read command switches the mode and it returns mCh341_PARA_CMD_STS (byte 1 (second byte)) of 207. It then does AC01 requests to read a single byte of data and does this until finished. 

* -> A7-read
* -> mCh341_PARA_CMD_STS
* <- 207
* -> AC01
* <- Byte


And repeats this until the entire dataset is read.

```
Offset(h) 00 01 02 03 04 05 06 07

00000000  01 80 0E F5 E6 F8 03 24  .€.õæø.$
00000008  0A E5 0F F5 E6 F8 02 24  .å.õæø.$
00000010  0A E5 10 F5 E6 F8 04 E8  .å.õæø.è
00000018  80 01 60 03 70 11 F5 E6  €.`.p.õæ
00000020  0A A8 F1 0A 75 75 07 80  .¨ñ.uu.€
00000028  03 80 02 4F 30 03 60 05  .€.O0.`.
00000030  70 D5 50 94 D3 20 54 87  pÕP”Ó T‡
00000038  E5 12 01 80 12 06 56 12  å..€..V.
00000040  04 70 06 60 F2 70 08 45  .p.`òp.E
00000048  09 E5 08 15 02 70 09 15  .å...p..
00000050  09 E5 09 F5 08 F5 FF 74  .å.õ.õÿt
00000058  87 F5 E4 B5 C2 30 12 02  ‡õäµÂ0..
00000060  80 0C 15 02 22 AF D2 F0  €..."¯Òð
00000068  ED 7A 0A 12 7F 7E FD E6  íz...~ýæ
00000070  F8 B0 13 02 C8 1C 02 88  ø°..È..ˆ
00000078  19 02 19 88 12 30 5B 0F  ...ˆ.0[.
00000080  8E AA F1 A8 0A E6 F5 11  Žªñ¨.æõ.
00000088  70 03 01 11 05 DA 06 10  p....Ú..
00000090  42                       B
```

The meaning of this data is unknown. However, MoshiDraw knows what kind of board is being used here after it's been read. This thread attempting to read this data will keep working indefinitely until that data is read. However the board will work fine without that happening.

In order to leave this mode it sends the termination sequence of 7 termination commands. E.G. `a6 0d 25 2d 2d 05 05 0d` where A6 is the standard WriteData command and the bytes are all termination command. Followed by an epilogue command.

The status mode will then read 205 as laser ready.

# Coordinate Sytem.
The coordinate system is absolute. All values relative to the set offset position. And are in mils/steps from offset-origin point. Negative values are permitted, however using them such that the offset + position is still negative appears to send the board to the positive value thereof.  These are stored in 16 bit little endian formatting.

# Commands

The commands issued are single byte commands. However 4 of those bytes are literally randomish and the other 4 determine the command type. Which gives 16 different commands types for either the A6 commands or the A7 commands both which use the same swizzling.

## Swizzling

Half of the command byte has random data (random distribution) and the other half is control code. This gives us 4 bits of command code, 4 random bytes. The swizzling can actually thus be interpreted in several ways, but we'll put the first four bits as command and the last 4 bits as random.

The algorithm from unswizzled to swizzled form is:

    def swizzle(b, b0, b1, b2, b3, b4, b5, b6, b7):
        return ((b >> 0) & 1) << b7 | ((b >> 1) & 1) << b6 | \
               ((b >> 2) & 1) << b5 | ((b >> 3) & 1) << b4 | \
               ((b >> 4) & 1) << b3 | ((b >> 5) & 1) << b2 | \
               ((b >> 6) & 1) << b1 | ((b >> 7) & 1) << b0

    def convert(q):
        if q & 1:
            return swizzle(q, 7, 6, 2, 4, 3, 5, 1, 0)
        else:
            return swizzle(q, 5, 1, 7, 2, 4, 3, 6, 0)

Note that each lowest bit triggers a different type of swizzle. So every other bit is swizzled in a different form. The last bit remains in the same position in both so these different swizzles perfectly intermingle.

### Swizzle LUT
```
0x00 || 0x01 || 0x40 || 0x03 || 0x10 || 0x21 || 0x50 || 0x23 || 0x04 || 0x09 || 0x44 || 0x0b || 0x14 || 0x29 || 0x54 || 0x2b
0x08 || 0x11 || 0x48 || 0x13 || 0x18 || 0x31 || 0x58 || 0x33 || 0x0c || 0x19 || 0x4c || 0x1b || 0x1c || 0x39 || 0x5c || 0x3b
0x80 || 0x05 || 0xc0 || 0x07 || 0x90 || 0x25 || 0xd0 || 0x27 || 0x84 || 0x0d || 0xc4 || 0x0f || 0x94 || 0x2d || 0xd4 || 0x2f
0x88 || 0x15 || 0xc8 || 0x17 || 0x98 || 0x35 || 0xd8 || 0x37 || 0x8c || 0x1d || 0xcc || 0x1f || 0x9c || 0x3d || 0xdc || 0x3f
0x02 || 0x41 || 0x42 || 0x43 || 0x12 || 0x61 || 0x52 || 0x63 || 0x06 || 0x49 || 0x46 || 0x4b || 0x16 || 0x69 || 0x56 || 0x6b
0x0a || 0x51 || 0x4a || 0x53 || 0x1a || 0x71 || 0x5a || 0x73 || 0x0e || 0x59 || 0x4e || 0x5b || 0x1e || 0x79 || 0x5e || 0x7b
0x82 || 0x45 || 0xc2 || 0x47 || 0x92 || 0x65 || 0xd2 || 0x67 || 0x86 || 0x4d || 0xc6 || 0x4f || 0x96 || 0x6d || 0xd6 || 0x6f
0x8a || 0x55 || 0xca || 0x57 || 0x9a || 0x75 || 0xda || 0x77 || 0x8e || 0x5d || 0xce || 0x5f || 0x9e || 0x7d || 0xde || 0x7f
0x20 || 0x81 || 0x60 || 0x83 || 0x30 || 0xa1 || 0x70 || 0xa3 || 0x24 || 0x89 || 0x64 || 0x8b || 0x34 || 0xa9 || 0x74 || 0xab
0x28 || 0x91 || 0x68 || 0x93 || 0x38 || 0xb1 || 0x78 || 0xb3 || 0x2c || 0x99 || 0x6c || 0x9b || 0x3c || 0xb9 || 0x7c || 0xbb
0xa0 || 0x85 || 0xe0 || 0x87 || 0xb0 || 0xa5 || 0xf0 || 0xa7 || 0xa4 || 0x8d || 0xe4 || 0x8f || 0xb4 || 0xad || 0xf4 || 0xaf
0xa8 || 0x95 || 0xe8 || 0x97 || 0xb8 || 0xb5 || 0xf8 || 0xb7 || 0xac || 0x9d || 0xec || 0x9f || 0xbc || 0xbd || 0xfc || 0xbf
0x22 || 0xc1 || 0x62 || 0xc3 || 0x32 || 0xe1 || 0x72 || 0xe3 || 0x26 || 0xc9 || 0x66 || 0xcb || 0x36 || 0xe9 || 0x76 || 0xeb
0x2a || 0xd1 || 0x6a || 0xd3 || 0x3a || 0xf1 || 0x7a || 0xf3 || 0x2e || 0xd9 || 0x6e || 0xdb || 0x3e || 0xf9 || 0x7e || 0xfb
0xa2 || 0xc5 || 0xe2 || 0xc7 || 0xb2 || 0xe5 || 0xf2 || 0xe7 || 0xa6 || 0xcd || 0xe6 || 0xcf || 0xb6 || 0xed || 0xf6 || 0xef
0xaa || 0xd5 || 0xea || 0xd7 || 0xba || 0xf5 || 0xfa || 0xf7 || 0xae || 0xdd || 0xee || 0xdf || 0xbe || 0xfd || 0xfe || 0xff
```

All commands will be referenced by the first 4 bits. The remaining 4 bits fit a random distribution. These do not need a mixture. The board will respond to all of them so it's best to just use the first one and ignore the rest.

## A6 Moshiboard Commands
```
| SET_OFFSET  || 0 || 0x0? || z, x, y (6 bytes)
| UNKNOWN || 1 || 0x1? || unknown
| Terminal Commands || 1 || 0x2? || occurs in 7 byte clusters
| Move_Y_ABS || 1 || 0x3? || y, (2 bytes)
| Speed Raster || 1 || 0x4? || 1 byte
| Speed Vector || 1 || 0x5? || 2 bytes, speed and normal speed
| Move_X_ABS || 1 || 0x6? || (2 byte ABS X)
| Move_XY_ABS || 1 || 0x7? || (2 byte ABS X) (2 byte ABS Y)
| UNKNOWN || 1 || 0x8? || unknown
| UNKNOWN || 1 || 0x9? || unknown
| UNKNOWN || 1 || 0xA? || unknown
| Cut_Y_ABS || 1 || 0xB? || (2 byte ABS Y)
| UNKNOWN || 1 || 0xC? || unknown
| UNKNOWN || 1 || 0xD? || unknown
| Cut_X_ABS || 1 || 0xE? || (2 byte ABS X)
| Cut_XY_ABS || 1 || 0xF? || (2 byte ABS X) (2 byte ABS Y)
```

Bitwise this implies our flags for our 4 data bytes are: Laser, X-Value, Unknown, Y-Value

## A7 Moshiboard Commands

The A7 commands are WriteAddr commands these follow the same swizzling as the other commands and can just use any single form for every command.

```
| UNKNOWN  || 0 || 0x0? ||
| Freemotor/Stop || 1 || 0x1? || Stops current work in progress. Unlocks the rail.
| Epilogue || 1 || 0x2? || Sets the program to remain in 207 until next send is possible. 205.
| UNKNOWN || 1 || 0x3? ||
| UNKNOWN || 1 || 0x4? ||
| UNKNOWN || 1 || 0x5? ||
| Prologue || 1 || 0x6? || Sets the board to accept an A6 bunch of program code.
| Laser? || 1 || 0x7? || Seen when hitting the laser button in Moshidraw, might toggle the laser.
| UNKNOWN || 1 || 0x8? ||
| UNKNOWN || 1 || 0x9? ||
| UNKNOWN || 1 || 0xA? ||
| UNKNOWN || 1 || 0xB? ||
| UNKNOWN || 1 || 0xC? ||
| UNKNOWN || 1 || 0xD? ||
| READ || 1 || 0xE? || Read data mode. Seen during the read phase of boot.
| UNKNOWN || 1 || 0xF? ||
```

## Vector Program

The standard order for program is to start with Prologue, then send A6 commands. Then Epilogue and wait for the 205 signal. We start with Vector Speed which has two speeds within it, these are cut speed and normal speed. It is not clear if the normal speed is the rapid move speed but that seems likely. This would be something like `0x0a 0x13 0x27` 0A is the first form of the right data. 0x13 is 19 in decimal which is a speed of 20. 0x27 is 39 decimal which is a speed of 40 mm/s for "normal". Speed 0 is impossible. Since 0 gets 1 added and is a speed of 1mm/s. This is followed by the offset, for example `0x00 0x00 0x00 0x00 0x00 0x00 0x00` which is 0x00 the first command, SET_OFFSET, then the values are z = 0, x = 0, y = 0. Then we have a series of CUT_XY and MOVE_XY commands. We do not use the X or Y only forms those are purely for rasters. When finished we do a sequence of 7 terminal bytes for the terminal sequence.

### Jogs

Jogs are done in a vector program. These actually move the offset to the location and then move to 0,0 which is the current location. For example: `a61a13274000008a018a01ca00000000272f2f2f272727`

```
Vector Cut Speed: 20 (0x13) mm/s Normal Speed: 40 mm/s (0x27)
Set Location unknown: 0, x: 394, y: 394 (0000,8a01,8a01) 
move x: 0, y: 0 (0000,0000) 
Termination. * 7
```

## Raster Program

The standard raster program starts with a different speed command which takes only one speed which is in cm/s rather than mm/s. So we start with for, for example, `0x02 0x10` which is the first form of the raster speed code forms, and the value of 16 which is 160mm/s. We then set the position as usually, though this is really the current position. As we should have jogged to this position. All the motions done are either X or Y only. Both are this allows for y-rastering and x-rastering and the various steps in the correct directions. Finally when done we send terminal bytes. Send the A7 Prologue, and wait for it to finish.

# Implementations

To implement a Moshiboard Driver we need to connect to the CH341 and change the mode to EPP 1.9 with the control transfer. Everything we do is a program bookended with prologue and epilogue mode shifting commands. So to jog we send:
CH341WriteAttr(<Prologue>)
CH341WriteData(Program)
CH341WriteAttr(<Epilogue>)

We can then wait for our checks to the system request for the current status to be 205 rather than 207 then we send the next data using the same methodology. We do not have to worry about any checks between packets. Programs come in two flavors Vector and Raster:
* Vector programs are vector speed, set position, moveXY, cutXY, etc. termination sequence.
* Raster programs are raster speed, set position, moveX, cutX, moveY, cutY, etc. termination sequence.

The swizzling isn't needed since we can just use the first form of all the commands we need. We don't need to read data from the board, or check the chip version. We can just put it in EPP1.9 check the status and start sending data. Only using A782 <prologue> and A7080 <epilogue> and small versions of the vector and raster programs to move around. The board maintains its known position. So it can just jog to the start location, perform these actions. Etc. That's enough to do all the work needed.


# Links Official

* [http://www.moshidraw.com/English/Product/software/ Moshidraw]