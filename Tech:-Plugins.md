
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
