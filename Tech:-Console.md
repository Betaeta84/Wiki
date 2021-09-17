
## Console
Commands are sent to the console and parsed to execute commands. These are generally modelled after command-line functions. They are registered in the kernel at `command/<command>`.

We are addressing the console commands with regard to the Kernel. For general MeerK40t Console See: [[Help: Console Commands]].

To write and register commands your own commands within a module for MeerK40t or plugin. See: [[Tech: Console Command API]]. These as highly expressive as most modern CLI system but include some extended functionality like command chaining based on data context. The theory behind this is borrowed from [vpype](https://github.com/abey79/vpype) in part and [click](https://click.palletsprojects.com/en/7.x/) and other advanced systems to provide the Kernel API a very robust internal command system.
