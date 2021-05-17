
# K40 Nano Board Operations

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
