## Rulers

## Grids
You can influence the display of the rulers and the grid displayed if you right click on the scene background

![image](https://github.com/meerk40t/meerk40t/assets/2670784/8e43c463-5d6c-4f06-9304-d1fb44455f67)

You have the option to choose between 3 different sets of grids:
- The primary grid (the default, which shows the regular bed coordinates)
- A secondary grid (used for instance to display the distortion of a rotary)
- A circular grid (useful for fibre lasers and their centered bed)

<img width="196" alt="image" src="https://github.com/meerk40t/meerk40t/assets/2670784/62a39254-bfe3-4000-87dc-328c75bdc78d">

## Magnets
There is a functionality to quickly align elements: magnet-lines. You are creating them by double-clicking on the ruler-area (and if you click on that area again then the line will be removed).
![magnets](https://user-images.githubusercontent.com/2670784/161030963-fb73907c-bdb0-47dc-b0da-b9b207025dbe.png)
Whenever you move an element (group of elements), if you move it close to a magnet line then the selection will be attracted to it: the left side, the center or the right side for a vertical magnet line, the top side, the center or the bottom side to a horizontal magnet line.
(You can overrule this behaviour if you press the shift-key while moving - then there will be no position adjustment at all).

The "Strength of the attraction" (i.e. how close does a border need to be influenced by a magnet line), can be changed on the fly by right-clicking on the ruler area:

![image](https://github.com/meerk40t/meerk40t/assets/2670784/10a12608-378b-478f-8958-bb6f2ad57397)

You can have as many magnet-lines as you want. You can get rid of all of them if you dbl-click on the units indicator (left top side of the screen) or if you use the context-menu on the rulers. If there weren't any magnet lines then a magnetline for every grid line will be created instead.

![magnet](https://user-images.githubusercontent.com/2670784/161031110-016eb860-7321-4a11-a074-f64effb0a589.gif)

NB: the method for creating a magnetline by double clicking on the ruler area will focus on full tickmarks (i.e. grid lines) and the middle of two adjacent grid lines. If you need something finer, then just zoom in into the scene, this will permit finer positioning of the magnet lines.

