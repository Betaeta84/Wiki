If you are going to write code either for your own purposes or for the community at large you need to understand the kernel.

The Kernel/Context API system started in MeerK40t 0.3.0, and underwent several major revision on 0.7.0.

# Kernel

The Kernel serves as the central hub of communication between different objects within the system. These are mapped to particular contexts that have locations within the kernel. The contexts can have modules opened and modifiers applied to them. The kernel serves to store the location of registered objects, as well as providing a scheduler, signals, channels, and a command console to be used by the modules, modifiers, devices, plugins, and other objects.

The Kernel stores a persistence object, thread interactions, contexts, a translation routine, a run_later operation, jobs for the scheduler, listeners for signals, channels of dataflow, a list of devices, registered commands, plugins, a central dictionary of registered objects.


## Registration
A main dictionary of various keys to functions, data, modules, console commands etc. 

Objects are registered in the kernel, not instances of the object but references to that object. All objects that are registered attempt to call static sub_register(kernel) to register additional elements that object may want added. This builds the registered tree of objects that are available. These are references to the classes, small bits of data, and static information and many things that are expected to be reusable such as devices, modules, and modifiers.

## Context
Locations within the kernel space at specific paths. For example, the camera add-on stores camera settings in `camera/0` through `camera/5` and these settings are independent of each other.

Contexts serve as path relevant snapshots of the kernel. These are the primary interaction between the modules and the kernel. They permit getting other contexts of the kernel as well. This should serve as the primary interface code between the kernel and the modules. All contexts have delegated access to the kernel functions, several of these are path relative. The same key location at two different contexts refer to two different bits of data.

### Preferences

Contexts serve as to store persistent information. These are derived from the kernel preference, when the device is loaded.

Preferences are expected to be checked for existence prior to use. The `setting()` command provides a setting_type, a key, and a default value. Once called, `.key` is the preferred access method.

```python
self.context.setting(int, 'my_data', 0)
if my_data == 0:
     channel(_("Default Data Was Found.")
```


In the example, it is entirely possible that `my_data` is loaded from the persistent setting and has a non-zero value. These values are flushed during shutdown. This is the case for any variable that does not start with `_` and which is a `int`, `float`, `str`, or `bool`.

## Modifiers
Modifiers provide functionality for a particular context. The assumption is modifiers will alter the context or provide a link to the main code. For example `Spooler` provides .spooler at the given context as place to send cutcode. Modifiers also get called `boot()` when the kernel boots at start-up. These are always called the name of the modifier itself. So `modifier/Spooler` is opened as `modifier/Spooler`

## Modules
Modules are assumed to be allow many copies of the same class. These are opened at the context and stored in the `.opened` dictionary. These are given a name which can be dynamically assigned. They are not expected to change the context at which they are opened. 

Modules are opened classes. They should be registered in the `module/<module-name>` path in the kernel. These are opened and attached to devices. Sometimes they are registered in other specialty directories like `window` if they are GUI windows and are of little use otherwise. These are opened using the `open()` function on devices and kernels. If this module is already opened. The opened module is returned and any initialization parameters are called on the `restore()` function on the given module.

## Channels
Channels are dataflows within the kernel. For example the LhystudiosController accepts data through a channel. Likewise you send data to the controller from any registered code within MeerK40t. The LhystudiosController also sends data in a channel called `<context-path>/usb_send` which is the sets of packets the controller has sent. Channels are `.watch()` and `.unwatch()`.

The channels are an aspect of the Kernel. These allow channels to be opened and watched. These will convey every message sent to any watchers observing that channel. There does not need to be an open channel for that channel to be watched. Or a watcher for that channel to be opened. These provide streams of information while being agnostic as to where the information will end up. A channel may be assigned a greet for an initial watcher these can also be opened with a buffer which will be sent to any dialog when connecting to a particular channel. For example, the `usb` channel for the `LhystudiosDevice` is opened with a buffer, so any watchers connecting, such as the `USBConnect` window will be provided that buffer, even if that data happened before the channel was opened.

Channels are context dependent.

## Console
Commands are sent to the console and parsed to execute commands. These are generally modelled after command-line functions. They are registered in the kernel at `command/<command>` at the global scope and `1/command/<command>` at context specific scopes.

We are addressing the console commands with regard to the Kernel. For general MeerK40t Console See: [Help: Console Commands].

