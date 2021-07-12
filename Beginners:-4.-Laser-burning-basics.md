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
We are not going to repeat the information from the [Safety matters](./Beginners:-1.-Safety-matters) and [preparing your K40 hardware](./Beginners:-2.-Preparing-your-K40-hardware) pages here, but if you have not already read these Wiki pages regarding the potential health hazards from your K40, please go back and read them before proceeding further.

## Materials you can burn
* Wood
* Paper & Cardboard
* Leather
* Acrylic
* Ceramic Tiles
* Slate

### Wood

### Paper & Cardboard

### Leather

### Acrylic

### Ceramic Tiles

### Slate

## Next page
Now that you are familiar with the types of Laser burns and the sorts of materials you can burn, the next step in the Beginners' Tutorial is to [understand the user interface](./Beginners:-4.-Understanding-the-user-interface).

If you haven't already read the safety issues listed on the [Preparing you K40 Laser](./Beginners:-1.-Preparing-your-K40-laser) page, or haven't yet [installed MeerK40t](./Beginners:-2.-Installing-MeerK40t) we recommend that you go back a step or two and read them.

If you want to return to the [Beginners Index](./Beginners:-0.-Index) click [here](./Beginners:-0.-Index).

---
### Authors
The MeerK40t team is grateful for the help from @Sophist-UK in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃

## To Do
The sections on different materials need writing with example pictures and links to other resources.