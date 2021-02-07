# Contributing to Meerk40t
Meerk40t is open source, which means that you can contribute to improvements as a way of saying thank you for the benefits you gain from using this program. Obviously if you have Python programming skills improving the Meerk40t code is something that we would love your help with, but even if you don't have these technical skills you can still help by improving the documentation or translating the user interface into your own language or undertaking research experiments with your own K40 laser cutter. 

# Research needed
In order to underpin future improvements to Meerk40t we need to gather data about K40s and how they are used.

## Materials Database

We need to have a materials database that can be scaled relative to speed. The idea is that if we have somebody's consistent settings for a wide variety of materials.
* Is it possible to that these can be linearly scaled and work for others?
* If you can run these settings for oak at 3mm thickness, and that's like 40% too weak for somebody else, can they just scale their speeds by 40% and get a bunch of highly functional materials settings?
* Does the amount of photons needed to cut wood scale fairly linearly with the thickness. If I have settings for 2mm material is 3mm thick material just 50% additional photons, like slow down by 1/3?

## Time Estimation.
* How fast are rapid movements? Like in mm/s how fast does the laser head travel to calculate an equal speed?
* How fast are most of the operations done on the M2 Nano? The time guesses currently suck, because the time for other required processes are not known, nor are the speeds of various jogs and delays. How long does a raster really take? Does it scale with scanlines more readily?

## Accel Values.
In the Advanced section of Operations there are Accel values. These number 1/4. It's believed they relate to the starting and stopping of designs.
* What exactly are the effects of the Accel values?
* Are the accel values optimally set? Can higher accel values give better results.
* Accels values help trigger accel and decel jerks, what exactly triggers these within a design. It is noted that using a PPI setting less than 1000 will trigger more of these since the laser turns off more. 

## M2 Nano Research
Basic research on the Lhymicro board. There's some stuff that just isn't known. 
* How does the Chinese software react to a signal 207 error?
* What are the specifics of the S0 commands (this is now partially known)? What exactly is moving back and forth like that repeatedly trying to do in the original software and should it be implemented in MeerK40t?
* How far does a Jog without a counter switch move and in what direction? The initial values set in the N<X><Y>S1E code for board sets some values that move the laser head if triggered will affect how the Jerk between N<move>SE commands go, setting the value RN<L-move>SE will avoid the laser head moving location. How far does the laser head move? And can we just account for it?
* What does the N<move>SE code do in rasters?
* Can you switch an horizontal raster to a vertical raster without exiting out of the mode? How does the board react? Can these reactions be exactly quantified? Does it need to set the 'vertical major' setting to avoid this?
* What do M commands do in a raster? Is there a use to them?
* Can the lhychips be capped or code otherwise accessed?

## MeerBoard
Work on Project MeerBoard.
* Is there a commonly available cheap board that could perfectly clone the M2 Nano functionality to the degree that local software could not tell the difference?
* Is the project overall reasonable? Could we make knockoff streaming bytecode laserboards?
* Could this project also be expanded to include other useful features?

## Moshi
Work on Project Moshi
* Does the random seeming command code cycles have a real function or does the Moshiboard accept any code value?
* Is there enough market share for Project Moshi to make it worthwhile as a thing?

## RuidaEmulator
The Ruida Emulator allows RDWorks software to be used to drive the K40 laser. To help improve our prototype, we need to understand the following:
* <Needs finishing to open lines of research>

## Console Completion
The console is intended to be a complete solution. What processes can't be done with the console? What are the things people may want done that can't be done? (Raise issues for these)

# Wiki
This Wiki is (for the moment at least) the home of our documentation. The user part of the documentation is still a work in progress and any help you can provide would be welcome.

# Language Translations
Meerk40t's native language is English. TO provide the UI in other languages, a translation is required. 