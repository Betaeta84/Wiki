# Lifecycle
Plugins are registered both internally and from registered plugins. This is either a list of added values in `main.py` or entry points found in `meerk40t.plugins`. Regardless whether the plugins are internal or dynamic they work the same way through a series of lifecycle events. These are performed by the typical execution of the code calling `kernel.bootstrap(<event>)` at various parts doing program execution.

There is no expectation that these lifecycle events will be the exhaustive and if some code must at a certain point, it is acceptable to add in lifecycle events. 

All registered plugins both internal and dynamic call `def plugin(kernel, lifecycle)` this is called with the kernel object and the current lifecycle. This is done for each lifecycle event. Plugins are required to use these to register and execute themselves at the appropriate time within the overall program's lifecycle.

## console
Console occurs if there is to be no gui before pre-register.

The `wxmeerk40t` plugin uses `console` to register a `gui` command to launch the main window. So if the `-z` flag for no-gui is used rather than `-Z` to fully suppress the gui, this permits launching the gui from console.

## gui
Gui occurs if there is a to be a gui before pre-register

## preregister
Preregister is the first lifecycle step, before most data is registered in the kernel.

The `wxmeerk40t` plugin uses `preregister` to register the wx.App and the open this at the context `'/'`, and to ensure that `renderer.make_raster` code exists at kernel registered location `render-op/make_raster` as this code can be used (if it exists) in the planner.

## register
Register is the step when all plugins are expected to register their functionality in the kernel.

Many modules call this to register their code in the kernel. For example, `elements.py` calls `kernel.register("modifier/Elemental", Elemental)`. Some modules simply register console commands or other bits of code at this phase. 

## configure
Configure is after the registrations occur, when various processes need to occur but before booting.

## shutdown
Shutdown happens during program shutdown. The expectations is most things should have stopped ended by this point in the lifecycle.

## boot
Boot occurs after the start of the scheduler, and registration of the kernel-console commands.

Many modules use this to call activate and attach various modules registered during the register lifecycle phase.

## ready
Ready is post boot after everything should be loaded, registered, and booted.

## mainloop
Mainloop is a thread capturing lifecycle operation. In many gui startup routines the gui captures and uses the mainthread through the gui lifecycle process. This should occur during mainloop. There is no assurance that of the order of the plugins so this could occur either before the gui or after the gui for any particular plugin. Depending on the order the gui plugin.

The `wxmeerk40t` plugin uses this to start the meerk40tgui and execute `MainLoop()`.

