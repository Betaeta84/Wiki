
### DRAFT - to be extended

Whilst there are graphical tools in existence / in prepation to create elements there are a substantial amount of powerful commands available via the console to create simple to complex elements and to manipulate them.

# Element creation


## The grid -command 
The grid command takes at least two parameters to creae a full grid of copies of the selected element(s):
```
grid 3 4
```
will create a grid with 3 columns and 4 rows of the same element(s):

![grafik](https://user-images.githubusercontent.com/2670784/157295043-7360958b-220d-4c24-8c6f-d61e63813979.png)

By default these copies will be place immediately to the right / to the bottom of the original. If you want to space the copies apart you cann add two additional parameters to indicate the distance of one copy to another:

![grafik](https://user-images.githubusercontent.com/2670784/157295394-9fe1f638-ed26-46ae-8553-0e16443672d2.png)

There is one optional parameter origin, that describes where the orginal element(s) shall be located in respect to the grid. The parameter needs to be provided in the format:
````
grid ... --origin ox,oy    or       grid ... -o ox,oy
````
so e.g. 
````
grid  3 3 -o "2,2"
````
will create a 3x3 grid with the original elements right in the middle.
<img width="631" alt="image" src="https://user-images.githubusercontent.com/2670784/157057752-2c42df58-376b-4c1d-aa75-8ca346e023ad.png">

## The circ_copy - command
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

## The circ_array - command
takes some parameters to create (potentially rotated) copies on a circular arc with the orginal element(s) being one of these
```
circ_array <copies> <radius> [ <startangle> <endangle> <rotate> <deltaangle> ]
```
The optional parameters can be provided like:
--rotate --startangle 0deg etc.
<img width="287" alt="image" src="https://user-images.githubusercontent.com/2670784/157231020-90bd23b6-755c-43be-a41b-0988a2941bff.png">
Notabene: While circ_copy is creating copies around the original elements, circ_array is creating all the copies around a center just -1*radius to the left. So the original elements will be part of the circle.

## The polyshape - command
takes some parameters to create a [regular polygon](https://en.wikipedia.org/wiki/Regular_polygon)
<img width="443" alt="Polyshapes" src="https://user-images.githubusercontent.com/2670784/156363092-5d81b7c8-884d-4b00-8ec7-87fc3cc6e3a5.png">
```
polyshape <corners> <x> <y> <radius> <startangle> <inscribe>
```
- \<corners\> defines the amount of vertices of the polygon. 
These lie all on a circle around _x_, _y_  with radius _radius_ (see also _inscribe_ below)
- \<startangle\> (optional) defines where to start with the polygon: <br><img width="307" alt="Polyshape startangle" src="https://user-images.githubusercontent.com/2670784/156363262-c588b32f-1584-4679-9f76-cd78c1df6d35.png">
- \<inscribe\> (optional) as indicated above the vertices lie on a circumscribing circle of the polygon. If you want to have the polygon outside of the inscribing circle you can use the _inscribe_ parameter :
````
polyshape 3 2in 2in 1in --inscribe
````
<img width="248" alt="Circumscribing vs inscribing circle" src="https://user-images.githubusercontent.com/2670784/156576709-5cc53393-9e6f-4182-95ed-03b8e89b99c4.png">

If you do provide the additional parameter "_radius_inner_" then every other point will lie on the inner circle providing a more star-like figure:
```
polyshape 8 5cm 2cm 1cm --radius_inner 50%
````
![grafik](https://user-images.githubusercontent.com/2670784/157079311-3ee8eec5-4aa3-44eb-bc30-2be8ade1db0c.png)
You may specify actually the alternating sequence with the _alternate_seq_ option in conjunction with the aforementioned  _radius_inner_ option. This will create somewhat gear-like shapes:
![grafik](https://user-images.githubusercontent.com/2670784/157080432-4be6e4cf-52d0-4a58-83c5-22383982fb07.png)


## The star - command
takes some parameters to create a [regular star polygon](https://en.wikipedia.org/wiki/Star_polygon) 
```
star <corners> <density> <x> <y> <radius> [ <startangle> ]
```
<img width="483" alt="star" src="https://user-images.githubusercontent.com/2670784/156431877-4885a29e-1a74-48f1-9e3b-2830e71ab3e7.png">

- \<corners\> defines the amount of vertices of the polygon. 
- \<density\> define the line to draw from one vertice to the one \<density\> points away.
- These lie all on a circle around \<x\>, \<y\> with radius \<radius\>
- \<startangle\> (optional) defines where to start with the polygon

Please note that these **may** give some nice stars if the values for corners and density are an odd/even combination (if you want to know more read the wikipedia article linked above about those two numbers supposed to be coprimes). Meerk40t will provide some feedback if the chosen parameters are suboptimal:
<img src="https://user-images.githubusercontent.com/2670784/157040044-c433ae29-7564-4672-85bd-05f8f6cf2a9d.png">