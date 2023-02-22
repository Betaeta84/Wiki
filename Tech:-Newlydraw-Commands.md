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

# Captures
1. ZZZFile0;UL;ZED;
2. ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU-394,0;ZED;
3. ZZZFile0;ZQ;ZED
4. ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU0,396;ZED;
5. ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU394,0;ZED;
6. ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU0,-396;ZED;
7. ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PA;PU3957,19705;ZED;
8. ZZZFile0;RS;ZED;
9. (disabled) -- Expected ZZZFile;CV10;CR1;ZED;
10. (disabled) -- Expected ZZZFile;CV10;CR-1;ZED;
11. (disabled) -- Expected ZZZFile;CV10;WU10;ZED;
12. ZZZFile0;ZH1;ZED;
13. ZZZFile0;ZG1;ZED;
14. ZZZFile0;ZG;ZED;
15. ZZZFile0;ZK1;ZED;
16. ZZZFile0;ZT;ZED;
17. ZZZFile0;ZQ;ZED
18. ZZZFile0;PL2;DA60;TO50;ZED;
19. ZZZFile0;WV10;WR400;ZED;
20. ZZZFile0;WV10;WR-400;ZED;
21. ZZZFile0;WV10;WU400;ZED;

<img width="485" alt="secondsequence" src="https://user-images.githubusercontent.com/3302478/220779531-921628da-0209-4087-9892-78ae55d33293.png">


# External Links and Resources
* https://github.com/lynxis/laserusb