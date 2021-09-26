If you are going to write code either for your own purposes or for the community at large or help in [[other ways|https://github.com/meerk40t/meerk40t/wiki/Tech:-Help-wanted]], you may need to understand at least some of the underlying architecture. 

First the the wiki is not the primary source for this information, the primary source is the source primarily, not simply comments in the source code but also the readmes that are part of most of the directories within the source code.

Secondly assuming we have done our jobs well there should be a community of people involved with this project, currently the considerable discussion goes on in the irc channel [irc.libera.chat #MeerK40t](irc://irc.libera.chat#meerk40t). 

# Kernel

The Kernel serves as the central hub of communication between different aspects of the system. These are mapped to particular contexts that have path locations within the kernel. The contexts can have modules opened and modifiers applied to them.

* manages overall program [[lifecycle processes|Tech:-Lifecycle]].
* a central dictionary of [[registered objects|Tech:-Registered-Objects]].
* stores [[persistence values|Tech: Persistence Settings]] at a given [[contexts|Tech:-Contexts]].
* [[context path|Tech: Contexts]] locations for partitioning of information
* provides for [[signals, listeners|Tech: Signals]], partitioned at the context level
* manages [[thread interactions|Tech: Threading]] and [[shutdown|Tech:-Shutdown]]
* provides a [[scheduler, jobs for the scheduler|Tech:-Scheduler]]
* general data [[channels|Tech:-Channels]]
* a command [[console, registered commands|Tech:-Console]]
* execution and operation of [[modules|Tech:-Modules]], [[modifiers|Tech:-Modifiers]], [[plugins|Tech:-Plugins]]
* [[translation information and functionality|Tech:-Translations]]
* run_later operations for moving operations to a gui or other main thread
* [[plugins|Tech:-Plugins]] api
* hooks for [[MeerK40t Specific api elements|Tech:-MeerK40t-Specific-API]]

 There is a minor goal to make the kernel independent of the MeerK40t code. It serves as the central hub for MeerK40t but it *should* be able to serve as the central hub for any other program.

# Hardware
Understanding the hardware we are trying to drive is a very important step in understanding how we must do what we want to do.

## Lhystudios M2 Nano
* [[M2 Nano Communication Protocols|Tech:-Lhymicro-Control-Protocols]]
* [[M2 Nano Hardware Information|Tech:-Lhymicro-M2-Nano-Hardware]]
* [[M2 Nano Speed Research|Tech:-Physical-Speed-Measurements]]

# GUI
The core Meerk40t gui is called wxMeerK40t and is written in wxPython. This is designed specifically to be interchangeable or able to be omitted. Specifically we use AUI within wxPython. There's considerable amounts of information on how these work so familiarizing yourself on them does not need to be covered here. 

# Algorithms and Methods
Some algorithms being used by MeerK40t are extremely useful but not well known.

* [[Zingl-Bresenham Curve Plotting Algorithm|Tech:-Zingl-Bresenham-Curve-Plotting]]
* [[Internal Laser Commands|Tech: Internal Laser Commands]]

# Extras
Other articles have been written from time to time and are not specifically covered or needed by the rest of this technical documentation.
* [[What exactly is the laserhead animating?|Tech:-What-exactly-is-the-laserhead-animation-animating?]]