The USB connector on the board is hooked the CH341A or CH341B communications chip configured in parallel port (EPP19) mode. The CH341A/B chip's parallel port is connected to pins 30-37 of the 44 pin LHY730318 chip; running a MCS-51 instruction sets using an 8051 processor. 

It is believed all the usb communications deal directly with the CH341A/B and the data is merely sent to the processor to perform the microcontroller actions.

The 8051 processor has 128 bytes of memory. With 30 byte packets this is enough to send 4.2667 packets of data. Which is why it properly bottoms out during a raster. If high in pixel changes and moving quite quickly. It can bottom out before it getting new data.


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

