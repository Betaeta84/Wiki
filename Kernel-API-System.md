The Kernel/Context API system started in MeerK40t 0.3.0, and underwent several major revision on 0.7.0. This is how the internal bits of MeerK40t run, and hold together as a coherent system, and how you can write your own code for MeerK40t either for personal use or general consumption. The Kernel is the glue that holds these parts together so they can operate together quickly and modularly.

# Plugins

Writing a plugin for Meerk40t is easy. See the example plugin https://github.com/meerk40t/meerk40t-example-plugin

The relevant code is an entry point in the `setup.cfg`:

```ini
[options.entry_points]
meerk40t.plugins = Example = example.main:plugin
```

And the `plugin()` function:

```python 
def plugin(kernel):
    @kernel.console_command('example', help="Says Hello World.")
    def example_cmd(command, channel, _, args=tuple(), **kwargs):
        channel(_('Hello World'))
```

The code in the bootstrapping calls plugin() with the kernel for all `meerk40t.plugins` which allows them to register themselves and alter the code as needed. In the example, we use the `kernel.console_command` to register a command called "example" which calls the channel with "Hello World" when installed with pip. The pure python version of MeerK40t will find and execute this plugin, this will add a command that sends "Hello World" to the channel when called. The `_` function is for translation purposes.

# Kernel

The Kernel serves as the central hub of communication between different objects within the system. These are mapped to particular contexts that have locations within the kernel. The contexts can have modules opened and modifiers applied to them. The kernel serves to store the location of registered objects. It also provides access to:

* Contexts: Locations within the kernel space at specific paths. For example, the camera add-on stores camera settings in `camera/0` through `camera/5` and these settings are independent of each other.
* Modifiers: Modifiers provide functionality for a particular context. The assumption is modifiers will alter the context or provide a link to the main code. For example `Spooler` provides .spooler at the given context as place to send cutcode. Modifiers also get called `boot()` when the kernel boots at start-up. These are always called the name of the modifier itself. So `modifier/Spooler` is opened as `modifier/Spooler`
* Modules: Modules are assumed to be allow many copies of the same class. These are opened at the context and stored in the `.opened` dictionary. These are given a name which can be dynamically assigned. They are not expected to change the context at which they are opened. 
* Channels: Channels are dataflows within the kernel. For example the LhystudiosController accepts data through a channel. Likewise you send data to the controller from any registered code within MeerK40t. The LhystudiosController also sends data in a channel called `<context-path>/usb_send` which is the sets of packets the controller has sent. Channels are `.watch()` and `.unwatch()`.
* Console: Commands are sent to the console and parsed to execute commands. These are generally modelled after command-line functions. They are registered in the kernel at `command/<command>` at the global scope and `1/command/<command>` at context specific scopes. If multiple valid registrations for a command exist, the command will be context-first, then global. With the last accessed context being tried first. This can allow for multiple different devices to have the same command and still be entirely able to be accessed. 
* Signals: Signals are a different dataflow metric within the kernel. However, unlike Channels if a signal is called many times very rapidly it will result in a single trigger of the signal. You are not guaranteed to see every packet, you only get to see the last everything listening is notified on an update to a particular signal. 
* Threads: Threads are registered in the kernel and executed like most all multi-threaded operations. These are tracked and viewed with the `thread` channel for diagnostic purposes. And are required to complete for kernel shutdown.
* Scheduler: The scheduler is a thread which provides `Job` instances these can occur a number of times over a given duration. A scheduled job will skip run cycles if the job takes longer than the interval to complete. Refreshing guis and signals are processed through scheduled jobs.
* Timers: Timers are user set internal commands that execute repeatedly. 

## Scheduler

The kernel scheduler is allows for jobs to be scheduled. It launches a thread in the kernel which which iterates through the different scheduled jobs, and running them when required. This thread is also used as during shutdown to terminate everything in a safe manner.

### Jobs
Jobs are either called within a context for add_job() or a module can extend the Job and call schedule() and unschedule().

## Signaler
The signaler schedules itself within the Scheduler, and provides functionality for `listen()`, `unlisten()` and `signal()`. This allows non-duplicating signals to sent, so other instances within the kernel ecosystem know they should update, if they listened for a specific signal. The signaler does not force every message to be delivered, only the last message. When a listener is attached initially, it will get the last message for that signal if it exists. Using these for GUI fields will then give the last message sent, even if the GUI window was launched after that signal was initially sent.

Signals are context dependent.

## Channels

The channels are an aspect of the Kernel. These allow channels to be opened and watched. These will convey every message sent to any watchers observing that channel. There does not need to be an open channel for that channel to be watched. Or a watcher for that channel to be opened. These provide streams of information while being agnostic as to where the information will end up. A channel may be assigned a greet for an initial watcher these can also be opened with a buffer which will be sent to any dialog when connecting to a particular channel. For example, the `usb` channel for the `LhystudiosDevice` is opened with a buffer, so any watchers connecting, such as the `USBConnect` window will be provided that buffer, even if that data happened before the channel was opened.

Channels are context dependent.

## Kernel Registration

Objects are registered in the kernel, not instances of the object but references to that object. All objects that are registered attempt to call static sub_register(kernel) to register additional elements that object may want added. This builds the registered tree of objects that are available. These are references to the classes, small bits of data, and static information and many things that are expected to be reusable such as devices, modules, and modifiers.

## Preferences

Contexts serve as to store persistent information. These are derived from the kernel preference, when the device is loaded.

Preferences are expected to be checked for existence prior to use. The `setting()` command provides a setting_type, a key, and a default value. Once called, `.key` is the preferred access method.

```python
self.context.setting(int, 'my_data', 0)
if my_data == 0:
     channel(_("Default Data Was Found.")
```

In the example, it is entirely possible that `my_data` is loaded from the persistent setting and has a non-zero value. These values are flushed during shutdown. This is the case for any variable that does not start with `_` and which is a `int`, `float`, `str`, or `bool`.

# Context
Contexts serve as path relevant snapshots of the kernel. These are the primary interaction between the modules and the kernel. They permit getting other contexts of the kernel as well. This should serve as the primary interface code between the kernel and the modules. All contexts have delegated access to the kernel functions, several of these are path relative. The same key location at two different contexts refer to two different bits of data.

# Windows

The GUI used registers windows in `window/<window-name>` in the kernel and these can be loaded up like any module. The kernel doesn't need any additional information here.

# Modules
Modules are opened classes. They should be registered in the `module/<module-name>` path in the kernel. These are opened and attached to devices. Sometimes they are registered in other specialty directories like `window` if they are GUI windows and are of little use otherwise. These are opened using the `open()` function on devices and kernels. If this module is already opened. The opened module is returned and any initialization parameters are called on the `restore()` function on the given module.

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

# Backends
Backend devices are registered in `device/<device-name>` this is expected to be referenced in any autobooting device location. (Any context starting with a number).