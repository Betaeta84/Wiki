# Simulation

![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/3324df5a-3910-4f94-a54f-2aaae9e82881)

The simulation window allows you to preview the expected burn results: sequence of operations, durations etc.
It allows to fiddle with optimisation parameters and get an insight if and to what extent certain optimisation options would speed up the burn process.

If you are happy with what you are seeing then can you send the cutcode (the translation of elements and operations into laser commands) directly to the laser.

Optimisations: If you click on the button with the Arrow left indicator (<img src="https://github.com/meerk40t/meerk40t/assets/2670784/7da3759d-a85e-4d0d-8b3f-d62c27a24fa6" width="25">), then a panel with optimisation options appear. You can change them and click "Recalculate" to assess the impact these will have. You will find details about the optimisation options here: [Optimisation options](https://github.com/meerk40t/meerk40t/wiki/Online-Help:-OPTIMISATION)

### Tips: 
- Change the Mode-Option at the bottom of the screen to change between
  - Step: jumps from one draw primitive (shape segment) to another - useful to see how mk has split the things into smaller parts. Images on the other hand are considered just one primitive.
  - Time (either in sec or min) - are more realistic view in terms what happens during a given interval.
- You can zoom in and move around the scene preview by using the mouse scrollwheel as you can do in the main scene.

### Expert Tips:
(I hope you know what you are doing ;-) )
- Right click on the scene to get a context menu.
![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/e5070d2c-9bbf-4574-8619-8df0f306c348)
You can intentionally drop all lasercommands before / after the currently displayed execution step - this is helpful if you need to restart a job and don't want to do all the things until the point you needed to interrupt (intentionally stopped) again. Go to this point (or as close as you can get) and delete the cuts before.
- If you fold out the optimisation menu at the right hand side you have two more tabs, that could be interesting:
   - Operations: This a list of all the cutcode groups that MeerK40t creates under the hood. You can add additional instructions like a command to interrupt the burn and wait for a user command, and many more.
NB: Normally you would want to add special operations in the operation part of the tree (see operations), but this is an expert tool to change behaviour at a core level.
![grafik](https://github.com/meerk40t/meerk40t/assets/2670784/c17848e6-e6cb-4da9-be16-127fc6780d52)

   - Cutcode: if you select an operation in the previous tab, you can then see (and influence) the detailed laserinstructions.
