# The Backend

The backend driver architecture in MeerK40t is based around, at the core, a threaded queue called a spooler. In the simplest sense this just ensures that two different jobs do not run at the same time and job priority is respected. Each device has a spooler-thread that is tasked with checking the spooler-queue and running any jobs handing those jobs the relevant driver for that device.

## Spooler

The spooler executes in a thread and runs the `Spooler.run()` code which basically queries whether there are any jobs to perform within the queue and the spooler is not shutdown. These jobs can be sorted by priority and checked whether they should hold the current work (as would be expected during a pause).

### SpoolerJobs

The jobs within the spooler should provide a `.status`, an `execute(driver)` function, stop(), and providing some basic things like `elapsed_time()`, `is_running()` and `estimate_time()`

In python, this is called duck-casting. If it looks like a duck, and quacks like a duck, it's a duck; you can call the `quack()` function. The utility here is that `execute(driver)` for any spooler job is given the `driver` object that the program is executing on.

This permits dynamic jobs, like galvo-laser lighting outline jobs, or any custom written jobs. You can program the job to do anything you want or need it to do. You can add that job to the spooler, and when that job, makes it to the top of the queue it will execute the job, even if job itself is infinite or dynamic. The jobs being run in the spooler can perform any actions.

### LaserJob

The most common job however is the LaserJob. This is a simple list of cutcode objects (with some other allowed types). The types are not predefined but anything which provides either a `generate()` generator.

### Lists and Direct Generators

Any items which are themselves able to generate objects like a list, are called recursively. And so a `list()` of generators is called in order.


#### Generate()

Each item in a `LaserJob` is expected to yield `tuples` or `str` that are either command calls or command calls with arguments.

For example, the HomeCut object has a generator that reads:

```python
    def generate(self):
        yield "home"
```

And the `DwellCut` object has a generator:

```python
    def generate(self):
        yield "rapid_mode"
        start = self.start
        yield "move_abs", start[0], start[1]
        yield "dwell", self.dwell_time
```

The `generate()` functions are effectively yielding data to execute on the driver. The expectation is that a `driver` will consist of a object with a lot of hypothetical functional calls. These function calls do not need to exist. But, for example, if the driver has a command `dwell` which accepts a `dwell_time` argument then that function will be called by the `DwellCut` bit of cutcode. 

The most important `.generate()` function is the one called in the `CutCode` class. Which reads:

```python
    def generate(self):
        for cutobject in self.flat():
            yield "plot", cutobject
        yield "plot_start"
```

This effectively sets the stage for how `Cutcode` is processed and sent to the drivers. Each `CutObject` we call the `driver.plot()` function and then when done we call the `driver.plot_start()` function. So CutObjects like `CubicCut`, `LineCut`, `QuadCut` etc are usually just sent to the driver via a `plot()` command.

### Plot-Generators

The geometry `CutObject`s typically have a `generator()` function which produces `x,y,power` values for the movements. Sometimes the geometry is sent directly to the laser (such as happens with GRBL), and other geometric shapes are interpolated. Other times, it's processed in something like a `PlotPlanner` to ensure these single step moves are useful and in line with driver specific settings. How this works is entirely the domain of the driver.

## Driver

So the spooler on the device is calls each job in sequence calling `.execute(<driver>)`. And ultimately what a driver is, is not specified in this definition. It could be anything, but typically it's any object with the correct functions. Typically the driver takes those function calls and deals with the interfacing whether USB, network, or other.

The function of the driver is by convention rather than decree. Any thing that accepts the right calls is a driver. 

