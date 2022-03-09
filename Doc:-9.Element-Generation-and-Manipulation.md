
### DRAFT - to be extended

Whilst there are graphical tools in existence / in prepation to create elements there are a substantial amount of powerful commands available via the console to create simple to complex elements and to manipulate them.

# Element creation
## Basic Elements
### circle          circle <x> <y> <r> or circle <r>
### ellipse         ellipse <cx> <cy> <rx> <ry>
### line            adds line to scene
### outline         outline the current selected elements
### polygon         polygon (float float)*
### polyline        polyline (float float)*
### rect            adds rectangle to scene
### text            text <text>

## Advanced Shapes
### The shape - command
takes some parameters to create a [regular polygon](https://en.wikipedia.org/wiki/Regular_polygon) or regular star polygons (see below)
<img width="525" alt="image" src="https://user-images.githubusercontent.com/2670784/157543255-3f740ba4-9a72-4d14-9483-d345a7695ab0.png">

```
shape <corners> <x> <y> <radius> <startangle> <inscribed>
```
- \<corners\> defines the amount of vertices of the polygon. 
These lie all on a circle around _x_, _y_  with radius _radius_ (see also _inscribe_ below)
- \<startangle\> (optional) defines where to start with the polygon: <br>
<img width="381" alt="image" src="https://user-images.githubusercontent.com/2670784/157543827-66ed5b5a-de82-4d01-bd28-eeb275b55960.png">

- \<inscribed\> (optional) as indicated above the vertices lie on a circumscribing circle of the polygon. If you want to have the polygon outside of the inscribing circle you can use the _inscribed_ parameter :
````
shape 3 2in 2in 1in --inscribed
````
<img width="323" alt="image" src="https://user-images.githubusercontent.com/2670784/157544641-4106aa99-5a5d-4c50-b87e-8c5ab6a2708c.png">

If you do provide the additional parameter "_radius_inner_" then every other point will lie on the inner circle providing a more star-like figure:
```
shape 8 5cm 2cm 1cm --radius_inner 50%
````
![grafik](https://user-images.githubusercontent.com/2670784/157079311-3ee8eec5-4aa3-44eb-bc30-2be8ade1db0c.png)
You may specify actually the alternating sequence with the _alternate_seq_ option in conjunction with the aforementioned  _radius_inner_ option. This will create somewhat gear-like shapes:
<img width="386" alt="Gears" src="https://user-images.githubusercontent.com/2670784/157545282-cb92b724-d45d-4c90-a1f1-adfdf737cae4.png">

If you provide the optional parameter "--density" then you will create a [regular star polygon](https://en.wikipedia.org/wiki/Star_polygon) 
```
shape <corners> <x> <y> <radius> [ <startangle> ] --density num
```
<img width="150" alt="image" src="https://user-images.githubusercontent.com/2670784/157546288-e25480e0-9c7e-41f9-8840-4be32e7cfae8.png">

- \<corners\> defines the amount of vertices of the polygon. 
- \<density\> define the line to draw from one vertice to the one \<density\> points away.
- These lie all on a circle around \<x\>, \<y\> with radius \<radius\>
- \<startangle\> (optional) defines where to start with the polygon

Please note that these **may** give some nice stars if the values for corners and density are an odd/even combination (if you want to know more read the wikipedia article linked above about those two numbers supposed to be coprimes). Meerk40t will provide some feedback if the chosen parameters are suboptimal:
<img width="416" alt="density" src="https://user-images.githubusercontent.com/2670784/157542101-d507a104-bed6-4c2f-89ae-ae64a8cff902.png">

You can combine and experiment with all parameters to get some nice looking polygons, i.e.
<img width="265" alt="image" src="https://user-images.githubusercontent.com/2670784/157542754-5d56f65b-1a2b-4110-947e-0988706c9df4.png">

NB: you may invoke shape with 1 corner and 2 corners as well: these will result in 1 single point, a line with length _radius_ respectively
<img src="https://user-images.githubusercontent.com/2670784/157540787-a6641f08-9fe7-4d42-8e04-d7a69ad63ea4.png" >

### outline         outline the current selected elements


## Element Duplication

### The grid -command 
The grid command takes at least two parameters to create a full grid of copies of the selected element(s):
```
grid 3 4
```
will create a grid with 3 columns and 4 rows of the same element(s):

![grafik](https://user-images.githubusercontent.com/2670784/157295043-7360958b-220d-4c24-8c6f-d61e63813979.png)

By default these copies will be placed immediately to the right / to the bottom of the original. If you want to space the copies apart you can add two additional parameters to indicate the distance of one copy to another:

![grafik](https://user-images.githubusercontent.com/2670784/157295394-9fe1f638-ed26-46ae-8553-0e16443672d2.png)

There is one optional parameter origin, that describes where the orginal element(s) shall be located in respect to the grid. The parameter needs to be provided in the format:
````
grid ... --origin ox,oy    or       grid ... -o ox,oy
````
so e.g. 
````
grid  3 3 -o 2,2
````
will create a 3x3 grid with the original elements right in the middle.
<img width="631" alt="image" src="https://user-images.githubusercontent.com/2670784/157057752-2c42df58-376b-4c1d-aa75-8ca346e023ad.png">

### The circ_copy - command
takes some parameters to create (potentially rotated) copies on a circular arc around the orginal element(s)
<img width="599" alt="image" src="https://user-images.githubusercontent.com/2670784/157058280-411695d1-5fe0-4693-a5c4-aaadcee6bf51.png">
```
circ_copy <copies> <radius> [ <startangle> <endangle> <rotate> <deltaangle> ]
```
The optional parameters can be provided like:
--rotate --startangle 0deg etc.
<img width="262" alt="image" src="https://user-images.githubusercontent.com/2670784/157059433-5d055323-af31-4acd-b25a-76037a3915fd.png">
Notabene: normally you will equally distribute the amount of copies between startangle and endangle. If you omit the endangle and provide the optional _deltaangle_, then the amount of copies will be created starting from the _startangle_ every one _deltaangle_ apart:
![grafik](https://user-images.githubusercontent.com/2670784/157075081-24466851-7c48-4813-880b-adf4ff0be136.png)

### The radial - command
takes some parameters to create (potentially rotated) copies on a circular arc with the orginal element(s) being one of these
```
radial <repeats> <radius> [ <startangle> <endangle> <rotate> <deltaangle> ]
```
The optional parameters can be provided like:
--rotate --startangle 0deg etc.
<img width="395" alt="image" src="https://user-images.githubusercontent.com/2670784/157548376-7087cd96-c26e-4ddb-b880-7ebf306b6a7b.png">

Notabene: While circ_copy is creating copies around the original elements, radial is creating all the copies around a center just -1*radius to the left. So the original elements will be part of the circle.

# Element Manipulation
copy
align           align elements
delete          delete elements
fill            fill <svg color>
matrix          matrix <sx> <kx> <sy> <ky> <tx> <ty>
merge           merge elements
path            convert any shapes to paths
reify           reify affine transformations
reset           reset affine transformations
resize          resize <x-pos> <y-pos> <width> <height>
rotate          rotate <angle>
scale           scale <scale> [<scale-y>]?
step            step <raster-step-size>
stroke          stroke <svg color>
stroke-width    stroke-width <length>
subpath         break elements
translate       translate <tx> <ty>