Console commands within the API are registered with several decorators. Here's an example used internally for the `thread` command:

```python
        @self.console_command("thread", help="show threads")
        def thread(command, channel, _, args=tuple(), **kwargs):
            """
            Display the currently registered threads within the Kernel.
            """
            channel(_("----------"))
            channel(_("Registered Threads:"))
            for i, thread_name in enumerate(list(self.threads)):
                thread = self.threads[thread_name]
                parts = list()
                parts.append("%d:" % (i + 1))
                parts.append(str(thread))
                if thread.is_alive:
                    parts.append(_("is alive."))
                channel(" ".join(parts))
            channel(_("----------"))
            return
```

### context.console_command
The decorator is being used to register the console command. This can be done *anywhere* in registration code.

#### command
The command is either a string or tuple of strings (if this is reserving more than one namespace) which is the command that will be typed to trigger this command. It is acceptable to use the same command used elsewhere *only* if the namespace of the input_type is different. For example both `operations` and `clipboard` have a `cut` command. But, because `clipboard` must be given a `clipboard` type the namespaces do not overlap and both can be used in their respective contexts.

#### hidden
Hidden prevents this command from showing up in help.

#### help
Help is the quick help message of this particular command.


### context.console_argument

Registers required operands for the command, these must exist. If they do not the command is called with them set to `None`.

```python
        @self.console_argument("extended_help", type=str)
        @self.console_command(("help", "?"), hidden=True, help="help <help>")
```

For example, here's the argument for `help`/`?` it accepts an extended_help which is the command that should be looked up and presented to the user for extended help. In the signature of the function being decorated there an argument `extended_help`

`def help(command, channel, _, extended_help, output=None, input=None, args=tuple(), **kwargs):`

It does not need to be defaulted because it will be given a value, even if that value is `None`

#### type
The type argument is used to define a given type for this console_argument. These can be anything and the string data will be used to initialize this type.

#### input_type
The input_type is either a string or tuple of strings to be used to register the command in the kernel. This command will only be called if the data_type matches the data_type of the given input_type. If set to None then this should be the first command in a series, if set to a data_type *and* `None` then the command must be able to be both the first command in the sequence *and* able to take the requested data_type.

#### output_type
The output_type will give the valid data types this command may return. Any console_command should return `"output_type", <data>` unless the result of the function in None. If the output type is `None` or omitted there is no chaining for the command `args` or `remainder` values will be given. With a given output_type, the expectation is this is a chained function and any command accepting the given output_type can be chained together with this function.

#### regex
The regex is a bool that says whether the given command is a regex of a pattern that would match the given command. In some extreme cases, the `bindalias` captures `.*` values as an alias can be established dynamically. It is also common to use this for suffix values like `plan0` or `timer2` where the suffix is part of the command and given to the Console Function in the `command` value.

#### help
The help data will provide the quick command explanation located in help. The extended help will be the docstring within the decorated command.

#### default
The default is the value to be used if no value is given.

### context.command_option
Provides optional 

For example this option for path 
```
@self.console_option("path", "p", type=str, help="Path of variables to set.")
```

#### action
The action can only be "store_true", works for bool values to simply store the value `true` if they exist.

#### default
The default is the value to be used if no value is given.


### Console Function
The console function is the function being decorated

#### command
The command is either a string or tuple of strings (if this is reserving more than one namespace) which is the command that will be typed to trigger this command.

#### channel
The channel is the console channel. This is to assist in giving replies and responses to the console or providing data if that's the goal of this function.

#### _ (translation)
The _ function is the translation function. If we are providing information we should do in a way that respects internationalization.

#### <arguments>
The arguments are required arguments in the function. These will be defined with `console_argument` functions.

#### <options>
The options are optional arguments provided with either a `-` prefix of a single letter or `--` prefix for the entire option name.

#### args
The args function provides a tuple of string objects that were parsed, but not used. If no `output_type` is defined for the given function all remaining arguments are passed to this console function.

#### remainder
The remainder is a single string of the remaining values in a non-parsed fashion.


## Signaler
Signals are a different dataflow metric within the kernel. However, unlike Channels if a signal is called many times very rapidly it will result in a single trigger of the signal. You are not guaranteed to see every packet, you only get to see the last everything listening is notified on an update to a particular signal.

