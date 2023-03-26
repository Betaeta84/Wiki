
# G3Version8

The G3Version8 laser cutting board is typically controlled with NewlyDraw specifically with the EnG3BinV8.dll driver. Most of the remaining Newlydraw supporting devices run Leetro lasers. 


## USB Protocol

The USB transfer protocol is straight forward and was first determined by Lynxis (see other sources). The protocol is connect to the USB device located at the VID/PID 0x0471 0x0999 -- This give the VID as `Philips (or NXP)`.

Each send send data event has three steps.


1) WRITE_INTERRUPT (0x01) endpoint transfers the packet size in int16le from 0x0000 to 0x1000 (4091). Spanning more than one packet requires truncation (without regard to the current space at 0x1000).

2) READ_INTERRUPT (0x81) endpoint should respond 0x01 of 1 byte which means OK. According to Lynxis' work, anything other than the 0x01 means failure.

3) WRITE_BULK (0x82) endpoint will be sent the packet of the size specified.

## Coordinate System

The coordinate system as seen from reverse engineering on a Workline/USB device was that of having swapped X and Y values. The NewlyDraw device tends to specify a given DPI for the Width and Height. Usually this appears to be near 1000 making meaning the typical step size is about 1 mil (1/1000th of an inch).

## Format
The principle underlying formatting is HPGL. All commands are single commands delimited by semicolons (`;`). Some metastructural commands are added as well as realtime laser control commands which are not part of typical HPGL command set. However, all commands are formatted like HPGL commands.


### File Structures