Some calls are expected to exist by the spooler:
* `hold_work(priority)` -- Asks whether the spooler should stop at this point or continue spooling. If work comes back with a hold we force wait the spooler.
* `job_start(job)` -- Notifies that the job is started.
* `job_finish(job)` -- Notifies that the job is finished.
* `laser_off()` -- Turns off the laser.
* `laser_on()` -- Turns on the laser.
* `plot()` -- Sends a bit of cutcode.
* `plot_start()` -- Notifies that the cutcode sending ended.
* `move_abs(x, y)` -- Moves the laser in real world units (absolute).
* `move_rel(dx, dy)` -- Moves the laser in real world units (relative).
* `home` -- Homes the laser.
* `physical_home` -- Homes the laser (physically invokes homing routine).
* `unlock_rail` -- Issues an unlock rail command.
* `wait(ms)` -- Causes the laser to pause execution for this period of time.
* `wait_finished()` -- Causes the laser to sync to realtime. Should have an empty and fully executed queue.
* `function()` -- Calls the function given to the function. (This is to execute things in order).
* `beep()` -- Calls for a system beep.
* `console()` -- Calls for the calling of this console command.
* `signal(signal, *args)` -- Calls to issue this signal.
* `pause()` -- Pause the laser.
* `resume()` -- Resume the laser.
* `reset()` -- Reset the laser.

Some care needs to be taken with regards to pausing the laser to allow `resume()` to be allowed. These are usually executed real-time and are not typically called as part of a Job.

The job of the driver is to translate whatever function calls made on the class into something the laser understands. How this happens differs from driver to driver. There are also commands that may not make functional sense from one driver to another, these calls can be ignored. Drivers are not required to have any of these function calls and lasers do not necessarily need to implement these types.


# Special Examples

The backend includes a lot of abstraction. Where completely different implementations can be permitted and implemented without causing any issue to the backend. This is by design. While the most basic example would be to implement a device which has a spooler and driver. There are a lot of easy to understand hacks into this backend to implement killer features.

## GCode Save

Here is the save gcode `save_job` command:

```python
        def gcode_save(channel, _, filename, data=None, **kwargs):
            if filename is None:
                raise CommandSyntaxError
            try:
                with open(filename, "w") as f:
                    driver = GRBLDriver(self)
                    job = LaserJob(filename, list(data.plan), driver=driver)
                    driver.out_pipe = f.write
                    driver.out_real = f.write
                    job.execute()

            except (PermissionError, OSError):
                channel(_("Could not save: {filename}").format(filename=filename))
```

To write a given set of cutcode (found in `data.plan`) to disk, we create a `GRBLDriver` object a `LaserJob` object which contains the relevant cutcode and points to the driver. And we redirect the output for the `out_pipe` and `out_real` to `f.write` and execute the job. This calls all the needed commands on the `Driver` because that's what the `LaserJob` does. All the output data is redirected to saving to file out.


## RDJob

The RDJob is used for both `Blob` loading, so that you could execute a .rd file on a lihuiyu laser if you wanted and for the `ruidacontrol` program. In both cases what happens is that the RDJob implements the hooks to being a `SpoolerJob`. So it contains some amount of raw ruida laser code.

The RuidaEmulator takes any data sent to it and replies with Ruida-like responses. But, any program data is written directly to the RDJob which is then sent to the spooler. So if you open a `.rd` file it creates a `Blob` node which represents all the commands in that raw data. The `RDJob` can parse that data and will call the functions on a `driver`. This means that RDJobs in the spooler can execute Ruida code regardless of the type of job. In a raw file, this merely sends a static job. In the case of `ruidacontrol` it sends a dynamic job repeatedly as more data is sent to the RDJob and it requeues the job as needed.

## Driver To Path

Another special example is the PlotterDriver in `driver_to_path()` this class emulates a normal driverlike plotter. So if you execute a SpoolerJob(), but pass the PlotterDriver to the Job it will create a path and any relevant operations. This permits use to convert a `.rd` blob back into a regular Path if we want. We take anything executed on the Driver by the SpoolerJob() to rebuild path data.

## Future Proof

The general layers of abstraction are so that we can add new features and makes existing architecture a lot easier to use. If a new laser, or other device, has something we've never encountered before, we do not need to rebuild everything from scratch. We can add new types of already existing roles. So, for example, there is an expectation that we should switch out the cutcode use in the backend. This means we would need a different type of SpoolerJob than LaserJob, one which carries `Geomstr` backend data, which would call a different command on the driver. 

While such a change would be very profound, we could just as easily establish a parallel system and deprecate the previous system. Though Cutcode, `plot()`, and `LaserJob` are intrinsic the current system. All that the backend requires is that we have a type of spooler-job that calls arbitrary functions on a driver.



