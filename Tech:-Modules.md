
## Modules
Modules are assumed to be allow many copies of the same class. These are opened at the context and stored in the `.opened` dictionary. These are given a name which can be dynamically assigned. They are not expected to change the context at which they are opened. 

Modules are opened classes. They should be registered in the `module/<module-name>` path in the kernel. These are opened and attached to devices. Sometimes they are registered in other specialty directories like `window` if they are GUI windows and are of little use otherwise. These are opened using the `open()` function on devices and kernels. If this module is already opened. The opened module is returned and any initialization parameters are called on the `restore()` function on the given module.