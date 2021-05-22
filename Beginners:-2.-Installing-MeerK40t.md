You can run MeerK40t either from a self contained executables (for Windows 32-bit or 64-bit, MacOS, OSX, Linux, Raspberry Pi 32-bit or 64-bit) or you can run from the Python source code (perhaps on other platforms if the drivers are available).

Most people tend to run from a self-contained executable if there is one for their platform.

Once you have read and digested this page, the next step in the Beginners' Tutorial is to [understand the MeerK40t user interface](./Beginners:-3.-The-User-Interface).

If you haven't already read the safety issues listed on the [Preparing you K40 Laser](./Beginners:-1.-Preparing-your-K40-laser) page, we recommend that you go back a step and read it.

If you want to return to the [Beginners Index](./Beginners:-0.-Index) click [here](./Beginners:-0.-Index).

## Contents
* [Precompiled Binaries](#precompiled-binaries)
* [Running from Python Source](#running-from-python-source)

## Precompiled Binaries

The precompiled binaries are available from https://github.com/meerk40t/meerk40t/releases

In most cases, it should be as simple as downloading the binaries and installing them using the normal facilities of your operating system.

Detailed instructions are available for the following operating systems:
* [ARM: Linux: Raspberry Pi](./Install:-Raspberry-Pi)
* [Intel: Linux: Mint](./Install:-Linux-Mint)
* [Intel: Linux: Ubuntu](./Install:-Ubuntu-Linux)
* [Intel: Linux: Ubuntu 18.04.03](./Install:-Ubuntu-18.04.3)
* [Intel: Mac OSX](./Install:-Mac-OSX)
* [Intel: Windows](./Install:-Windows)

TODO: As you can see, there is some duplication here. If you are willing to help, and can test these instructions, any de-duplication would be helpful.

**Notes:**
1. Precompiled Binaries do not have the translations for other locales/languages working very effectively. However at present there are no comprehensive translations available, so this may not be an issue. **But if you want to contribute** by working on [translations](./Tech:-Foreign-Language-Translations), this might change.

## Running from Python Source
The pre-packaged executables above consist of a combined Python interpreter, any co-requisite Python packages and a snapshot of the MeerK40t source code.

An alternative to installing and running an executable is to install Python, the co-requisite Python packages and a copy (snapshot or Git clone) of the MeerK40t source code.

Why would you want to do this?
* An executable is not available for your hardware architecture and operating system but a version of Python and all the co-requisite Python packages are available
* You want to assist with the development of MeerK40t itself or of a plugin

Doing this requires a bit of technical knowledge of Python and will be more difficult to support than a packaged executable.

Please follow the [detailed installation instructions for running from Python source](./Install:-Running-from-source).

## Next page
Now that you have installed MeerK40t, the next step in the Beginners' Tutorial is to [understand the MeerK40t user interface](./Beginners:-3.-The-User-Interface).

If you haven't already read the safety issues listed on the [Preparing you K40 Laser](./Beginners:-1.-Preparing-your-K40-laser) page, we recommend that you go back a step and read it.

If you want to return to the [Beginners Index](./Beginners:-0.-Index) click [here](./Beginners:-0.-Index).

---
### Authors
The MeerK40t team is grateful for the help from @Sophist-UK in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