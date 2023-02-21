# NewlyDraw connection
Data is transferred over the USB at the vid/pid 0x0471 0x0999.

# NewlyDraw protocol
* The protocol is to connect to the WRITE_INTERRUPT (0x01) endpoint transfer the packet size in int16le from 0000 to 1000 (4092).
 Packets are truncated at 4092 and remainder is sent in next packet. This is not sliced at a command end but mid command.
* Read from the READ_INTERRUPT (0x81) endpoint for the value 01 which means OK.
* Then send the WRITE_BULK endpoint (0x02) the packet of modified HPGL like code.

# NewlyDraw commands
Newly draw internally uses an HPGL-like formatting with specialty commands including a length based raster routine.

## UL 
Unlock rail

## PR (HPGL)
Pen Relative

## IN (HPGL)
Initialize

## DW
Unknown

## ZED
File end.

## SP# (HPGL)
Select Pen. Different numbers are seen this may be a mode.

## BT#
Unknown

## TO#
Unknown

## DA#
Unknown

## PL#
Unknown

## BC#
Unknown

## BD#
Unknown

## TX#
TX255 is likely max.

## PU (HPGL)
Pen Up to location. Do not fire laser.

## PD (HPGL)
Pen Down to location. Do not fire laser.

## ZZZFile#
Start file. Always seen as first command.

## VS#
Unknown

## BT#
Unknown

## VQ#
Speed. Advanced Speed0.

## VP#
Unknown

## CV#
Unknown

## WV#
Unknown

## WU#
Unknown

## WR#
Unknown

## VX#
Unknown

## VY#
Unknown

## VM#
Unknown

## VJ#
Unknown

## VK#
Unknown

## CU#
Unknown

## WV#
Unknown

## WU#
Unknown

## WR#
Unknown

## CR#
Unknown

## PA (HPGL)
Pen Absolute

## ZQ
Stops the laser.

## ZT
Unknown

## ZG
Unknown

## RS
Unknown

## ZH#
Unknown

## ZK#
Unknown

## ZG#
Unknown

## YF/YZ
Raster command. This is followed by 0x00 then the length in bits of the relevant data to follow and then a string of bytes zero-padded to the nearest full byte.


# External Links and Resources
* https://github.com/lynxis/laserusb