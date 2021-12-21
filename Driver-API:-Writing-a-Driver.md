The following is written circa 0.8.0 and is intended to be useful in that context.

To write a 0.8.0 driver you must define a [[plugin]] function and add this either directly edited into the meerk40t main.py file or with an entrypoint hook as part of an external plugin.

## Provider

To make a driver you need to register a provider:

```python
def plugin(kernel, lifecycle=None):
    if lifecycle == "register":
        kernel.register("provider/device/mylaserdriver", MyLaserDevice)
```

## Driver Starting

We should also tend to force start any devices that exist of at the desired path for the persistent settings.

```python
    if lifecycle == "preboot":
        suffix = "mylaser"
        for d in kernel.root.derivable():
            if d.startswith(suffix):
                kernel.root(
                    "service device start -p {path} {suffix}\n".format(
                        path=d, suffix=suffix
                    )
                )
```

Alternatively we can just tell the service to start when we boot. This will boot at least one of our devices. While this example has it at boot, we could also do so during the preboot stage.

```python
    if lifecycle == "boot":
        kernel.root("service device start mylaser\n")
```

## Driver

The driver API requires that anything registered as a service for .device must be of type Service and possess the following attributes:

* current_x: float -- Current position in the x coordinate (usually starting at 0)
* current_y: float -- Current position in the y coordinate (usually starting at 0)
* bedwidth: float -- Size of laser bed width
* bedheight: float -- Size of laser bed height
* spooler: Spooler -- The spooler should be of the Spooler class and can be initialized with `Spooler(self)`
* label: str -- What the particular device is called.
* viewbuffer: str -- The buffer of the device if available. This can just be `""`.
* scale_x: float -- The scaling factor in the x direction typically 1.0
* scale_y: float -- The scaling factor in the y direction typically 1.0


```
class MyLaserDevice(Service):
    def __init__(self, kernel, path, *args, **kwargs):
        Service.__init__(self, kernel, path)
        self.name = "MyLaserDevice"
        self.current_x = 0.0
        self.current_y = 0.0
        self.bedheight = 20000.0
        self.bedwidth = 20000.0
        self.scale_x = 1.0
        self.scale_y = 1.0
```

