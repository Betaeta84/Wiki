
# Pre-MeerK40t

Tatarize, first author.

Early on after my brother bought a K40 laser, the best and first open source solution was K40 Whisperer which was and still is quite nice for it's simplicity. I was not very enamored of some of the code in it, as I wanted to write some base-level code for the M2 Nano with python and it didn't support the basic plotter like functions that I would expect of a library but it had enough basic understanding and work that I started my first major project, tearing apart the core part of the whisperer.

This started the [K40Nano Project](https://github.com/K40Nano/K40Nano). The idea was simple, if you had something you wanted your laser to do it would work as a very bare bones plotter like API. You could then write basic python code and it would plot things. I tore apart all the code, removing strands of spaghetti from unneeded user interface and I basically got the needed code down to a single file. NanoUSB. This basically just found the device and connected to it using pyusb. And nearly every line was taken from the example code given by pyusb as how to do that. There was also a really weird note in the code that basically said "I have not been able to figure out the relationship between the speeds in the first column and the codes in the second columns below.", and that was how Whisperer was calculating the speeds, it figured some numbers that sort of implied some speeds and was trying to do some linear interpolation between them. This would often do rather weird things.

So I set off on my first side quest, and using some computer level macros and the original Chinese software I dumped every speed code it could generate. I then figured out that it wasn't one big number but two different three digit numbers between 000 and 255 (where interpolating might somewhat work but would be very random). I figured out the encoding then did a bunch of math and a few experiments and I ended up with tests that could 1:1 perfectly produce the codes that were used on the original software. Even the ones that were obviously wildly flawed and bugged (some codes would be 16777215 or so for the 3 digit number between 000 and 255). This code was included in [K40 Whisperer 0.31](https://github.com/jkramarz/K40-Whisperer/commit/cadb4830e5d49139c2c64f4df4601bc78962aa42) (after providing proof it worked with an extremely extensive test suite).

So, the oldest file in all of MeerK40t is `LaserSpeed.py` it's mostly the same code as the original code, and is about the same code you'd find in Whisperer today, it predates the entire MeerK40t project. I licensed it initially under MIT which I preferred, but all of Whisperer was licensed under GPL, and I built K40Nano basically by carving chunks off Whisperer.  I wrote a few tools under MIT at [K40Tools](https://github.com/K40Nano/K40Tools) this was mostly a bunch of basic scripts that used K40Nano's plotting interface. But, this too seemed a bit limited in the licensing department. Especially since I basically deleted most of whisperer to make K40Nano and the only things that survived were pyusb example code.

So I decided to set out on my own and write my bit of code from scratch to license how I wanted. So I wrote... [https://github.com/K40Nano/Jk40](JK40). I found a nice bit of software, [LibLaserCut](https://github.com/t-oster/LibLaserCut) (Part if Visicut) which was java based and started contributing code. And again, something sort of stuck in my craw. IT was build around a lot of gcode lasers but with a lot of potential backends but did not actually do the regular work of sending realtime commands (like pause, and resume, and estop). It was much more for lasers that would get sent a bunch of commands and then do that stuff with those lasers. But, it's job was first and last, just sending the file. Also, java had some issues with windows and was kind of a jerk with regard to directly talking with the USB.

# MeerK40t

About July 20th, 2019 Tatarize started the meerk40t github.

The first version was released Sep 23, 2019.

## 0.1 
Sep 23, 2019. 0.1 --- Completed Phase 0. We could load something and send it to the laser, and it sort of worked. "Before perfect it should be competent enough to let reasonably competent users test some stuff." See issue #20 ( https://github.com/meerk40t/meerk40t/issues/20 )

Around this time laser gods did a write-up ( https://lasergods.com/meerk40t-stock-k40-software/ ).
https://www.youtube.com/watch?v=-d9hD4O9KGk

![image](https://github.com/meerk40t/meerk40t/assets/3302478/e30f35a2-e442-47c5-8c40-45fc44170a39)


## SVGElements

After that, the main project basically went dead until Dec. 2019. Largely because I realized there was a real need for robust SVG loading. And the couple things of feedback I got was only going to get worse unless svg loading was treated with respect it required. This largely spawned the first major spinoff of meerk40t.

## 0.2
Dec 26, 2019. 0.2 --- Added in SVGElements. This was a large amount of work between October and December of 2019, a general census showed that the typical methods of loading things weren't very good and a lot of very intricate work mapping out a lot of SVG was needed in order to correctly load svgs for use in plotter based devices.

![image](https://github.com/meerk40t/meerk40t/assets/3302478/7e3dfcec-fc0a-4064-9348-f0e01b64ba68)

## 0.3
Jan 17, 2020, 0.3 --- Added the kernel with some understandings of lifecycle, added in things like signals so that windows could be allowed to update fairly well. It was clear that without a central hub of how things should interact and the required protocols that the project would quickly become unwieldy. Every part of the program would need to know all the other parts of the programs. Unless something was done to mitigate that would-be complexity and deal with each of the parts in a 1:1 fashion.

![image](https://github.com/meerk40t/meerk40t/assets/3302478/b1b95e8c-cfcb-421f-b6d5-ae0b6d4bc1c6)


## 0.4
0.4 --- Was never published but was part of the major breaking changes included in 0.5.

## 0.5
Feb 18, 2020 - 0.5 ---- This largely was due to the addition of dual drivers, where you could potentially point the backend towards several different outputs. At this time the typical relationship, was that you could stack drivers with a spooler, controller, and output. The idea was you could redirect the output to like a file or whatever else you needed. This also added in the CLI mode and let the device be potentially run without a gui. The Kernel was less general and more specific towards driving laser devices. 

![image](https://github.com/meerk40t/meerk40t/assets/3302478/57d50c03-5cbf-4e1d-987b-c1f635ef58c0)


## 0.6

Jun 23, 2020, 0.6.0 --- Major revision added in Kernel / Device as basically both able to boot where the kernel would control the booting of the different Devices. The devices could run on their own. And you could in theory have multiple devices though it would almost always require two different main windows. This basically had devices like their own mini-kernels.
Added in the scene and widget system, also the very basic first use of Camera.

![image](https://github.com/meerk40t/meerk40t/assets/3302478/6a394a26-6d25-4c2f-9723-005864662964)

Later in the series things like keymap, notes, terminal, and rasterwizard were added.

## 0.7

Around 0.6.17 or so the first betas of 0.7.0 were launched. This was one of the biggest rewrites. During this time switching the branches the very reasonable suggest to make the project tree look reasonable was made. Inspirations from vpype lead to camera being spun off into it's own `plugin`. The biggest change for 0.7 was the elimination of the devices within Kernel. This means these devices would not boot and none of them would have their own threads, etc. And lifecycles were added in a much more robust fashion.

![image](https://github.com/meerk40t/meerk40t/assets/3302478/5dfc4fb7-8c48-42de-8b79-7c8d0d20bd25)

First full release:

![image](https://github.com/meerk40t/meerk40t/assets/3302478/6d1f199f-b721-4456-bc4c-3538fc08efc0)

Sophist did an absolutely heroic amount of bug-checking and reporting for this version.

## 0.8

This was either the biggest or second biggest rewrite after 0.7. Both of these required nearly all code to be touched in one way or another.

The biggest change in 0.8 was that Modifiers were eliminated and Services were added. This somewhat mattered since camera was a modifier but due to the switch couldn't be included except by moving it back into the main code. The kernel had a lookup dictionary called `registered` which largely serves as the central repository of things within the kernel. It became clear as more and more drivers were being supported that these drivers had different buttons and realtime commands and these needed to be treated as first order citizens. We couldn't merely adapt some code mostly intended for one laser device to another device. We needed to be able to swap them out basically entirely when we switched devices. This was the genesis of services. They mostly allow the entire lookup to be changed if a Service which is itself a subset of a Context is switched it becomes the `.device` value for all contexts, it also triggers anything listening to the lookup to know that the values have changed. And most importantly it allowed the service to override the `.registered` lookup with their own values. So if your device had a command `estop` and another device had a command `estop` then running `estop` in console would run the command of the `active` device. This works regardless how many of that type of device exist.

Earliest 0.8:
![image](https://github.com/meerk40t/meerk40t/assets/3302478/f32f3015-3224-430c-8659-ab9b22f7b788)

First Full 0.8:
![image](https://github.com/meerk40t/meerk40t/assets/3302478/3878ff05-dd34-48e9-84cd-0b9bd92357ab)


## 0.9

The big change for 0.9 was the use of geomstr as the core geometry privative. And having learned my lessons from 0.8 and 0.7 changes, extreme care was taken to scaffold the change such that things did not break during the change. This moves away from using the SVGElements path commands as default primitives which had been the case since 0.3. The ribbonbar was also changed around this time specifically because, jpirnay and Tatarize had mostly written all the internal code and might as well do the drawing too so as to have even better control to do things like make vertical toolbars for tools. It was considerably more work than it was initially assumed.

![image](https://github.com/meerk40t/meerk40t/assets/3302478/45c3187c-761e-4fa3-b6cb-8b9ee7cefafe)

## 1.0

We'd probably go to 0.10.0 before 1.0.0. I had brought it up after the huge amount of working going to 0.8.0 and everybody seemed opposed to calling things `1.0.0`. It might end up being a soft change when the last of the core functionality is completely established, or after spinning off a number of sub-projects from the current codebase.

# IRC History.

* Paiin started out the #meerk40t channel on the largest and one of the oldest most stable irc networks for free software: freenode. Channel #meerk40t on Freenode was opened Jan 17th 2020, with Tatarize and Paiin being founding members.
* Tiger joined nearly a year later on Jan 8th 2021.
* Sophist started raising Issues by Feb 7th, 2021, and joined the chat May 16, 2021.
* Sophist alerted us that there was some drama with freenode around May 23 or so, and founded #meerk40t on libera.chat on spec.
* Paiin refused to switch over until Freenode fully died.
* Freenode blew up Jun 16, 2021, Tatarize was the last person to say anything on the network, "You're on Fire." on the last server before it finally got destroyed.

# Discord History

* apbarratt founded the Discord server November 18th, 2021. The Libera server was largely abandoned, because unlike the switch to Liberia, Paiin was less of a pain to move, and it had a lot of the features we needed like posting images and keeping chat around without needing to have been on the server when the person typed it.
* apbarratt about immediately resigned to spend more time with his family.
* On March 17th, 2022 Tatarize was given full control over the server.
* Sophist resigned from the main group May 22, 2022 due to perceived biased towards and bullying by Paiin. He still (11/2023) does a fantastic amount of the bug checking and feature suggestions.
* Jpirnay does an absolutely heroic amount of coding going from around May 2022.

# And then something unexpected happened...
jpirnay secondary author

![david](https://github.com/user-attachments/assets/0e5d52fb-b11e-43fb-af2b-088fe86167ae)
MeerK40t is the result of an incredible piece of work by David Olsen aka Tatarize.
He created this program over 4 years allowing users across the world to get the best out of their K40 equipment (and additional lasertypes).

Despite having no risk factors for getting cancer, he developed a tumor on his tongue that metastasized into his lungs before the doctors could stop it and passed away on July 26, 2024.
- He was a mentor, an inspiration and a friend - David you will be missed but you won't be forgotten.
- Please join the fight against cancer and consider donating to one of the many research and charity organisations across the world.

David passed on the baton for the development of MeerK40t to us, so this story, his heritage will continue. We want to use this moment though to encourage people to help and participate in this journey. So if you are interested have a look at [Assisting the project](https://github.com/meerk40t/meerk40t/wiki#help-wanted) and reach out to us - we would be delighted to hear from you.