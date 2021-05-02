
# K40 Nano Board Operations

The K40 board consists of two core chips a CH341A that does the communications and is put directly in PARALLEL mode for EPP 1.9, and the LHY730318 chip that does the code processing. Communication wise the K40 uses a USB at idVendor=0x1a86, and idProduct=0x5512, reading at address 0x82 and writes at address 0x2.

The data is written data typically in 2 packets of 34 bytes of data. An 0xA6 packet send command followed by 0x00 and the data, which is ended with 0x00 and the CRC code.

The CRC is onewire CRC coded data across the payload, bytes[2-32).

A packet may also consist of just 0xA0 (`HELLO` or mCh341_PARA_CMD_STS). When this happens the read will return six bytes of data. The second byte (Byte #1) contains the EPP pins of the data. 

* 206: `OK`. Everything is fine, we're ready.
* 238: `BUSY`. Cannot accept a packet just now.
* 207: `CRC_ERROR`. The CRC of that packet was wrong, and it was not processed.
* 236: `TASK_COMPLETE`. The 'F' command was sent and the stuff the K40 was doing has finished.
* 239: `POWER`. The board is just booting up or shutting off and doesn't have enough power.
* 204: `UNKNOWN_ERROR`. This can be produced by sending `FNSENSE` to the machine.

Update: Some experience with a lossy channel has lead to some revisiting of 207 as a CRC_ERROR the proper value is merely error. This happened while running the stock CH341 driver. So there are codes for 0,0 which may actually be LibUSB timeouts.

```
b'\x00iDaUhDaUhDaUiDaUhDaUiDaUhDaUiD\xb9'
[0, 0, 0, 0, 0, 0]
[255, 238, 0, 0, 0, 0]
[255, 238, 0, 0, 0, 0]
[255, 238, 0, 0, 0, 0]
[255, 238, 0, 0, 0, 0]
[255, 207, 0, 0, 0, 0]
```

The interesting thing here is that 207 happens after the 238 clears. So it's busy for a while, then it responds that there was an error. This ordering matters for some critical operations.

With proper resending on 207 there were a number of errors that cropped up within a noisy channel. 

Without resend a series of three rasters experienced:

First Raster:
```
b'\x00UdD|kUdDdU|kDdUdD072UhDdUhDdUd\x9c'
[0, 0, 0, 0, 0, 0]
[255, 207, 0, 0, 0, 0]
[255, 207, 0, 0, 0, 0]
b'\x00DdUdDdUdD|wUdD064U|cDdUdDtUdDp\xbe'
```

Second Raster:
```
b'\x00DdUdDdUdDxUhDdU|cDdU072DdUhDdUQ'
[0, 0, 0, 0, 0, 0]
[255, 207, 0, 0, 0, 0]
[255, 207, 0, 0, 0, 0]
b'\x00hDdUdDdUhDdUdDdUhDdUdDdUhDdUhDE'
```

Third Raster
```
b'\x00UdDhUdDlUdDtUdD|kUdDdUdDtUdDxU*'
[255, 207, 0, 0, 0, 0]
[255, 207, 0, 0, 0, 0]
b'\x00hDhUdDxUlD|cUdDlUdDhUdDtUdDhUd\xe9'
...
b'\x00080DdUdD|oUdDdUdD096U|wDdUdDtU\\'
[255, 207, 0, 0, 0, 0]
[255, 207, 0, 0, 0, 0]
b'\x00dD|kUdDhUdDlUdDxUdDdUdDtUdDxUd\x81'
```

And the first raster came out without issue, the second raster came out without issue, and the third raster had notable deficits. Shifting registration to the left two times. The error struck while going in the B direction (both times). This is means the 207 packet in those final two cases *should* have been resent. But The first packet appears to have lost about 312mils of an inch and second missed packet lost 375mils. The first packet's distances are `d h d l d t d y k d d d t d x` = 152. And the second packets distances are `80 96 d d y o d d d y w d d t` = 312. These seem accurate but do not quite match the correct values. Maybe something I am not considering with the packet being extracted from the recognized values.

However it is clear that dropping the packets on any 207 was a problem, never dropping the packet is also a problem. There is the notable difference that the status in the should be dropped packets is 0 (for the CH341 driver. But, pure 207 without any intermediate packets which may indicate that 207 means ERROR and in this case the error was the 0 packet. Whereas right after the send 207 means CRC error and the packet should be resent.

## 204 Single Axis Sequence.

The 204 is also seen in single axis testing.
egv IV2240983G000G001BS0RERzzzvLzzzvRzzzvLzzzvFNSE-$
* Will home
* Run a quick back and forth single axis test.
* Stop.
* But, the exit code is 204 for ending the wait.
* If you keep waiting this becomes 236
* If you send IPP$ this will respond with a 207 state.


# Hardware

The USB plug on the board is connected directly to the CH341A or CH341B communications chip (pins 9-12) configured in parallel port (EPP19) mode. The CH341A/B chip's parallel port is connected to pins 30-37 of the 44 pin LHY730318 microcontroller; running a MCS-51 instruction sets using an 8051 processor. The small chip near the LHY730318 is a simple HC14 schmitt trigger. The Active port (which may or may not be populated with header pins) is from top to bottom: GND, Tx (micro pin 7), Rx (micro pin 5), and +5V (verified on recent M2:9 board from 2019).

It is believed all the usb communications deal directly with the CH341A/B and the data is merely sent to the processor to perform the microcontroller's programmed actions.

The 8051 processor has 128 bytes of memory. With 30 byte packets this is enough to send 4.2667 packets of data. Which is why it properly bottoms out during a raster. If high in pixel changes and moving quite quickly. It can bottom out before it getting new data.

LHY Chip Pin Connections:
Pin 1 - 
Pin 2 - 
Pin 1 - 
Pin 2 - 
Pin 1 - 
Pin 2 - 
Pin 1 - 
Pin 2 - 
--- 
```
Bit7~Bit0<==>D7-D0 
 * Bit8<==>ERR#    Bit9<==>PEMP    Bit10<==>INT#    Bit11<==>SLCT    Bit13<==>WAIT#    Bit14<==>DATAS#/READ#    Bit15<==>ADDRS#/ADDR/ALE
 The pins below can only be used in output mode:
 Bit16<==>RESET#    Bit17<==>WRITE#    Bit18<==>SCL    Bit29<==>SDA
```
```
BOOL CH34xSetOutput( ULONG	iEnable, ULONG iSetDirOut, ULONG iSetDataOut)
{
	ULONG mLength;
	UCHAR mBuffer[32];
	mBuffer[0] = CH341A_CMD_SET_OUTPUT;
	mBuffer[1] = 0x6A;
	mBuffer[2] = (UCHAR)( iEnable & 0x1F );
	mBuffer[3] = (UCHAR)( iSetDataOut >> 8 & 0xEF );
	mBuffer[4] = (UCHAR)( iSetDirOut >> 8 & 0xEF | 0x10 );
	mBuffer[5] = (UCHAR)( iSetDataOut & 0xFF );
	mBuffer[6] = (UCHAR)( iSetDirOut & 0xFF );
	mBuffer[7] = (UCHAR)( iSetDataOut >> 16 & 0x0F );
	mBuffer[8] = 0;
	mBuffer[9] = 0;
	mBuffer[10] = 0;
	mLength = 11;
	if ( CH34xWriteData(  mBuffer, &mLength ) ) {  //Write Data 
		if ( mLength >= 8 ) return( true);
	}
	return( false);
}
```

```
#define		CH341A_CMD_SET_OUTPUT	0xA1		// Set Para Output
#define		CH341A_CMD_IO_ADDR		0xA2		//MEM Addr write/read
#define		CH341A_CMD_PRINT_OUT	0xA3		// Print output
#define		CH341A_CMD_PWM_OUT		0xA4		// PWM Out Command
#define		CH341A_CMD_SHORT_PKT	0xA5		//short package
#define		CH341A_CMD_SPI_STREAM	0xA8		//SPI Interface Command
//#define		mCH341A_CMD_SIO_STREAM	0xA9		
#define		CH341A_CMD_I2C_STREAM	0xAA		// I2C Interface Command
#define		CH341A_CMD_UIO_STREAM	0xAB		// UIO Interface Command
#define		CH341A_CMD_PIO_STREAM	0xAE		// PIO Interface Command

#define		CH341_PARA_MODE_EPP	0x00		
#define		CH341_PARA_MODE_EPP17	0x00		
#define		CH341_PARA_MODE_EPP19	0x01		
#define		CH341_PARA_MODE_MEM	0x02		
#define		CH341_PARA_MODE_ECP	0x03	

#define		StateBitERR			0x00000100	
#define		StateBitPEMP			0x00000200
#define		StateBitINT			0x00000400	
#define		StateBitSLCT			0x00000800	
#define		StateBitWAIT			0x00002000
#define		StateBitDATAS			0x00004000	
#define		StateBitADDRS			0x00008000	
#define		StateBitRESET			0x00010000	
#define		StateBitWRITE			0x00020000	
#define		StateBitSCL			0x00400000	
#define		StateBitSDA			0x00800000	
```

Good resource to understand the CH341 chip.
https://www.onetransistor.eu/2017/09/ch341a-usb-i2c-programming.html

# Packet Unknown Sniffed.

Dimsum labs did some analysis, of their packet sniffed logs there are two exchanges that are unknown.

https://github.com/dimsumlabs/lasercutter/tree/master/protocol
```
0000   80 fd c6 22 04 88 ff ff 53 03 02 07 01 00 2d 00   ..."....S.....-.
0010   2a c8 89 58 00 00 00 00 f5 c2 0a 00 8d ff ff ff   *..X............
0020   22 00 00 00 22 00 00 00 00 00 00 00 00 00 00 00   "..."...........
0030   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0040   a6 00 41 a4 86 67 f9 8a 78 f9 f1 79 7d 44 f6 34   ..A..g..x..y}D.4
0050   13 0e a1 46 46 46 46 46 46 46 46 46 46 46 46 46   ...FFFFFFFFFFFFF
0060   a6 25                                             .%
```

The A6 at 0x40 means that this is sent via the EPP to the LHYchip. It still takes the zero, and send A followed by non-ascii. Filling the packet with F then the CRC. But, this isn't lhymicro-gl, since it clearly isn't ascii. It's clearly bulk-out to the device though. This likely makes the thing it said 'A' a4 86 67 f9 8a 78 f9 f1 79 7d 44 f6 34 13 0e a1 which after the A is 16 bytes.

And again:
```
0000   c0 00 ee 26 04 88 ff ff 53 03 02 07 01 00 2d 00   ...&....S.....-.
0010   29 c8 89 58 00 00 00 00 f9 6e 0d 00 8d ff ff ff   )..X.....n......
0020   22 00 00 00 22 00 00 00 00 00 00 00 00 00 00 00   "..."...........
0030   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0040   a6 00 41 4b 30 46 46 46 46 46 46 46 46 46 46 46   ..AK0FFFFFFFFFFF
0050   46 46 46 46 46 46 46 46 46 46 46 46 46 46 46 46   FFFFFFFFFFFFFFFF
0060   a6 a4                                             ..
```

Sent the "AK0" like it was a command the device would know. Here K0 might be a command. Or it could be a different A command being executed on 4b 30.


# Stepper Motor Drivers

The M2 Nano board is equipped with a pair of Allegro A4988 stepper motor driver chips. The chips are configured in 1/16th step mode by holding MS1, MS2, and MS3 (A4988 pins 9, 10, 11) high from a combined trace on the PCB. Both X and Y axis drivers share the same 0.70 Volt REF input trace on the circuit board (routed to A4988 pins 17); but the individual chips are connected to different value sense resistors. The X axis uses R470 (0.47 Ω), and Y axis R360 (0.36 Ω) sense resistors. That drives the X axis motor at 0.44A per phase, and the Y axis motor 0.33A per phase.

# EX+/- Peripherals Switch

The EX+ and EX- pins can be connected to a transistor to control an electro-mechanical relay; there is very little current available. More conveniently, it can directly drive a solid-state relay (SSR). This can be used to turn on and off external devices automatically when the laser is running a job. The EX port is turned back off again about 8 seconds after the job completes. The SSR-40DA has been tested to work, and is an inexpensive plug and play solution. 

# TL / GND Laser Control

External control of the laser. Pulling the TL pin down will fire the laser. Beware; placing a load on the TL pin also has this effect. The TL pin is normally high, ~4.68 Volts on the tested board. 

The TL pin can also be viewed as an output. When the laser is firing via USB control, or the test switch; the normally high TL pin goes low. While pulsed with MeerK40t's PPI feature, the voltages will read in-between the high measurement (~4.68 Volts), and about 0.10 Volts for the low at 1,000 PPI (full power setting). For example, roughly 2.4 Volts was measured at 500 PPI. 