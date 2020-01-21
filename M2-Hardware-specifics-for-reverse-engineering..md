The board is hooks the CH341A usb communications chip in parallel port mode to the LHY730318 chip running a MCS-51 instruction sets using an 8051 processor.

It is believed all the usb communications deal directly with the CH341A and the data is merely sent to the processor to perform the microcontroller actions.

The 8051 processor has 128 bytes of memory. With 30 byte packets this is enough to send 4.2667 packets of data. Which is why it properly bottoms out during a raster. If high in pixel changes and moving quite quickly. It can bottom out before it getting new data.