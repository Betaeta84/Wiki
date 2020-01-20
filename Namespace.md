# Namespace Declaration

`<svg> attribute: xmlns:meerk40t="https://github.com/meerk40t/meerk40t/wiki/Namespace"`

This basically just flags the SVG as being produced by Meek40t.

The elements in the file will be <text>, <image> and <path> and these can be given attributes regarding properties for the lasercutter:

* 'speed',
* 'overscan',
* 'power',
* 'passes',
* 'raster_direction',
* 'raster_step',
* 'd_ratio'

If set on an svg object, these will be used when the file is loaded. You can use this to code data elsewhere and load it in MeerK40t.