The default native unit is 1 mil. This is equal to a step for a standard K40 Laser. If this is not desired for your laser and you need particular units at a different value the scale_x and scale_y values can be used to convert from native meerk40t native units to the desired units. (Currently in 0.8.0 this doesn't actually do anything, but it will).

## Spooler

The primary method of interacting with a device is through the Spooler. The device must have a spooler so that any projects or data sent to the device can be retrieved and processed. Spoolers are a typesafe queue of lasercode elements. These are typically processed with `.peek()` and `.pop()`

Items in the spooler are `int` or a `tuple` of a single LaserCommand constant, or generators themselves or have a `.generate()` function which yield either integers of LaserCommandConstants or tuples of a LaserCommandConstant with some parameters. For Example:
```python
       def trace_quick():
           yield COMMAND_MODE_RAPID
           yield COMMAND_MOVE, bbox[0], bbox[1]
           yield COMMAND_MOVE, bbox[2], bbox[1]
           yield COMMAND_MOVE, bbox[2], bbox[3]
           yield COMMAND_MOVE, bbox[0], bbox[3]
           yield COMMAND_MOVE, bbox[0], bbox[1]
```

## LaserCode

The current commands for LaserCode are:
```python

COMMAND_LASER_OFF = 1  # Turns laser off
COMMAND_LASER_ON = 2  # Turns laser on
COMMAND_LASER_DISABLE = 5  # Disables the laser
COMMAND_LASER_ENABLE = 6  # Enables the laser
COMMAND_MOVE = 10  # Performs a line move
COMMAND_CUT = 11  # Performs a line cut.
COMMAND_WAIT = 20  # Pauses the given time in seconds. (floats accepted).
COMMAND_WAIT_FINISH = 21  # WAIT until the buffer is finished.
COMMAND_JOG = 30  # Jogs the machine in rapid mode.
COMMAND_JOG_SWITCH = 31  # Jogs the machine in rapid mode.
COMMAND_JOG_FINISH = 32

COMMAND_MODE_RAPID = 50
COMMAND_MODE_FINISHED = 51
COMMAND_MODE_PROGRAM = 52
COMMAND_MODE_RASTER = 53

COMMAND_PLOT = 100  # Takes a cutobject
COMMAND_PLOT_START = 101  # Starts plotter
COMMAND_BLOB = 110  # Data blob.

COMMAND_SET_SPEED = 200  # sets the speed for the device
COMMAND_SET_POWER = 201  # sets the power. Out of 1000. Unknown power method.
COMMAND_SET_PPI = 203  # sets the PPI power. Out of 1000.
COMMAND_SET_PWM = 204  # sets the PWM power. Out of 1000.
COMMAND_SET_STEP = 205  # sets the raster step for the device
COMMAND_SET_DIRECTION = 209  # sets the directions for the device.
COMMAND_SET_OVERSCAN = 206
COMMAND_SET_D_RATIO = 207  # sets the diagonal_ratio for the device
COMMAND_SET_ACCELERATION = 208  # sets the acceleration for the device 1-4
COMMAND_SET_INCREMENTAL = 210  # sets the commands to be relative to current position
COMMAND_SET_ABSOLUTE = 211  # sets the commands to be absolute positions.
COMMAND_SET_POSITION = (
    220  # Without moving sets the current position to the given coord.
)

COMMAND_HOME = 300  # Homes the device
COMMAND_LOCK = 301  # Locks the rail
COMMAND_UNLOCK = 302  # Unlocks the rail.
COMMAND_BEEP = 320  # Beep.
COMMAND_FUNCTION = 350  # Execute the function given by this command. Blocking.
COMMAND_SIGNAL = 360  # Sends the signal, given: "signal_name", operands.
```

The `meerk40t.core.drivers.Driver` code is the default implementation of interpreting this data.

## Spooled Objects

The general outline of how spooling works permits a large number of objects to be spooled so long as they have a .generate() function to produce lasercode or are themselves lasercode.


### Cutcode

Cutcode is usually placed directly in the spooler. This works because cutcode provides a .generate() function which calls `COMMAND_PLOT` for each cutcode object it possesses and finally `COMMAND_PLOT_START` after the final cutcode object. This provides small cutcode objects that should be processed.

Every cutcode object has `.settings` which is a `LaserSettings()` object that should provide the settings for that particular cut. Every cut object also provides `.start()`, `.end()`, `.length()`, `upper()`, `lower()`, `left()`, `right()`, however the most important is the `.generator()` function which provides a series of integer absolute location plot points in `x, y, laser` of what should be lasered and how strongly. A laser value of 0 means do not laser. A laser value of 1 means full laser power. A missing value for laser implies `1` full laser power.

The cutcode objects are `LineCut`, `QuadCut`, `CubicCut`, `RasterCut`, and `RawCut`

The `meerk40t.core.plotplanner.PlotPlanner` code is used to convert these objects into more easily digestible elements especially for the M2 Nano and other traditional xy-plotter type lasers. Doing things like applying PPI, pulse shifting, and grouping elements into direct orthogonal/diagonal movements. Other drivers may not need these operations or they may be needed in these ways.

### Blobs

A data blob is sent via the `COMMAND_BLOB` LaserCode value. These should typically be ignored unless they are specifically for your driver. For example the `EGVBlob` object can be spooled and has a `generate()` function which yields a COMMAND_BLOB and bytearray of lhymicro-gl code. This isn't able to be processed by other drivers and thus should be ignored.

### LaserOperations

Some laser operations are also spoolable objects specifically the `Dots` LaserOperation isn't converted into Cutcode but rather issues a series of commands to move to particular locations and fire the laser for a set period of time. Using LaserCode operations COMMAND_MOVE, COMMAND_WAIT, COMMAND_LASER_ON, COMMAND_LASER_OFF.

### CommandOperation

Command Operations are spoolable and they simply yield whatever command they possess. This is a simple way to send specific instructions to the laser and are usually used to perform COMMAND_HOME, and COMMAND_BEEP, at various times during the processing of the data.

### LaserCodeNode

LaserCode Nodes are just a sequence of LaserCode commands held in a elements.Node type. these yield whatever commands they possess.

## Spooling API

The general idea for how to process code therefore is to perform the actions found in the lasercode of the spooler to the best of the driver's ability. Some of these contain commands like `COMMAND_BEEP` which should just be used to call `beep` within the console. However, if the driver's current progress is unknown or unknowable (send and ignore) style then it might be correct to suppress the beep since it's typically only used for progress indication.

# Console Commands

Drivers are services and will contribute to the lookup while they are active, this means any console commands registered within the service during initialization will be generally available. If there are special functions for the device like connecting or disconnecting a the USB or writing out a special filetype these should be added as part of the console commands. So that when the driver is active the commands will be available to the user.

There is an expectation of some GUI elements that there should be `estop`, `pause`, `resume`, `home` but if these commands don't exist those UI elements will simply not function and it's up to GUI designers to ensure their elements work.

# GUI

Drivers tend to need some driver specific UI elements in order to provide a good user experience. The goal in 0.8.0 and more largely within MeerK40t itself is to provide this by having UI elements specific to a particular driver. This is done using a `service plugin`. When a plugin is registered it is queried for the lifecycle events `service` and `module` and if it returns a provider it will be registered specifically as a `service plugin` or `module plugin`. In our example we would return the established provider in response.

```python
def plugin(service, lifecycle):
    if lifecycle == "service":
        return "provider/device/mylaserdriver"

    if lifecycle == "added":
        service.register("window/Controller", MyLaserControllerGui)
        service.register("window/Configuration", MyLaserConfiguration)
```

We can then also register buttons to open these windows:

```python
        service.register(
            "button/control/Controller",
            {
                "label": _("Controller"),
                "icon": icons8_connected_50,
                "tip": _("Opens Controller Window"),
                "action": lambda v: service("window toggle Controller\n")
            },
        )
        service.register(
            "button/config/Configuration",
            {
                "label": _("Config"),
                "icon": icons8_computer_support_50,
                "tip": _("Opens device-specific configuration window"),
                "action": lambda v: service("window toggle Configuration\n"),
            },
        )
```

Most of the 0.8.0 GUI works like this, all one needs to do is make windows, panes, and buttons and register them within the service. The `service plugin` is given the `Service` object on the lifecycle events. While a traditional plugin would get the Kernel lifecycle events, a `service plugin` receives the lifecycle events of the service. If more than one service is created with the provider the code can be called more than once, thus using a global is undesirable, but the service itself is passed to the plugin() function.

When the service is activated these GUI elements will be displayed and they will be removed when the driver in question is deactivated. So a GUI applied to a driver does not need to use any of this code. The gui plugin() and driver plugin() do not need to overlap. This can allow the meerk40t in a pure command-line mode to interact with your driver without needing wxPython or any other gui library.