The file structure to convey the modified HPGL are not found in any of the regular [https://en.wikipedia.org/wiki/HP-GL HPGL] commands. All data is stored in a files these all begin with `ZZZFile#` which specifies the file we are writing. These are stored on an SD card located on the controller. The SD card is not formatted with a FAT-like data structure or anything directly and obviously accessible. This files can be created, sent, and "started" in the regular use of the laser. These also remain stored in persistent memory so that the same job can be executed multiple times in a single command. While the file numbers go from 0-9 this gives 8 different regular use files for saving and replaying.

The file contains a root and two different sections.
These sections are the root, `DW` section and the `GZ` section. Sections are ended with the command `ZED`.

#### Root
Data not sent as part of a given draw section are usually intended for direct execution.

#### Frame Section
The `DW` section contains the frame. This is the job frame outline of the data begin sent. This gets executed by the realtime `Draw Frame` `ZH#` and `Move Frame` `ZK#` commands which are listed in NewlyDraw's control panel as `Draw Rect` and `Move Rect`. These are commands to move around the edges of the job, to trace the edges of the job. This outer edge trace is provided within the file so that it can be executed to check the frame of the job prior to calling `Execute` `ZG#` (called `Start` in Newlydraw).

#### Program Section
The second section is `GZ` section this section is executed when `Start` (`Execute`) is called with the command `ZG#` as a realtime command.

#### ZZZFile0
The File0 is usually executed directly and instantly. So realtime commands like Run a program in `File2` would be `ZZZFile0;ZG2;ZED;` this would execute the command `ZG2` directly. It runs in File0 so that it executes rather than is merely stored in a file and not executed until instructed to do so. In NewlyDraw the `ZG#` are usually called automatically after the file is sent, but this is not necessary. This file is used for realtime commands, and to execute movements directly on the laser. While File0 commands usually do not contain other sections they do end with `ZED` commands.

#### ZZZFile#
The `File1` through `File9` designations are usually sent as full programs with both a `DW` and `GZ` section. They are usually structured `ZZZFile1;DW;<Frame Commands>;ZED;GZ;<Program Commands>;ZED;`

These are not automatically executed. The Frame Section `DW` is executed by the `ZH#` or `ZK#` commands. The `GZ` section is executed by the `ZG#` command.

#### ZED
The ZED command closes the file. This is done after the root end of the root or after the end of each of the `DW` or `GZ` sections.



### Realtime Commands

The realtime commands are usually sent as part of a File0. This is usually a single command or a short set of commands.
The usual structure is: `ZZZFile0;<Realtime>;ZED;`

#### Laser Control
* `ZZZFile0;UL;ZED;` - Unlock Rail.
* `ZZZFile0;ZQ;ZED` - Stop. Halt Execution.
* `ZZZFile0;RS;ZED;` - Home device.
* `ZZZFile0;ZH1;ZED;` - Draw Rect Program 1
* `ZZZFile0;ZG1;ZED;` - Start Program 1
* `ZZZFile0;ZG;ZED;` - Resume Current Job.
* `ZZZFile0;ZK1;ZED;` - Move Rect
* `ZZZFile0;ZT;ZED;` - Pause Current Job.
* `ZZZFile0;ZQ;ZED` - Stop Current Job
* `ZZZFile0;PL2;DA60;TO50;ZED;` - Fire laser in place. Power 100%, 50 MS.

#### Jog
* `ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU-394,0;ZED;` - Move Top 10mm
* `ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU0,394;ZED;` - Move Left 10mm
* `ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU394,0;ZED;` - Move Bottom 10mm
* `ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PR;PU0,-394;ZED;` - Move Right 10mm
* `ZZZFile0;VP100;VK100;SP2;VQ15;VJ5;VS5;PA;PU{x},{y};ZED;` - Directly move to position X, Y

#### Z Axis Realtime
* `ZZZFile;CV10;CR1;ZED;` - Step Z axis 1 units (speed 10)
* `ZZZFile;CV10;CR-1;ZED;` - Step Z axis 1 units (speed 10)
* `ZZZFile;CV10;W2U10;ZED;` - Move Z axis to position 10 (speed 10)

#### W Axis Realtime
* `ZZZFile0;WV10;WR400;ZED;` - Move W axis 400 units (speed 10)
* `ZZZFile0;WV10;WR-400;ZED;` - Move W axis -400 units (speed 10)
* `ZZZFile0;WV10;WU400;ZED;` - Move W axis to position 400 (speed 10)

#### `VX/VY/VM` Home Velocities

This is the X Home Velocity, Y Home Velocity, and Angle Velocity. These are only ever directly specified as part of a realtime command:
`ZZZFile0;VX12;VY12;VM12;ZED;` - Which would set these values to 12 cm/s for each value. 


#### Captures
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



### Settings Commands

There are several novel and otherwise unseen commands used in the modified HPGL used by this control device. A couple like `VS` is seen but is treated considerably differently here. The obvious changes are the commands which convey meaning that would not be relevant to a pen plotter but would be relevant to a laser.

#### `VS` - Pen Speed
VS is common in regular HPGL however rather than convey a direct velocity the G3 actually creates a structure log-based table-lookup set of settings for the speed. There is significantly more need to differentiate low speeds than there are for high speeds.
* Speeds greater than 100mm/s are increments of 10mm/s from 0 to 127.
* Speeds less than 1mm/s are increments of 0.2mm/s starting at 127 (0mm/s) and ending at 132 (1mm/s).
* Speeds less than 5mm/s (but greater than 1mm/s) are increments of 0.2mm/s starting at 132 (1mm/s) and ending at 147 (5mm/s).
* Speeds less than 15mm/s (but greater than 5mm/s) are increments of 0.5mm/s starting at 147 (5mm/s) and ending at 162 (15mm/s).
* Speeds less than 100mm/s (but greater than 15mm/s) are increments of 1mm/s from 162 (15mm/s) and ending at 255 (93 mm/s)?

So for example, a speed of 18 mm/s would be between 15 and 100mm/s so it would be 147 + 18 = 165.

#### `VP` - Cut DC
Unlike most laser systems the G3V8 can specify the current to the stepper motors during a cut.

#### `VK` - Move DC
Specifies the current to the stepper motors during a movement.

#### `VQ` - Corner Speed
This is the speed at which the device moves from one corner to another corner. The acceleration will go for longer than the drawn line, and it will overstep that value. And for the next line needs to move to a position to accelerate into that corner. This is the speed of the laser moving from that corner to the next corner.


#### `VJ` - Acceleration Length

This is the length used to get up to the required speed.

#### `PL` - PWM Frequency

Power Width Modulation Frequency. Usually pretty low like `PL2` or `PL4` it sets the power-width-modulation frequency or how fast it alternates to convey the power width.

#### `DA` - Power Selection

DA denotes power between 0 and 255 (100%).

#### `BD` - Raster Step Horizontal

This controls the steps taken per bit of a `YF` or `YZ` command.

#### `BC` - Raster Step Vertical

This controls the steps taken per bit of the `XF` or `XZ` command.

#### `BT` - Raster Unknown
Unknown raster-only setting value.



### Raster Commands

The rasters used by the G3V8 are bit-based rasters. Rather than convey specific commands to do each movement it specifies a directional command `YF`, `YZ`, `XF`, `XZ` this is followed by 3 bytes of size in `int24le` format. And then a series of bits padded out to the next full word. So 10 bits would write as `XF\x00\x00\x0A\xFF\xC0` Giving the specific size as 10 `0x0A` and specifying 8 bits of 1s and then two additional bits of 1 aka, `0b1100_0000`.

The `YF` and `XF` commands are processed backwards from the direction. So, regardless whether the laser is going right-to-left or left-to-right the bitstream is always written right-to-left.

Most raster commands begin with `YZ` which is Right Raster (in the reverse engineered coordinate system).

For example `GZ;IN;PL4;VP100;VK100;SP2;SP2;VQ15;VJ24;VS10;PR;PR;PR;PR;PU5481,-14819;BT1;DA50;BC0;BD8;PR;PU8,0;SP0;VQ20;VJ18;VS30;YZ��.ÿÿÿÿÿø;PR;PU8,0;YF��.@�����;`

####  `BD` Raster step YF, YZ

The BD modifier means each bit counts as the specified number of steps. BD8 for example means each step along the YF or YZ value is 8 steps rather than 1.

#### `BC` Raster Step XF, XZ

The BC modifier means each bit along the XF and XZ directions counts as the specified number of steps.

#### `BT` Unknown

BT is only seen in rasters and usually has a value of 1 but is not specifically known at this time.

#### `YZ` Raster Right

A right raster will start by going left, rastering while going right, then bouncing back after stopping.

#### `YF` Raster Left

A left raster will start by going right, rastering while going left, then bouncing back after stopping.

A YF raster's bitstream is processed in reverse of the directionality.

#### `XZ` Raster Bottom

A bottom raster will start by going top, rastering while going bottom, then bouncing back after stopping.

#### `XF` Raster Top

A top raster will start by going bottom, rastering  while going top, then bouncing back after stopping.

A XF raster's bitstream is processed in reverse of the directionality.




### Timing

The timing commands are wait or dwell commands specified in ms. The value range is between 0 and 255. If you need to specify a time greater than 255 this should be done by stringing multiple commands together. The features for `delay laser on` and `delay laser off` are implemented with these commands. If the laser doesn't begin firing right away a delay for `TX<value>` can be implemented before the `PD<X>,<Y>` command is sent. This turns the laser on for that amount of time before the laser movement should begin. This would ensure the laser delays before a cut. A similar strategy is used to implement `delay laser off` issuing the `TX<time_in_ms>` after the end of the laser cut.

#### `TO` - Wait
Wait this specific amount of time. Without any laser firing.

#### `TX` - Dwell
Fire the laser for this amount of time, at the current location.




### HPGL Standard

Most of the rest of the commands used are found in standard HPGL.

#### `IN` Initialize

Initialize resets various settings, but this is only seen during a raster. When a raster section is used the typical commands are `ZED;GZ;IN;` which ends the previous `DW` section starts the `GZ` section and then resets the settings. While this is a standard HPGL command it's only seen during raster evetns.

#### `SP` - Select Pen

In the pen plotter world this would actually select a different pen which could be a different color and set at a different speed and would execute those relevant commands. In the G3Version8 it tends to get called without much in the way of good reason. The `DW` section is usually run in `SP0` whereas the programmed sections tend to get run in `SP1` and `SP2` traditionally this would have it's own speed and other settings. The `VP` and `VK` commands seem to only get set once, but usually we see speed set each time the pen is changed.

#### `PR` - Pen Relative

For unknown reasons this is sent multiple times by Newlydraw but it denotes that the PU and PD commands are relative to the current location rather than in absolute values.

#### `RA` - Pen Absolute

This denotes that the position given is in absolute values from 0,0 which is typically located in the upper left hand corner of the device.

#### `PD` - Pen Down

Move this direction firing the laser.

#### `PU` - Pen Up

Move to this position without firing the laser.

# Supporting Lasers

The lasers which use this underlying controller board are as follows:
* Raylaser\U-SET
* Beijing SZTaiming\usb
* Gama\usb
* CYBERTECH LTD\U-SET
* Artsign\JSM-40U/3040U/3060U
* Artsign\JSM-40N/3040N/3060N
* Light Technology\LH40U/3040U/3060U
* Greatsign\LE40U/3040U/3060U
* Helo Lasergraviermaschine\HLG 40N
* Workline laser\usb
* HPC LASER\LS 3020
* SICONO\SIC-L40B
* Rabbit\Rabbit40B
* ZL Tech\ZL40B
* Jinan Suke\USB
* Jinan Jinweik\Laser B
* Lion laser\usb
* VILLA L. & FIGLIO S.R.L.\Laser B
* AMC CO damascus\Laser B
* Jinan Ruijie\Laser U
* Mini Laser\USB
* Jinan Xinyi\USB
* Duowei Laser\Laser U
* Weifang Tiangong\Laser B
* Jinan DaGong\TLU series
* Jinan Senfeng\Laser U
* ZhengZhou LeCai\LC Laser
* ZhengZhou LeCai\LC Plasma
* Wuhan Jinli\JL Cylinder
* DongGuan EverTech\ETL3525 Laser
* DongGuan EverTech\ETL3525 Laser
* Wuhan Anwei\AW-U
* Liaocheng Xinxing\U

## G3 Prior to Version 8
* Raylaser\L-SET
* Artsign\JSM-3040/3060
* Light Technology\LH40/3040/3060
* Greatsign\LE40/3040/3060
* SICONO\SIC-L400
* Rabbit\Rabbit3040
* ZL Tech\ZL4030
* Jinan GuangHan\GH-3040
* Jinan Jinweik\Laser A
* Jinan Xinyi\Parallel
* Weifang Tiangong\Laser A
* Jinan DaGong\TL series
* Jinan Mingrui\MR-A
* Jinan Senfeng\6090A
* Jinan Senfeng\1280A
* Jinan Xunjie\XJ-III
* Jinan XingHui\xh1490/6040/9060B
* Liaocheng Kexin\kx-560 580 690
* Tianjin XinTai\ST1310
* Tianjin XinTai\Laser/Plasma
* GuangDong YangSheng\YS-I Laser
* DongGuan EverTech\ETL40 Laser
* DongGuan EverTech\ETL60 Laser
* DongGuan HuaTian\HT-DK-A
* ShenZhen SanNeng\SN-60
* ShenZhen SanNeng\SN-80A
* Hubei SenMao\SM Laser
* Shanghai JianCheng\JC960A
* Wuhan Huaguang\HG-8A
* Wuhan Meisi\MS-I Fire
* Wuhan Meisi\MS-I Laser
* Wuhan Meisi\MS-I Plasma
* Wuhan Anwei\AW-H
* Wuhan Tiangong\HGVII
* Wuhan Sangong\SCC-C
* Wuhan ChangXin\CXA-6040
* Beijing Hongdat\CD46F
* Jinan Jinshida\KEN-RASER
* Shandong HongJiShunKe\HJSK6090B
* Guangzhou Huaz\HZL40 Laser
* Guangzhou Huaz\HZL60 Laser
* Guangzhou HongYi\HONGYI-I
* Wuhan Ray-Tech\RDL40 Laser
* Wuhan Ray-Tech\RDL60 Laser
* Jinan Senfeng\6090
* Jinan DaGong\D9260
* Jinan DaGong\D1080
* Liaocheng Xinxing\DG-07A
* Beijing Shengyuan\DG-07A

# Links

## Reverse engineering resources.
* [https://github.com/lynxis/laserusb Lynxis LaserUsb] First known attempt to reverse engineer the device.
* [https://edutechwiki.unige.ch/en/G3Version8 Edutech documentation] Edutech documentation on the format.
