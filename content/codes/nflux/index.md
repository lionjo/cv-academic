---
title: nflux
summary: A numba accelerated python framework to rapidly calculate neutron wall loads.
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

# nflux: A neutron wall load calculator and optimizer for stellarators.

nflux is a python code that aims for a fast calculation of the neutron load on a 2D hypersurface.
It assumes profiles and a flux surface position. It then calculates a fusion rate obtains the neutron flux on a given hypersurface.

gitlab: https://gitlab.mpcdf.mpg.de/fwa/nflux

## Method

### Instances
[nflux] has two instances
1. The flux surfaces (with profiles assigned)
2. A first wall

<p align='center'>
<img width='250'  src= 2020-10-30-15-04-15.png>
</p>

Internally, these surfaces are represented by a Fourier description. Every surface $s$ is parametrized by two matrices, $R_{m,n}^s$ and $z_{m,n}^s$ is mapped to cylindrical coordinates via:
$$
\begin{aligned}
R&= \sum_{m=0}^{m_{\mathrm{max}}} \sum_{n=-n_{\mathrm{max}}}^{n_{\mathrm{max}}} R_{m,n}^s\cos \left(mu - N_fnv\right) \\
z &= \sum_{m=0}^{m_{\mathrm{max}}} \sum_{n=-n_{\mathrm{max}}}^{n_{\mathrm{max}}} z_{m,n}^s\sin \left(mu - N_fnv\right) \\
\phi &= v
\end{aligned}
$$

### Discretization

The source and the wall are discretized into $N_{wall}$ wall area elements and $N_{source}$ volume elements:
<p align='center'>
<img width='250'  src= 2020-10-30-14-54-37.png>
</p>

The neutron flux at the wall element is given in terms of the nomenclature above as:
$$
\begin{aligned}
Q_{i} = E_n \sum_{j} \frac{\Phi_{j}^{\mathrm{source}}}{4\pi\, d_{i,j}^{2}} { \hat{\mathbf{n}}}_{i} \cdot {\hat{\mathbf{d}}}_{i,j}.
\end{aligned}
$$
Where $E_n$ is the neutron energy of a fusion neutron and $d_{i,j}$ is the distance between wall element and source volume element, $\mathbf{n}$ is the normal vector on the wall element.
The source intensity is obtained by integrating the reaction rate $f_i$ over the volume element:
$$
\begin{aligned}
\Phi^{\mathrm{source}}_{i} =  f_i \left. \sqrt{g} \right\vert_{i} \Delta s_i \Delta u_i \Delta v_i
\end{aligned}
$$


## Interpolation


<!--For each toroidal cross section assign a poloidal cooordinate $u_k$:
$$
\begin{aligned}
    u_k &= \frac{\sum_{i=0}^k||\mathbf{d}_{i}^{(poloidal)}||}{\sum_{i=0}^N||\mathbf{d}_{i}^{(poloidal)}||} \\
    \mathbf{d}_{i}^{(poloidal)} &= \mathbf{d}_i - \left(\mathbf{d}_i \cdot \mathbf{e}_i^{(toroidal)}\right) \mathbf{e}_i^{(toroidal)}, ~i\in[0,N]_\mathbb{N}\\
    \mathbf{d}_i &= \mathbf{x}_i - \mathbf{x}_{i-1}, ~\mathbf{x}_{-1}\equiv \mathbf{x}_N
\end{aligned}
$$

As the first index is supposed to correspond to its $z$ coordinate, redefine $u$ as:
$$
\begin{aligned}
\tilde{u}_j = (u_j-u_0)\frac{1-\tilde{z}_0}{u_{N+1}-u_0}+\tilde{z}_0
\end{aligned}
$$-->

## Use Cases

All figures by Huaijin Wang.

### Compare different stellarator reactors

[nflux] can be used to to compare different stellarators.

<p align='center'>
<img width='550'  src= 2020-10-30-15-53-36.png>
</p>

Peaking factor defined as:
$$
f_{peak} = \frac{q_{max}}{q_{avg}}
$$
This enters directly in reactor designs via
$$
\tau_{blanket}=\frac{d_{max}}{D\cdot\langle nwl\rangle\cdot f_{peak}}
$$
EUROFER steel has maximal dpa of
$$
d_{max} \sim 60~dpa
$$
Steel has
$$
D \sim 10~dpa\frac{m^2}{\mathrm{MW\,yr}}
$$
### Optimize the first wall wrt homogeneity

[nflux] can be used to optimize for a first wall that achieves higher homogeneity:

By modifying the first wall for the same configuration (an upscaled W7-X) one can obtain:
<p align='center'>
<img width='350'  src= 2020-10-30-15-54-56.png>
</p>

Where the first wall was adapted like this:
<p align='center'>
<img width='350'  src= 2020-10-30-15-56-14.png>
</p>

This ignores coil constraints though which is work in progress.



[nflux]: https://gitlab.mpcdf.mpg.de/fwa/nflux