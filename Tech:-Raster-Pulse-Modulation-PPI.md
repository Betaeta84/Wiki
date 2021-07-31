## Background
Whilst the K40 has a hardware panel control for selecting the laser beam strength, once this is set, the laser is always fired at this strength. But often we will want to fire the laser at different strengths at different points of our burn e.g. to create lighter or darker colouration or to burn way more or less material.

Raster Pulse Modulation is a means of doing this by turning the laser on and off many e.g. hundreds or thousands of times per second. For example, if you are rastering an image at (say) 400mm/s (a speed that many or most K40s are capable of without losing position), the K40 could be turning the laser on or off over 15,000 times per second.

And because the laser dot is 3-5mils or more in diameter, if you turn the laser on and off every dot then these overlap and it is equivalent to the laser being 50% weaker. By varying the PPI between (say) 200 and 1000 PPI, you can effectively create a variable laser strength between 20% & 100% of what you set on the front panel.

## The PPI Basics (for Vectors)
The idea behind PPI is that you do a [Bresenham](./Tech:-Zingl-Bresenham-Curve-Plotting)-like error-carry algorithm. Usually you go some direction and have a 1 or 0 for whether you fire the pixel or not. The PPI applies a different meaning to the 1 and 0.

* 1 means you add the value of PPI to the pulse_value.
* 0 means you do not.
* If the value of the pulse_value is greater than 1000...
  1. You have the laser on for that mil of distance (that dot, that pixel)
  2. You subtract 1000 from pulse_value.

That’s basically the entire algorithm. When PPI is 1000 every 1 gets the laser; when it’s something like 200 every 5th dot gets the laser; etc. Basically it’s a carry forward value there. The real trick though is PPI is pulses per inch. The stock controller uses 1 mil of distance as its native step size and there are 1000 steps per inch, so when you set the PPI to 200 it will do 200 (ppi) * 1000 (mils/inch) over that inch, which is 200,000, which means that the inch will get 200 laser pulses. That’s exactly the definition of PPI - Pulses per inch. QED. And because 200PPI has 1 overlapping pixel with the laser on, and 4 overlapping pixels with the laser off, and 200 PPI is equivalent to 20% of the K40 front-panel power.

## Rasters too
Not only that but it actually works for rasters too. It turns out that if you use the same thing with the rasters you can take the grey-scale value of the pixel and use that for PPI. And as before carry any remaining pulse data forward. So if it’s a 25% dark section of the raster, rather than saying that's below 50% so we will treat it as 0% dark and have the laser off, instead it says add 250 to the PPI counter. And when that adds up to more than 1,000 fire the pulse. In a 25% dark section, it fires every 4th pulse. 

When you are rastering with a step of 1, then each pixel in the image accounts for exactly one mil in the X-axis, but when you are rastering with e.g. a step of 4, so it only lasers lines every 4 mil apart and treats each x-axis pixel as if it were 4 pixels. In this case, rather than calculating each pixel in the image as either on or off and then doing that for 4 mils, instead we can count that pixel 4 times instead and vary the laser for each of those 4 times. So if the darkness is 50% and the step is 4, then instead of doing 4-pixels with the laser on and then 4-pixels off (1111,0000), it is able to count each pixel 4 times and turn the laser on and off every other pixel instead (1010,1010) for a better quality result. Similarly if the pixel is 80% dark instead of 0000,1111,1111,1111 with 200PPI left over we will do 0111,1011,1101,1110 with 200PPI left over.

## Burn pauses
As you will discover if you read about [the controller protocols](./Tech:-Lhymicro-Control-Protocols), the amount of data that needs to be sent to the K40 is in part dependent on the frequency with which you turn the laser on and off. Because the PPI rastering creates more flickering from the laser, consequently the amount of data that needs to be sent to the K40 can be significantly larger and it is then possible to be unable to send the data fast enough for a high-speed raster. The symptoms of this are the head rapidly slowing down, back tracking and then starting again. 

500 PPI is the most verbose frequency. Anything higher or lower than this will be less verbose. So using a higher PPI and lower K40 front panel power, or a lower PPI and a higher K40 front panel power can both potentially help - or alternatively use a slower speed and a lower front-panel power.

# To Do
The above explanation is unclear how darkness values and Operation property PPIs combine. So in addition to the description above of how steps and grey-scale pulse modulation combine, it might be helpful to have a description of how the grey-scale pulse modulation and Operation PPI combine, and indeed how all three combine.

---
### Authors
The MeerK40t team is grateful for the help from @tatarize in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