MeerK40t uses an internal interim language for spooling and interpreting laser commands, sitting between SVG and the M2-Nano low-level commands.

NOTE: Never use the integer value, only the command name. The integer values are permitted to change.

Some naming implications:

* `Rapid` implies the command is performed in default mode.
* `Cut` implies the laser is on for the command.
* `Shift` implies the laser is off for this command.
* `Simple` means the movement must be an octant move. Either +-x, +-y, or +-x and +-y where abs(x) == abs(y).

* Move alone doesn't imply anything about the laser mode.

# High Level Commands:

* COMMAND_PLOT: takes a plot object with a .plot() function which generates simple plot commands.
* COMMAND_RASTER: takes a plot object with a .plot() function which generates simple raster commands.
* COMMAND_FUNCTION - Execute the function given by this command. Blocking.
* COMMAND_SIGNAL - Sends the signal, given: "signal_name", operands.

# Low Level Commands

* COMMAND_LASER_OFF - Turns laser off
* COMMAND_LASER_ON - Tuns laser on

* COMMAND_RAPID_MOVE - In default mode, performs move
* COMMAND_MOVE - At speed, performs a line move (laser state current)
* COMMAND_SHIFT - At speed, performs a line shift (laser off)
* COMMAND_CUT - At speed, performs a line cut (laser on)
* COMMAND_CUT_QUAD - # From current position. At speed, performs a quadratic bezier cut
* COMMAND_CUT_CUBIC - From current position. At speed, performs a cubic bezier cut
* COMMAND_HSTEP - Causes horizontal raster step
* COMMAND_VSTEP - Causes a vertical raster step
* COMMAND_WAIT - Pauses the given time in seconds. (floats accepted).
* COMMAND_WAIT_BUFFER_EMPTY - WAIT until the buffer is empty or below 1 sendable packet.

* COMMAND_MODE_DEFAULT
* COMMAND_MODE_COMPACT
* COMMAND_MODE_CONCAT

* COMMAND_SET_SPEED - sets the speed for the device
* COMMAND_SET_POWER - sets the PPI power. Out of 1000.
* COMMAND_SET_STEP - sets the raster step for the device
* COMMAND_SET_D_RATIO - sets the d_ratio for the device
* COMMAND_SET_ACCELERATION - sets the acceleration for the device 1-4
* COMMAND_SET_DIRECTION - sets the directions for the device.
* COMMAND_SET_INCREMENTAL - sets the commands to be relative to current position
* COMMAND_SET_ABSOLUTE - sets the commands to be absolute positions.
* COMMAND_SET_POSITION - Without moving sets the current position to the given coord.

* COMMAND_HOME - Homes the device
* COMMAND_LOCK - Locks the rail
* COMMAND_UNLOCK - Unlocks the rail.
* COMMAND_BEEP - Beep

* COMMAND_OPEN - Opens the channel, general hello.
* COMMAND_CLOSE - The channel will close. No valid commands will be parsed after this.

* COMMAND_RESET - Resets the state, purges buffers
* COMMAND_PAUSE - Issue a pause command.
* COMMAND_RESUME - Issue a resume command.
    * A COMMAND_RESUME would have to be issued in realtime since in a paused state the commands
will not be processed.
* COMMAND_STATUS - Issue a status command.
