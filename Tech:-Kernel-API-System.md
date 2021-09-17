If you are going to write code either for your own purposes or for the community at large you need to understand the underlying architecture.

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
* execution and operation of [[modules|Tech:-Modules]], [[modifiers|Tech:-Modifiers]], [[devices|Tech:-Devices]], [[plugins|Tech:-Plugins]]
* [[translation information and functionality|Tech:-Translations]]
* run_later operations for moving operations to a gui or other main thread
* a list of [[devices|Tech:-Devices]]
* [[plugins|Tech:-Plugins]] api


# MeerK40t Specific API

Often it may be essential to extend MeerK40t in a more specific fashion. Add in a new window, provide a new filetype, edit the tree menu options, or add a function to the system menubar, or define webhelp.

## Loaders
Loaders are part of the elements.py code. They register filetypes to be loaded and stored in the elements tree.

`kernel.add_loader()`

Loaders are required to have a few functions:
`load_types()`: This is a generator which gives three values: str: Description, tuple: Extensions, str: Mimetype.

This is intended to give the description of what this loads, the extensions those files can be found under, and the mimetype of that type of file.

`load(pathname, [group])`: The path of the thing needing to be loaded. And optional the LaserNode group this should be added to.

Loaders are expected to append the data of that file path to the kernel.elements value.

## Savers
Savers are part of the elements.py code. They register filetypes to be exported and saved to disk.

`kernel.add_saver()`

Required saver functions are:

`save_types()`: This is the generator which gives three values: str: Description, str: Extension, str: Mimetype.

Unlike the loaders the second item is `Extension` and is intended to give 1 extension value: the one we should use.

## Backends

Backend devices are registered in `device/<device-name>` this is expected to be referenced in any autobooting device location. (Any context starting with a number).

## Windows

The GUI used registers windows in `window/<window-name>` in the kernel and these can be loaded up like any module. Windows in MeerK40t are `modules`. The kernel doesn't need any additional information here. These are loaded by calling open() with the given module or by calling the `window` command in console, which is registered in the `wxmeerk40t.py` app code. This command cannot be used in purely CLI mode.
