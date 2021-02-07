The idea behind PPI is that you do a bresenham like error carry algorithm. Usually you go some direction and have a 1 or 0 for whether you fire the pixel or not. The PPI applies a different meaning to the 1 and 0.

> 1 means you add the value of PPI to the pulse_value.
> 0 means you do not.
> If the value of the pulse_value is greater than 1000.
>      You laser that mil of distance (that dot, that pixel)
>      Subtract 1000 from pulse_value.

That’s basically the entire algorithm. It could be added to Whisperer in pretty short order too. When PPI is 1000 every 1 gets the laser. When it’s something like 200 every 5th dot gets the laser. Etc. Basically it’s a carry forward value there. The real trick though is PPI is pulses per inch. The stock controller uses 1 mil of distance as its native unit. So when you set the PPI to 200 so 20% power, and draw. It will do 200 (ppi) * 1000 (mils/inch) over that inch, which is 200000, which means that the inch will get 200 laser pulses. That’s exactly the definition of PPI. Pulses per inch. QED.

It’s kinda a game changer, but I also have a number of other advantages. And I’m totally all for hackable software. I want folks to be able to figure out how it works and write their own stuff for it.

Not only that but it actually works for rasters too. I had just done PPI from the vector shapes using PPI to then cycle the fire or not-fire pulses. But, it turns out that if you use the same thing with the rasters you can take the value of the pixel and use that just as easy. And carry the extra pulse data forward. So if it’s a 25% dark section rather than saying that’s actually 0% dark, draw no pixels. It says add 250 to the PPI counter. And when that adds up to more than 1000 fire the pulse. In a 25% dark section, it fires every 4th pulse. But, it can work even in complex data. And can modulate the larger pixel sections even in greyscale. So if the grey is 50% and the step is 4. It should actually do 0101 rather than 1111 or 0000 if the pixel is 80% grey it’ll do 0,1,1,1 (with 200 PPI left over).

This requires more flickering from the laser and consequently can have code sets that can be executed before it reads new data. So even with a full buffer on the software side the controller can start bottoming out and doing a stop and start. Slowing down the raster speed will stall the operations some, and prevent this.