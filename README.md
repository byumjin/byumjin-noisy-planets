# CIS 566 Project 1: Noisy Planets

* Univer sity of Pennsylvania - CIS 566 Project 1: Noisy Planets
* pennkey : byumjin
* name : [Byumjin Kim](https://github.com/byumjin)

![](imgs/main.png)

# Live Demo

[Live Demo Link](https://byumjin.github.io/byumjin-noisy-planets/)

# Overview

- I have created a procedural planet, which is refered to [Implicit Procedural Planet Generation](https://static1.squarespace.com/static/58a1bc3c3e00be6bfe6c228c/t/58a4d25146c3c4233fb15cc2/1487196929690/ImplicitProceduralPlanetGeneration-Report.pdf) mostly, on my shader.


## GUI

![](imgs/GUI.png)

- This project has many options which can control various values to change the planet.
- Tesselations : it decides how much the planet should be divided.
- SunDirection : it shows the sun's direction which indicates the direction of its directional light.
- Colors : The user can pick each terrain layer's color
- Terrain : It has values wihch can change a certain terrain's slope, shape, height and etc.
- Time : The user can switch it off or on.
- EnviromentMap : The user can switch it off or on.
- Noise4D : The user can switch it off or on.
- Shader :  The user can select several surface reflection models.


## Terrain

- Terrains are consists of 5 layers (shoreline, foliage, steep, snow, ice)

### LandScape

- I have used Fractal Brownian Motion to make procedural landscape.

- OceanHeight(absolute value) decides the height of water from the center of planet.

| 0.8 | 0.9 | 1.0 | 1.1 |
| --- | --- | --- | --- |
| ![](imgs/ocean_00.png) | ![](imgs/ocean_01.png) | ![](imgs/default.png) | ![](imgs/ocean_03.png) |

- ShoreHieght(relative value) decides the height of shore area from the Ocean's Height.

- SnowHieght(absolute value) decides the height of snowy area.

| 0.9 | 1.0 | 1.1 |
| --- | --- | --- |
| ![](imgs/default.png) | ![](imgs/snow_01.png) | ![](imgs/snow_02.png) |

- PolarCapsAttitude(absolute Y value) decides the attitude of the starting height of icy land.

| 1.1 | 0.0 |
| --- | --- |
| ![](imgs/polarCap_01.png) | ![](imgs/polarCap_02.png) |

- Using with Terrain Exponential, The user can make default terrain to make it steeper, more dramatic or more homogenous in height.
  Steeper area shows steeps color more, and more homogenous are shows foliage color more. 

| 0.15 | 0.35 | 0.5 | 1.0 |
| --- | --- | --- | --- |
| ![](imgs/exp_00.png) | ![](imgs/default.png) | ![](imgs/exp_01.png) | ![](imgs/exp_02.png) |

### Ocean
- Depending on the water depth from its terrain, deeper ocean has darker color and more wavy surface.
- I also have used Fractal Brownian Motion with higher frequency to make wave.

### Surface Normal
- I think surface normal is the most important feature to make our planet fancy.
- [The paper](https://static1.squarespace.com/static/58a1bc3c3e00be6bfe6c228c/t/58a4d25146c3c4233fb15cc2/1487196929690/ImplicitProceduralPlanetGeneration-Report.pdf) recommands to use Gradient Approximation to get surface normal. But, it is too expensive because it needs use the FBM function 6 times. Instead of using that, I used the result of our FBM with dFdx and dFdy functions on fragment shader. If our drawned number of fragment is less that the number of vertex of our planet, it is much efficient because it just use the FBM fuction 2 times. But, if the area ofour planet on fragment shader is not enough, it will make ugly noise. To handle this, FBM function has LOD parameter. If the distance between the planet and camera is getting further, the number of loop of the FBM decrease. In other word, it shows less detail of the terrain.

| LOD 0 | LOD 1 | LOD 2 | LOD 3 | LOD 4 |
| --- | --- | --- | --- | --- |
| ![](imgs/LOD_01.png) | ![](imgs/LOD_02.png) | ![](imgs/LOD_03.png) | ![](imgs/LOD_04.png) | ![](imgs/LOD_05.png) |


## Reflection Model

- Each terrain layer has its roughness, respectively.
- Depending on its roughness, it reflects different amount of light energy along its surface normal. 

| Lambert | Blinn-Phong | Phsically-based |
| --- | --- | --- |
| ![](imgs/lambert.png) | ![](imgs/blinn.png) | ![](imgs/pbs.png) |

### Environment Map

- Enviroment map is one of the efficient way to make fancy reflection effect with cheap performance.

| Off | On |
| --- | --- |
| ![](imgs/no_env.png) | ![](imgs/default.png) |


## Background

### Halo

- Instead of using ray-tracing to find the edge of the planet, I traced screen space position of the planet to make gradation effect.

| Blue | Pink |
| --- | --- |
| ![](imgs/default.png) | ![](imgs/halo_01.png) |

### Star-field

- I also have used FBM to make twinkling stars.
- Depending on its screen space position, its twinkling period is decided.


## Reference
- [Implicit Procedural Planet Generation](https://static1.squarespace.com/static/58a1bc3c3e00be6bfe6c228c/t/58a4d25146c3c4233fb15cc2/1487196929690/ImplicitProceduralPlanetGeneration-Report.pdf)