The signaler schedules itself within the Scheduler, and provides functionality for `listen()`, `unlisten()` and `signal()`. This allows non-duplicating signals to sent, so other instances within the kernel ecosystem know they should update, if they listened for a specific signal. The signaler does not force every message to be delivered, only the last message. When a listener is attached initially, it will get the last message for that signal if it exists. Using these for GUI fields will then give the last message sent, even if the GUI window was launched after that signal was initially sent.

Signals are context dependent.


## Threads
Threads are registered in the kernel and executed like most all multi-threaded operations. These are tracked and viewed with the `thread` channel for diagnostic purposes. And are required to complete for kernel shutdown.

## Scheduler

The scheduler is a thread which provides `Job` instances these can occur a number of times over a given duration. A scheduled job will skip run cycles if the job takes longer than the interval to complete. Refreshing guis and signals are processed through scheduled jobs.

The kernel scheduler is allows for jobs to be scheduled. It launches a thread in the kernel which which iterates through the different scheduled jobs, and running them when required. This thread is also used as during shutdown to terminate everything in a safe manner.

### Jobs
Jobs are either called within a context for add_job() or a module can extend the Job and call schedule() and unschedule().

## Timers
Timers are user set internal commands that execute repeatedly.


***

# Plugins

Plugins are registered either directly in the main as would be done during compiling into an .exe or .dmg file, or are utilized through `pip` to allow manipulations of the meerk40t systems.

Writing a plugin for Meerk40t is easy. See the example plugin https://github.com/meerk40t/meerk40t-example-plugin

The relevant code is an entry point in the `setup.cfg`:

```ini
[options.entry_points]
meerk40t.plugins = Example = example.main:plugin
```

And the `plugin()` function:

```python 
def plugin(kernel, lifecycle):
    if lifecycle == 'register':
        """
        Register our changes to meerk40t. These should modify the registered values within meerk40t or install different
        modules and modifiers to be used in meerk40t.
        """

        @kernel.console_command('example', help="Says Hello World.")
        def example_cmd(command, channel, _, args=tuple(), **kwargs):
            channel(_('Hello World'))
    elif lifecycle == 'boot':
        """
        Do some persistent actions or start modules and modifiers. Register any scheduled tasks or threads that need
        to be running for our plugin to work. 
        """
        pass
    elif lifecycle == 'ready':
        """
        Start process running. Sometimes not all modules and modifiers will be ready as they are processed in order
        during boot. If your thread or work depends on other parts of the system being fully established they should 
        work here.
        """
        pass
    elif lifecycle == 'mainloop':
        """
        This is the start of the gui and will capture the default thread as gui thread. If we are writing a new gui
        system and we need this thread to do our work. It should be captured here. This is the main work of the program. 
        """
        pass
    elif lifecycle == 'shutdown':
        """
        Meerk40t's closing down, our plugin should adjust accordingly. All registered meerk40t processes will be stopped
        any plugin processes should also be stopped so the program can close correctly.
        """
        pass
```

The code in the kernel calls plugin() with different lifecycle events for all `meerk40t.plugins` which allows them to register themselves and alter the code as needed. In the example, we use the `kernel.console_command` to register a command called "example" which calls the channel with "Hello World" during the `register` lifecycle. These plugins will be found when installed with `pip`. The pure python version of MeerK40t will find and execute this plugin, this will add a command that sends "Hello World" to the channel when called. The `_` function is for translation purposes.


***

# MeerK40t Specific API

## Windows

The GUI used registers windows in `window/<window-name>` in the kernel and these can be loaded up like any module. The kernel doesn't need any additional information here. These are loaded by calling open() with the given module or by calling the `window` command in console.

## Loaders
kernel.add_loader()

Loaders are required to have a few functions:
`load_types()`: This is a generator which gives three values: str: Description, tuple: Extensions, str: Mimetype.

This is intended to give the description of what this loads, the extensions those files can be found under, and the mimetype of that type of file.

`load(pathname, [group])`: The path of the thing needing to be loaded. And optional the LaserNode group this should be added to.

Loaders are expected to append the data of that file path to the kernel.elements value.

## Savers
kernel.add_saver()

Required saver functions are:

`save_types()`: This is the generator which gives three values: str: Description, str: Extension, str: Mimetype.

Unlike the loaders the second item is `Extension` and is intended to give 1 extension value: the one we should use.

## Backends

Backend devices are registered in `device/<device-name>` this is expected to be referenced in any autobooting device location. (Any context starting with a number).