This page is a consolidation of various previous wiki pages for M2-Nano **hardware**. If you have any further information about the M2-Nano hardware, please add it here rather than creating a new Wiki page.

For information about how to control the M2-Nano (i.e. its g-code etc.) please see [Tech: Lhymicro M2-Nano Control]().

## Contents

* [Scott Marshall's Observations]()

## Scott Marshall's Observations
M2 Nano Laser Control Board

Observed and deduced information

by [Scott Marshall](mailto:scott594@aol.com?subject=Ref:%20MeerK40t%20Wiki)

### Introduction
This document attempts to serve the Google K40 Community and other users of the “M2 Nano” Controller board produced by Lihuiyu Studio Labs.

The board used for this examination is Version 7, and is what could be considered a 2nd Generation controller, replacing the infamous “Moshi-draw” multiple board arrangement used in many of the older K40 laser cutters.

The information in this document is not sourced from the manufacturer, nor proven accurate for any boards beyond the ones I have on hand.

### Health & Safety Warning
The K40 as supplied is a hazardous device, without any interlocks, safety equipment, UL or ANY approval, or even safety warning decals. Please be careful.
These Laser cutters should be used/modified only by qualified persons familiar with high voltages, laser light, and the proper procedures for working on the associated equipment.

I (Scott Marshall) am in no way responsible for any harm, damages, or injury/death that occurs while using this information.  I hope it helps you in your quest for laser Nirvana, and be careful  out there!

### The Board
On the Boards lower left (USB connector left) you find:
* the only large cap on the board, 47uf
* an unknown inductor
* a 104 (.1uf) RF bypass cap.

Between the Driver ICs and Motor connectors are 4 way Diode networks, and SMD Caps, most probably to suppress ringing form the motor windings.

### Interconnection
This section details each connector and what it's function is. 
There are still a few pins I'm not sure of (C4), but it's almost complete.

The board contains 7 Connectors, I have arbitrarily labeled them #1 - #7, 
starting with the power supply connector and proceeding clockwise as with a QFN.
Connector pins will be labeled Left to Right with board edge facing you.

#### Connector #1
Power Supply: 4 Pin 4 x .044” male pins, LO, 5V, GND, 24V

This Connector is used in the K40 to connect the M2Nano to the Combination High voltage laser/low voltage control power supply via a 4 conductor cable.
Its connections are as follows:

* PIN 1 -  LO connects to the 2 pin #6 connector Pin LO, and to FS2, a SMD fuse above the LM317. I suspect “LO” is Laser Operate.
* PIN 2 – 5V – as it says, 5 volt control power input. It is tied directly to FS1 (Right side) and leaving the fuse to Connector #7 pin #5 (5V), leaving  Connector #7, it's tied to L1, presumably a noise suppression choke and into the rest of the board.
* PIN 3 – GND – as it says it's Ground. It's tied to the lower ground plane directly at the pin, and to the top ground plane thru numerous vias.
* PIN 4 – 24V – This is probably only used to feed the A4895 Stepper Drivers and then the motors. It is connected to MG, which appears to be an anti-reverse/ring suppression diode, and a 47uf electrolytic with a 104(.1uf) high/low frequency filter network. From there it appears to go directly to the Allegro A4895 motor drivers.

#### Connector #2
This is has 4 male pins on .100” centers, a JST-4 style connector. It is labeled “Y”.

Pins 1-4 go to the Y axis motor connector.  The connector is located directly below the A4895 driver IC, and a network of 4ea 1260 size diodes (S6) and 2 capacitors (106). There are also 2 470 ohm 1260 resistors.

#### Connector #3 
This is has 4 male pins on .100” centers.  JST-4 style connector. It is labeled “X”.

This is the X Axis version of Connector #2, and the connections and support circuitry are identical to the Y axis.

#### Connector #4
This is a connector for 12 conductor (2 layer) 'FFP' Flat cable. It appears this connector is redundant, many of it's connections duplicate other connectors. The exception seems to be the Y axis Motor.

Pin 1 is indicated by a cut corner located in the upper left corner. 

##### Top Row
* Pin#1 – X Axis Motor, ties to #3-pin 1
* Pin#2 – (YL) Y Axis Limit Switch - tied to #4-pin 12, #8-pin 2(YL) 
* Pin#3 – GROUND - tied to #4-pin 9, all other GND pins
* Pin#4 - ?
* Pin#5 – X Axis Motor, tied to #3-pin 3
* Pin#6 – X Axis Motor, tied to #3-pin 2
##### Bottom Row
* Pin#7 - ?
* Pin#8 – X Axis Motor, tied to #3-pin 4
* Pin#9 – GROUND – Tied to #4-pin 3, all other GND pins
* Pin#10 –(XL) X Axis Limit Switch – tied to #8-pin 3 (XL)
* Pin#11 - ?
* Pin#12 - (YL) Y Axis Limit Switch tied to #4-pin 2, #8-pin 2(YL)

#### Connector #5
USB Type B Connection. This is the PC connection, and the only way to communicate with and move data into the Laser Cutter.

When the M2Nano is installed in the K40, there is a slot through which you connect your USB cable.

#### Connector #6
JST-2 male pin connector. Air Assist control.

This is a low level output which goes high when a job is started, then drops back to low approximately 15 sec after the job is completed. 
It requires an amplifier transistor to operate a relay coil. 
An “Arduino” Relay module could be used to control a solenoid valve, but external power would be needed to pull in the valve coil (fused 120vac could be used).

* Pin#1 – EX-  GROUND – to be used as the return side of the output.
* Pin#2 – EX+  This is the Logic level (5V<10ma) Air Assist control signal.

#### Connector #7
JST-2 male pin connector. “LO” 

This is a duplicate of the “LO” signal in the power supply cable (Connector#1-pin 1) and is a logic level (5V<10ma) signal that provides on/off control of the laser. 
* Pin#1 – GND - GROUND – to be used as the return side of the output.
* Pin#2 – LO – Logic level Laser fire output as described above.

#### Connector #8
JST-5 male pin connector. End of travel detection connector.

This connector contains ground and 5V power to supply the opto-interupter end of travel detection boards, and the 5V logic signal from them.
These signals zero the position counter.

* Pin#1, Pin#4 - GND – GROUND
* Pin#2 – YL – Y Limit – Y Axis end of travel (Top) signal (5V<10ma) 
* Pin#3 – XL – X Limit – X Axis end of travel (Left) Signal (5v<10ma)
* Pin#5 – 5V – 5 volt power to operate the LEDs in the opto-interupters.