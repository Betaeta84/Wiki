The Kernel API system started in MeerK40t 0.3.0, this is how the internal bits of MeerK40t run and how you can write your own code for MeerK40t either for personal use or general consumption. The Kernel is the glue that holds these parts together so they can operate together quickly and modularly.

See discussion: [#107](https://github.com/meerk40t/meerk40t/issues/107)

# Modules
The core in MeerK40t is a Module. These are registered with:

`kernel.add_module('module_name', module)`

There are several default modules that are just easy and common and should be included and are located in `DefaultModules.py` for example:

`kernel.add_module('SVGLoader', SVGLoader())` will add in svg loading capabilities to the Kernel. When a module is added it sets that module  for the given name, and calls the function `initialize(kernel)` with the kernel. Modules should have an initialize() function that registers the rest of the capabilities of that module. In the case of SVGLoader it calls:

`kernel.add_loader("SVGLoader", self)`

Which adds itself as a loader. See Loaders for more information.

It also calls int settings called `bed_width` and `bed_height` with default values of 320 and 220 respectively. These settings are use to scale the SVG document to the scene size when width is given as a percent, and it needs this information to do that, so it registers those. See Settings for more information.

When kernel.shutdown() is called all registered modules have their `stop()` function called assuming those functions exists.

kernel.boot(): Boot the scheduler and start sending signals.
kernel.shutdown(): Stop the scheduler, shutdown all the modules, wait for the threads to die.
kernel.elements: Stores the default LaserNode data all modules are expected to be working with.
# Scheduler
The Kernel runs a Scheduler to run particular tasks at particular times. Which will run various events at given intervals or particular times as needed. Modules can often forego their own threads and use the scheduler.

# Threads

Threads are registered with:
`kernel.add_thread('thread_name', thread)`

Currently they are only important in that the shutdown of the Kernel will be head until no thread is alive. If a thread exists, modules are asked to have them peacefully die when stop() is called. The shutdown will then wait for those threads to die.


# Settings

kernel.setting(type, 'setting_name', default_value)

Settings inside MeerK40t are persistent if two criteria are met. The settings is a type of `bool`, `int`, `float`, or `str` and the setting does not begin with a `_`. For example the K40Controller has a registered setting of `_device_log` this is which contains the current USB log. However, since it starts with `_` this will not persist. The K40Controller also has a setting of `packet_count` which is an integer. Since it meets both of these criteria it will persist.

The persistence in the Kernel is because it's given a config object. MeerK40t is intended to be agnostic about what this is but it's always a wx.Config object currently. When this config is set it loads all the persistent settings and saves them all on shutdown. 

When a setting is set, it will be attached to the kernel so if the K40Controller is loaded then the `kernel.packet_count` is the place where that data is stored. Any module can access this data. However, since modules are agnostic as to the what data exists, any module wishing to access that data is advised to *also* register the setting: `kernel.setting(int, 'packet_count', 0)` Then if the module doesn't exist, then it will still work fine.

# Controls
kernel.add_control(name, function)

This simply exposes a callable function for the Kernel. For example:

`kernel.execute('K40-Pause')`

Called from anywhere will have the K40Controller pause, and stop sending data.

# Signals
Information updates are passed through signals to allow GUI updates anywhere. These signals are sent by a registered scheduler job that will call all registered listeners about any updated data information. This allows modules to communicate and seem responsive even between different windows and different processes etc. Modules are advised to use signals about realtime data updates or to make interactions between different modules happen. These signals for wxPython-MeerK40t are run in the gui thread. This is because `kernel.run_later` is set to `wx.CallAfter`.

kernel.listen(signal, listener_function)
kernel.unlisten(signal, listener_function)
kernel.signal(signal, value)

# Loaders
kernel.add_loader()

Loaders are required to have a few functions:
load_types(): This is a generator which gives three values: str: Description, tuple: Extensions, str: Mimetype.

This is intended to give the description of what this loads, the extensions those files can be found under, and the mimetype of that type of file.

load(pathname, [group]): The path of the thing needing to be loaded. And optional the LaserNode group this should be added to.

Loaders are expected to append the data of that file path to the kernel.elements value.

# Savers
kernel.add_saver()

Required saver functions are:

save_types(): This is the generator which gives three values: str: Description, str: Extension, str: Mimetype.

Unlike the loaders the second item is `Extension` and is intended to give 1 extension value, the one we should be be use.


# Windows (Speculative) 

kernel.add_window()
kernel.close_old_window()

Since the Kernel is agnostic about the type of GUI it's using the functionality here is minor. On close_old_window it calls `.Close()` on the window.

# Others

There are stub items that aren't actually used anywhere:

effects: Effects convert LaserNode data into different LaserNode data.
The idea here was to mimic the way inkscape extensions current work and provide a general place to put things that modify LaserNode data in useful ways. So you could add a module that would register an effect that would be called in various places to rework that data. 

operations: Operations are functions that can be arbitrarily added to spoolers.
The idea here would be a sort of spooler function that somebody might want to write.

Notably these 'others' are a bit vague now and unlike the other things are not actually a part of the kernel, just ideas about the kernel.

# Backends
* Some of these will be added in 0.4.0
* Most of these don't yet exist.

There is a need for a modular backend to support multiple K40 devices in the same instance of MeerK40t but also, to support alternative backends even those made out of similar pieces. For example to connect through the LibUsb Method to the K40 device, we need to have a spooler, interpreted by a `LhymicroInterpreter`, and sent to a `LibUsb_CH341` pipe. However, if we wanted to support, Moshiboards, we'd have to connect our spooler to a `MoshiInterpreter` since the code generated is different, however, because Moshiboards also use the same CH341->Microcontroller setup as Lhystudio boards we could still use the same `LibUSB_CH341` pipe. However, what if we don't want to switch out our Windows drivers for LibUSB? But, rather use the `CH341DLL.dll` library to control our laser? In that case we'd connect our spooler to the `LhymicroInterpreter` and use the `Windll_CH341 pipe`. And what if we're not sure? Then we connect to the `Auto_CH341` pipe which automatically selects the driver that works.

We can likewise could add interpreters that will write gcode commands, or pipes that save the data to a file, or anything else. The main point is these are the primary things we need to go from `LaserCommands` to Lasering.

## Job
A job is any set of generic commands to be carried out by the laser or system in general. These are generator functions which yield a series of LaserCommands and operators.

## Spooler
The spooler objects take a queue of jobs and processes them all in a thread. This converts, the Jobs in the spooler's queue into single actionable commands. These are then given to an interpreter for processing. The default spooler should be sufficent for 

## Interpreter
The interpreters accept the various LaserCommands and store states and create code from those commands in a language agnostic fashion. For the Stock Controller this is the `LhymicroInterpreter` which converts commands in to Lhymicro-GL code.

## Pipe
The pipes are destination agnostic data channels for bytes of data. For the Stock Controller this is the `LibUsb_CH341` pipe which sends the data to the CH341 via the LibUSB driver.  

