Tatarize, first author.

Early on after my brother bought a K40 laser, the best and first open source solution was K40 Whisperer which was and still is quite nice for it's simplicity. I was not very enamored of some of the code in it, as I wanted to write some base-level code for the M2 Nano with python and it didn't support the basic plotter like functions that I would expect of a library but it had enough basic understanding and work that I started my first major project, tearing apart the core part of the whisperer.

This started the [K40Nano Project](https://github.com/K40Nano/K40Nano). The idea was simple, if you had something you wanted your laser to do it would work as a very bare bones plotter like API. You could then write basic python code and it would plot things. I tore apart all the code, removing strands of spaghetti from unneeded user interface and I basically got the needed code down to a single file. NanoUSB. This basically just found the device and connected to it using pyusb. And nearly every line was taken from the example code given by pyusb as how to do that. There was also a really weird note in the code that basically said "I have not been able to figure out the relationship between the speeds in the first column and the codes in the second columns below.", and that was how Whisperer was calculating the speeds, it figured some numbers that sort of implied some speeds and was trying to do some linear interpolation between them. This would often do rather weird things.

So I set off on my first side quest, and using some computer level macros and the original Chinese software I dumped every speed code it could generate. I then figured out that it wasn't one big number but two different three digit numbers between 000 and 255 (where interpolating might somewhat work but would be very random). I figured out the encoding then did a bunch of math and a few experiments and I ended up with tests that could 1:1 perfectly produce the codes that were used on the original software. Even the ones that were obviously wildly flawed and bugged (some codes would be 16777215 or so for the 3 digit number between 000 and 255). This code was included in [K40 Whisperer 0.31](https://github.com/jkramarz/K40-Whisperer/commit/cadb4830e5d49139c2c64f4df4601bc78962aa42) (after providing proof it worked with an extremely extensive test suite).

So, the oldest file in all of MeerK40t is `LaserSpeed.py` it's mostly the same code as the original code, and is about the same code you'd find in Whisperer today, it predates the entire MeerK40t project. I licensed it initially under MIT which I preferred, but all of Whisperer was licensed under GPL, and I built K40Nano basically by carving chunks off Whisperer.  I wrote a few tools under MIT at [K40Tools](https://github.com/K40Nano/K40Tools) this was mostly a bunch of basic scripts that used K40Nano's plotting interface. But, this too seemed a bit limited in the licensing department. Especially since I basically deleted most of whisperer to make K40Nano and the only things that survived were pyusb example code.

So I decided to set out on my own and write my bit of code from scratch to license how I wanted. So I wrote... [https://github.com/K40Nano/Jk40](JK40). I found a nice bit of software, [LibLaserCut](https://github.com/t-oster/LibLaserCut) (Part if Visicut) which was java based and started contributing code. And again, something sort of stuck in my craw. IT was build around a lot of gcode lasers but with a lot of potential backends but did not actually do the regular work of sending realtime commands (like pause, and resume, and estop). It was much more for lasers that would get sent a bunch of commands and then do that stuff with those lasers. But, it's job was first and last, just sending the file. Also, java had some issues with windows and was kind of a jerk with regard to directly talking with the USB.

# MeerK40t

About July 20th, 2019 I started the meerk40t github.

The first version was released Sep 23, 2019.

Sep 23, 2019. 0.1 --- Completed Phase 0. We could load something and send it to the laser, and it sort of worked. "Before perfect it should be competent enough to let reasonably competent users test some stuff." See issue #20 ( https://github.com/meerk40t/meerk40t/issues/20 )

Around this time laser gods did a write-up ( https://lasergods.com/meerk40t-stock-k40-software/ ).
https://www.youtube.com/watch?v=-d9hD4O9KGk

# SVGElements

After that it basically went dead until Dec. 2019. Largely because I realized there was a real need for robust SVG loading. And the couple things of feedback I got was only going to get worse unless svg loading was treated with respect it required.

Dec 26, 2019. 0.2 --- Added in SVGElements. This was a large amount of work between October and December of 2019, a general census showed that the typical methods of loading things weren't very good and a lot of very intricate work mapping out a lot of SVG was needed in order to correctly load svgs for use in plotter based devices.

Shortly after this Inpain/Paiin came aboard late Dec 2019. He started pointing out issues with other OSes, and started releasing some of these pre-built setups by Jan. Channel #meerk40t on Freenode was opened Jan 17th 2020. Tiger joined nearly a year later on  Jan 8th 2021. Sophist started raising Issues by Feb 7th, 2021.

Jan 170.3 --- Added the kernel with some understandings of lifecycle, added in things like signals so that windows could be allowed to update fairly well.

0.4 --- Was never published but was part of the major breaking changes included in 0.5.

Feb 18, 2020 - 0.5 ---- This largely was due to the addition of dual drivers, where you could potentially point the backend towards several different outputs. At this time the typical relationship, was that you could stack drivers with a spooler, controller, and output. The idea was you could redirect the output to like a file or whatever else you needed.

Jun 23, 2020, 0.6.0 --- Major revision added in Kernel / Device as basically both able to boot where the kernel would control the booting of the different Devices. The devices could run on their own. And you could in theory have multiple devices though it would almost always require two different main windows. This basically had devices like their own mini-kernels.
Added in the scene and widget system.
