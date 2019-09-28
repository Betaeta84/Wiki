# Position Viewer

The thing that is actually being animated is the position the spooler.

To get the laser to do curves and somewhat diagonal lines, the code divides all commands into a series of octent moves. Which either move one stepper motor, the other stepper motor, or both. So it actually can only go diagonal or orthogonal. But, there's a series of really fantastic algorithms by Zingl who extended Bresenham's algorithms for curves and the like.

The machine can do diagonal and orthogonal moves for long distances. Move right by 1024 is `Bzzzz` Move diagonal by 1024 is `BRMzzzz` (though sometimes you don't need the BR flags). But, move at a 30° angle that long could be a few thousand characters.

What's happening is when the writer finishes writing a command it updates the position and the scene there adds a little segment to the bunch of lines it has there. And moves the circle reticle.

However, since straight line and exact diagonals are single commands, it will zip through them basically instantly. It also, can send that data much more easily to the machine since it's much shorter. A very straight-line centric piece could actually be finished a couple 30 byte packets. The dialog would just zap to the end, the spooler would be done, and the controller would also finish up. But, the laser would still be executing the laser commands.

Part of the issue is that I can't actually know exactly what the laser is doing. I could peek at the packets I'm sending and get a better gauge of the laser position or I could calculate the speed it should take and guess where it is. But, I know exactly what the spooler is doing so the laser head is said to be at the location it said to go at. Without taking into account that the controller will still have to send that packet and the machine still will have to execute it. -- The time between these isn't actually that much so it's a pretty good bet. But, it does result in it moving much faster when it's going orthogonal and diagonal

Usually this time is short. You can pause the spooler and see how long it takes to catch up. And you can pause the controller to see how long it take before the machine stops running commands. It could vary a bit depending on how long the commands take the machine to execute. In rasters it might stop within a second. For a really straight lined vector it could take many seconds. In fact, if it was really slow it could take a couple minutes

But, when the machine takes time to run a command, the controller will not be able to send a new command so the buffer will remain full, it will cause that writer to stop because the buffer is full and the animation to stop too. So it actually does tell you where you'll be in few seconds which is actually useful.