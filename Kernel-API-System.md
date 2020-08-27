The Kernel API system started in MeerK40t 0.3.0, this is how the internal bits of MeerK40t run and how you can write your own code for MeerK40t either for personal use or general consumption. The Kernel is the glue that holds these parts together so they can operate together quickly and modularly.

Some of this is speculative and not actively how it works due to legacy code.

# Registration
Objects are registered in the kernel. And all objects attempt to call sub_register(kernel) to register additional elements, as a static call on the class. This builds the registered tree of objects that are available. These are references to the classes, small bits of data, and static information.

Any interface using the kernel should register at least one object which it turn would register the rest of the tree. The directories in the registration are based on the type of object they are. Pipes should be registered in `pipe` and interpreters in `interpreter`, modules in `module`.

# Devices
A device is a specific for a particular kind of laser or backend. These should always have a spooler, interpreters, controllers and sometimes emulators and pipes. These are registered in the Kernel under 'device'. These will most importantly store the preferences for that particular device.

# Preferences
A preferences is a class persistent settings and storage. The Kernel is a type of Preferences. Instances of devices are also types of preferences. These are derived from the kernel preference, when the device is loaded. You can also derive the preferences from a would-be device to check what they are, the kernel does this for the `autoboot` flag.

Preferences are expected to be checked for existence prior to use. The `setting` command provides a setting_type, a key, and a default value. Once called `.'key'` is the preferred access method. For any preferences object where we need a preference, it is required to call the `setting()` for that information during initialization. If there is no persistence object backing the preference this will simply assign that setting to the default, but if there is one, we'll have the correct usage. The `flush` command should push the current settings to persistent storage. This is called, by default, during the device shutdown. However if a Preferences is not a device, it won't be shutdown and the settings will not flush().

Some code like Camera and Keymap have their own derived Preferences objects without being associated with a device.

# Scheduler
A scheduler is functionality attached to devices which allows for jobs to be scheduled. It launches a thread in the kernel which which iterates through the different scheduled jobs, and running them when required.

# Jobs
Currently Modules are Jobs and call schedule() and unschedule() but this will largely be changed. Later so that modules that inherit from Job can be scheduled(). 

# Signaler
The signaler attaches to the Kernel and Devices. It schedules itself, and provides functionality for `listen()`, `unlisten()` and `signal()`. This allows non-duplicating signals to sent on a changes so other GUI elements know they should update, if they listened for a specific signal. The signaler does not force every message to be delivered, only the last message. And attaching a listener will get the last message for that signal if it exists. Using these for GUI fields will then give the last message sent even if the GUI window was launched after that signal ended.

Devices will often delegate signals and listens to the kernel with a special prefix of the UID of the loaded kernel. You may then listen() the the signal on a particular device, and will not receive crosstalked information if multiple copies of that same device are instanced.

# Channels
The channels are an aspect of the Kernel and Devices. These allow channels to be opened and watched. These will convey every message sent to any watchers observing that channel. There does not need to be an open channel for that channel to be watched. Or a watcher for that channel to be opened. These provide streams of information while being agnostic as to where the information will end up. A channel may be assigned a greet for when the channel starts as well as a buffer which will be sent to any dialog when connecting to a particular channel. For example, the USB channel is opened with a buffer, so any watchers connecting, such as the USBConnect window will be provided that buffer, even if that data happened before the channel was opened.

# Modules
Modules are opened classes. They should be registered in the `module` directory in the kernel. These are opened and attached to devices. Sometimes they are registered in other specialty directories like `window` if they are GUI windows, and of little use otherwise. These are opened using the `open()` function on devices and kernels. If this module is already opened. The opened module is returned.

---

This should run on a pathed scheme where open(object, location) will open a particular object and assign it to that particular location. `open('window/CameraInterface', 'camera/0')` The camera/0 class will be given a derived device, with settings at this location.

A device is not a thing but a place. / is the kernel, where as /1 is the first device or /camera is the camera root. /camera/0 is camera0. These open paths can be created on the fly. They have persistent settings via a preference instance and they have. These launch instances at /1/spooler and /1/pipe.

We have settings. So a device object will contain bool, int, str, and floats. But, also things like spoolers, and pipes.

kernel = Kernel()
(default register, core functions: scheduler, signaler, elemental, channels)
kernel.open('scheduler')
kernel.open('signaler')
kernel.open('elemental')

kernel.register('device/Lhystudios', LhystudiosDevice)
kernel.register('load/SVGLoader', SVGLoader)
kernel.register('load/ImageLoader', ImageLoader)
kernel.register('save/SVGWriter', SVGWriter)

kernel.register('module/wxMeerK40t', wxMeerK40t)
kernel.open('module/wxMeerK40t')

place = kernel.derive('1')

camera = kernel.derive('camera')
camera0 = camera.derive('0')
camera0.open('window/CameraInterface')

place.open('device/Lhystudios')
- Opening the Lhystudios device forces it to register locally .spooler and .pipe and .interpreter.
place.open('module/channels')
- Channels opened here gives it `channel_open()` and `watch_channel()` functions.
place.open('module/signals')
- Signals opened here will bind .signal() and .listen() and .unlisten() to the root signaler.




# Lifecycle
In any useful API system the lifecycle of the objects is needed to utilize that system api. The Kernel API is the core element this has initialization `boot()` where the scheduler is started and `shutdown()` where all objects are shutdown and deregistered. Anything using a MeerK40t kernel should call `boot()` when the objects should start interacting and signals should start sending. And `shutdown()` should be called to ensure everything terminates correctly.

* Boot: Start the scheduler, signals, etc
* Config: Register the persistent settings object
* Activate_Device: Set the device object active. (None permitted)
* Shutdown: Shutdown the device, saving persistent settings, calling shutdown on all modules and waiting for all threads to stop.


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

Unlike the loaders the second item is `Extension` and is intended to give 1 extension value, the one we should use.


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

