---
title: Stella-Plot
summary: A wolfram language plot package for a stellarator machine. Provides high level functions to be usable in Mathematica or Python.
tags:
- PROCESS
date: "2021-01-23T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: 
  focal_point: Smart

links:
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

# wolframplotpackage: A Mathematica plot library for stellarators

A plot library for stellarator surfaces and stellarator coils.
`wolframplotpackage` can produce the plot above in 1 line (given a coils file and a vmec netcdf) by typing in wolfram language:
```
plotall[LCFSlocation, Coillocation]
```

It consists of 2 packages which you can load via:
```
<<"surface_render.m"
<<"coil_render.m"
```

Check the namespace:
```
?coilrender`*
?surfacerender`*
```
gitlab: https://gitlab.mpcdf.mpg.de/jtl/wolframplotpackages





## Flux Surfaces

`PlotSurfaceFromFourierVMEC` allows to plot from given fourier coefficients. 

```Mathematica
Fw = Last@
   PlotSurfaceFromFourierVMEC[RmncFW, zmnsFW, ntor, mpol, symmetry, 
    25, 0, Pi];
Sfw = Graphics3D[{Red, Specularity[White, 10], Fw}];
```
<p align='center'>
<img width='250'  src= 2020-10-30-16-04-13.png>
</p>

Directly from a vmec netcdf:
```
LCFS = Last@PlotLCFSFromNetcdf[LCFSlocation, 20];
Graphics3D[{Yellow, Specularity[White, 10], Opacity[.3], LCFS}];
```
<p align='center'>
<img width='250'  src= 2020-10-30-16-06-45.png>
</p>

`PlotSurfaceFromFourierWithScalarFieldVMEC` plots a scalar filed on a given hypersurface:
```
surfaceFW = PlotSurfaceFromFourierWithScalarFieldVMEC[
    RmncFW,zmnsFW,bmnc,ntor,mpol,symmetry,5,-Pi/symmetry,2Pi/symmetry*1.5,"TemperatureMap",2.2,2.7]
```
<p align='center'>
<img width='250'  src= 2020-10-30-16-08-31.png>
</p>


## Coils

```
coils = ImportCoilsFullModule[Coillocation, symmetry];
region = RenderCoils[coils, .6, .8]
```

<p align='center'>
<img width='250'  src= 2020-10-30-16-11-50.png>
</p>

`region` is *Mathematica* `Region` now and can be used as such:
It can be meshed, can be used to solved finite elements on it, can be used to to volume calculation etc.

Prettifiyng can be done by:

```
RegionPlot3D[region, PlotStyle -> {LightGray, Specularity[White, 10]}, 
  Boxed -> False, Mesh -> None, PlotRangePadding -> None]
```
<p align='center'>
<img width='350'  src= 2020-10-30-16-14-33.png>
</p>





## Usage in python
<p align='center'>
<img width='150'  src= 2020-10-30-16-23-27.png>
</p>

`wolframplotpackage` can be used in python too:
https://reference.wolfram.com/language/WolframClientForPython/

This is implemented in `pre-SPROCESS` as a generator of .stl files for stellarator walls and coils.