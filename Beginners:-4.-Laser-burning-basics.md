The purpose of this Wiki page is to give you an idea of the many things that you can do with your K40 and MeerK40t, and we hope that this will help you get a better idea of the things you might need to do in MeerK40t to make your burn a success.

If you haven't already read the safety issues listed on the [Preparing you K40 Laser](./Beginners:-1.-Preparing-your-K40-laser) page, or haven't yet [installed MeerK40t](./Beginners:-2.-Installing-MeerK40t) we recommend that you go back a step or two and read them.

If you want to return to the [Beginners Index](./Beginners:-0.-Index) click [here](./Beginners:-0.-Index).

## Contents
* [Vectors and Bitmaps](#vectors-and-bitmaps)
* [The 4 different types of burn](#4-different-types-of-burn)
* [A brief physics lesson](#a-brief-physics-lesson)
* [Health and Safety](#health-and-safety)
* [Materials you can laser](#materials-you-can-laser)

## Vectors and Bitmaps
The difference between Vectors and Bitmaps (or Rasters) is not difficult to understand:
* A vector is essentially a very thin line - it can be straight or it can be curved, but it is VERY thin - in theory it has infinitely small width, but in the case of lasering it is the width of the laser dot, which is a tiny faction of an inch or centimetre. For example, you would use a vector to describe the edge of the leather design you are going to cut out, or perhaps the edges of the letters you are going to burn onto the material. In the case of letters, the vectors would normally be one or more closed loops, and in this case the vectors might have a fill colour or gradient of colours.

* A bitmap is a 2-dimensional matrix of equal sized dots - and each dot can be any colour (or for lasering grey dots of varying darkness / brightness). Sometimes we constrain these dots to be either black or white rather than shades of grey. Photographs are held in a computer as bitmaps (in a variety of file formats), and sometimes diagrams which could be described using vectors are provided in image form as well.

## 4 different types of burn
In essence, there are two basic ways that your K40 can burn

* [Vector burns](#vector-burns)
* [Raster burns](#raster-burns)

and there are two variants for each of these, giving us four possible burn *Operations*.

### Vector burns
In a vector burn, in its simplest form the laser turns on, the laser head traces a path, horizontally, vertically, diagonally or in curves, and the laser burns away some or all of the material on that path. Because the focused laser beam is very small, these burns are very narrow.

If the laser passes across the material reasonably quickly or at lower power, the depth of the burn will be low leaving a thin groove in the material. This is called a **vector engrave"".

If you laser more slowly or at higher power or laser exactly the same path several times, the depth of the burn will be far greater, and if you do it powerfully enough you will eventually burn right the way through to the other side of the material. This is called a **vector cut**.

### Raster burns
A completely different way of burning is by sweeping the head backwards and forwards (normally in the X-direction) and moving slightly in the other direction for every sweep, turning the laser on and off extremely quickly indeed (up to several thousand times a second) to create areas where the material is burned, and areas where it isn't. If you are old enough to remember dot-matrix computer printers producing pictures, this is essentially the same concept - and indeed unless you are still using a typewriter type of printer (Teletype) pretty much every printer works this way these days (even though the laser printer hides this complexity very well indeed).

Photographs - which are bitmaps already - have to be burned this way. And if you want to burn the fill inside vectors, then again this is the only way to do it.

TODO: A short (20s) YouTube video showing a K40 doing these two types of burn.

So now we have described four types of burn Operation:

1. Vector **Cut**
2. Vector **Engrave**
3. Vector Fill **Raster**
4. **Image** Raster

and when we look at the user interface in the next lesson, you will immediately see these Operations reflected in MeerK40t.

### File formats
MeerK40t supports burning from the following filetypes:
* SVG - primarily designed for vectors, but images can also be included. This is the most comprehensive file format for providing designs for MeerK40t to burn. There are several graphical programs available for creating / editing SVG files, but the most common one used within the K40 community is [Inkscape](https://inkscape.org/).
* JPG, PNG, GIF - all of these are image (raster) formats.
* DXF

## A brief physics lesson
Hopefully you will already understand that the K40 laser works by burning away (or melting and evaporating) the surface of your material, and that it does this by focusing a laser light onto a very very small dot. Although the amount of energy you receive standing on a sunny beach is much more than the amount put out by your K40 laser, on the beach this energy is spread across the whole surface of your body - but by focussing the smaller amount of energy from the laser onto a tiny point, it can create great heat.

Staying with our beach analogy for a little longer, if you walked out from the shade onto the beach and then back into the shade, the longer you stayed out in the sun the warmer you would get - and the same is true for the laser. The longer it stays on the same spot of your material, the hotter your material will get and the more it will be burned. In other words, the amount of material that your laser will burn away depends on the power setting for the laser **and** the speed that the laser moves across the material. The more powerful the laser beam and / or the slower it moves, the greater the burn.

Similarly if you stay on the beach too long each day, you will get sunburned, but if you stayed for a short amount each day for several days, you will tan instead of get sunburn - and the same applies to lasering, as often several burns at a faster speed will produce less charring and a more pleasing result.

**Note:** For porous materials like wood or leather, you can also reduce charring by allowing the material to soak up water before you burn it.

TODO: Short youtube video showing vector burns on cardboard or wood at different speeds and different powers.

## Health and Safety
Before you start using your laser, it is worth covering in a bit more detail some of the dangers that this will incur and how to minimise those risks so that you can use your K40 safely. I am sure that we may be accused of nannying our users, but we would rather risk annoying you by ramming it home than hear later about an avoidable injury.

### Eyesight
As we have already mentioned, although the amount of sun that lands on your body when you stand out in the sun is far greater than the power of the K40 35W laser, the laser beam is very narrow and the power lands on a very small spot indeed. If the spot that the laser beam hits is inside your eye, you can be permanently blinded - and since the CO2 laser tube produces infra-red light which is invisible, you will literally never see it coming. If you want to see some examples of how short a pulse of laser light is needed to cause major damage, take a look at [this video](https://www.youtube.com/watch?v=-wXApAAh8xA).

The clear window in the K40 lid should have a reddish/orange tint and is designed to prevent Infra-Red light from the laser getting out of the K40 and into your eyes. Whilst the laser beam itself is in the infrared and stopped by the acrylic without any tinting, the laser also produces non-laser light at a wide range of other frequencies (including the also invisible ultra-violet band) which will be reflected more widely e.g. by the mirrors, and the colour of the window is designed to filter these out too. [This video](https://www.youtube.com/watch?v=tkPak94T0pA) gives a detailed explanation of K40 laser light safety.

Whilst wood and leather might absorb all the laser light, other materials (one example being acrylic) may absorb only part of the laser beam and reflect the rest. 

Additionally, if the material you are lasering burns, the light created by the burn - which is visible - can itself be bright enough to cause damage on your retinas. The window of the lid of your K40 is **not** designed to reduce these to safe levels.

For all the above reasons, you should **never** have the K40 lid open when the laser is operating.

The risk of damage to your eyesight can be mitigated by one or more of the following methods (which may seem to you like common sense and obvious, but are nonetheless worth stating for anyone for whom they are not obvious):

* Keep the lid of your laser machine fully closed when burning.  
* Fit a safety switch to the lid so that if it is lifted the power to the laser is cut off - better that your burn be ruined that one of your children losing their eyesight.
* Invest in a set of CO2-laser-specific glasses (which should be EN207 certified) to protect your eyes against the infrared laser beam. Consider getting dark versions of these glasses to protect against the bright "burn" light which is not laser reflection.
* Consider painting your bed with `Matt Black Stove Paint` - `Matt` means that it will not reflect laser light, `Black` means it will absorb the laser light instead, `Stove` means that it can survive high temperatures without burning or blistering etc. Doing this will have the added benefit of preventing reflections from burning the back of your material.

![image](https://user-images.githubusercontent.com/3001893/124183858-bc1c4280-dab0-11eb-8820-d30a1895927f.png)

### Fire
Laser cutting/burning only works because the laser is hot enough to burn or melt/evaporate the material. Usually the exposure to the level of heat to do this is brief enough and local enough not to cause a major fire (though it is common to experience excessive charring from localised flames around the laser beam) - but clearly there is a risk that the material might get too hot and burst into flames. If for any reason your stepper motors stop working whilst the laser continues to operate, the risk of fire may be severe. **The risk of fire** from a machine specifically designed to burn things should not be ignored.

These risks can be mitigated by:

* Always remaining with your K40 whilst you have a burn in progress
* Ensure that your K40 has an Emergency Stop Button / Off switch and regularly test that it is working
* Have a properly maintained CO2 fire extinguisher close at hand
* Keep a spray bottle of water to hand to supress small flames that may char your material (and which might grow into a major fire)

### Electrocution
Whilst you may get a nasty shock from the mains voltage in your house (at 240-V or 110V), providing that it is a short shock (e.g. where you jerk your hand away - rather than a continuous shock because e.g. you cannot let go of the wire) this is most unlikely to kill you, particularly if you have a Residual Current Circuit Breaker (RCD) in your mains installation.

However, your K40 CO2 laser operates using **an ionising voltage of between 14,000 and 20,000 volts** and this is quite high enough to kill you if you get shocked by it. Furthermore, your K40 power supply has capacitors in it that can retain this high voltage for some hours after it is turned off, so please be **very, very careful indeed** when you are poking around in the area of the high voltage wires (back of the PSU).

### Fumes
Some materials burn with unpleasant fumes. Other materials can give off genuinely toxic fumes - so you should research your materials to make sure that you know the risks. Either way you do **not** want to be breathing these fumes.

Additionally, you do not want the smoke from the burn so far from blocking the laser light from the burn you are continuing to make.

So an effective air extraction system for your K40 is essential. (See [Preparing Your Laser - Fumes](/meerk40t/meerk40t/wiki/Beginners:-1.-Preparing-your-K40-laser#fumes) for more details.

### Burns


## Materials you can burn
* Wood
* Paper & Cardboard
* Leather
* Acrylic
* Ceramic Tiles
* Slate

TODO: This section needs a lot of expansion

## Next page
Now that you are familiar with the types of Laser burns and the sorts of materials you can burn, the next step in the Beginners' Tutorial is to [understand the user interface](./Beginners:-4.-Understanding-the-user-interface).

If you haven't already read the safety issues listed on the [Preparing you K40 Laser](./Beginners:-1.-Preparing-your-K40-laser) page, or haven't yet [installed MeerK40t](./Beginners:-2.-Installing-MeerK40t) we recommend that you go back a step or two and read them.

If you want to return to the [Beginners Index](./Beginners:-0.-Index) click [here](./Beginners:-0.-Index).

---
### Authors
The MeerK40t team is grateful for the help from @Sophist-UK in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