
## Registration
A main dictionary of various keys to functions, data, modules, console commands etc.

Objects are registered in the kernel, not instances of the object but references to that object. All objects that are registered attempt to call static sub_register(kernel) to register additional elements that object may want added. This builds the registered tree of objects that are available. These are references to the classes, small bits of data, and static information and many things that are expected to be reusable such as devices, modules, and modifiers.
