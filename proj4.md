---
layout: default
title: Project 4
nav_order: 5
has_children: false
permalink: /proj4
---

# Project 4

## Overview


## Collaboration


## Part 4

Currently, our cloth will clip through itself upon self-collision. In part 4, we will efficiently implement self-collision using spatial hashing.

To implement the spatial map, we loop through all the point masses and hash their position. This position is a unique float identifier that represents the top left corner of a 3D box volume that a point occupies, and we used a polynomial to create a float from the x, y, and z coordinates. 

Now that we have a spatial hash map we can refer to, for every timestep we loop through every point and check to see if it will collide with another point on the cloth. We do this by hashing the point to check if any other points are within the 3D box volume. If there are, we need to determine whether they are 2 * thickness apart, and if so, we compute and apply a correction vector to the point mass. 

For part 4, our approach was to write the hash function, the spatial map, and then the self collision code. We had some bugs when implementing — the cloth was not rendering correctly in the first place, so we had to go back to previous parts and fix our code dealing with vertical cloths. Then, our cloth was still clipping on itself, and we realized that we were not initializing our spatial hash map properly. Once we fixed these mistakes, our cloth was successfully self-colliding with itself.

Here are some screenshots of the cloth falling on itself:

<img src="proj4_assets/fall_1.png" width=320>
<img src="proj4_assets/fall_2.png" width=320>
<img src="proj4_assets/fall_3.png" width=320>
<img src="proj4_assets/fall_4.png" width=320>

When varying the Ks values, a very low Ks value (ks = 10 N/m) makes the cloth very bouncy where you can almost see individual vectors and pieces of the cloth bouncing on itself. It also begins to unravel itself very quickly because of this, and does not come to rest easily. A higher Ks value (ks = 10000 N/m) creates a less springy, much smoother and stiffer cloth that easily comes to rest.

<img src="proj4_assets/ks_10.png" width=320>
<img src="proj4_assets/ks_10000.png" width=320>

When density is low (15 g/cm^2), the cloth feels very light and has very curvy bends when it folds over itself. It almost looks like a sheet of paper. However, when density is higher (150 g/cm^2), the cloth feels a lot heavier and looks more like a piece of clothing. It forms a lot of small bends when it folds over itself.

<img src="proj4_assets/density_15.png" width=320>
<img src="proj4_assets/density_150.png" width=320>

## Part 5

A shader program is a program that runs in parallel on the GPU. They take in an input and output a single 4D vector and help execute sections of the graphics pipeline to accelerate the raytracing process. The two shader types we deal with in this part of the project are vertex and fragment shaders. Vertex shaders help apply transforms to vertices, which help us write displacement mapping. Fragment shaders take in geometric attributes of a fragment to compute a color. Together, these two shaders work to create realistic lighting and material effects by calculating the interaction of light with the surfaces of 3D objects.

Our approach was to understand how to write the GLSL language by reviewing documentation and a couple examples. We then wrote and debugged each approach.

The first shading we implemented was diffuse shading, which was a simple equation we had to output. Then, in Blinn-Phong shading, we added ambient and specular lighting to the diffuse component. All these different types of light allow the cloth to reflect light more accurately, which are represented in the Blinn-Phong model. The implementation was a bit tricky, especially for specular shading. We fixed a lot of our bugs by normalizing our vectors and reviewing the diagrams in class to make sure we were utilizing the right vectors and values.

Below, we have screenshots representing only the ambient component, diffuse component, and the specular component.

<img src="proj4_assets/ambient.png" width=320>
<img src="proj4_assets/diffuse.png" width=320>
<img src="proj4_assets/specular.png" width=320>
<img src="proj4_assets/full.png" width=320>

We then implemented texture mapping. This was a fairly easy implementation, as all we needed to do was call the built-in function to sample from a texture at texture space uv.

<img src="proj4_assets/texture.png" width=320>

For the bump mapping implementation, we needed to factor in the height of the texture to output the right color. This was done by calculating a TBN (tangent-bitangent-normal) matrix which we multiplied to find the displaced model space normal, which was substituted into our diffuse and specular lighting to create a “bump” textured effect. For displacement mapping, we needed to actually alter the vertices for the cloth in the .vert file. 

# HI I NEED HELP HERE 

At a low coarseness, there is not too much difference between bump and displacement mapping. However, at a higher coarseness, the displacement mapping is a lot more accurate (e.g. we can see the cracks in the bricks) and something we might want to use in actual simulations.

High coarseness:

<img src="high_c/full.png" width=320>

Low coarseness:



Last but not least, we created a mirror shader that can sample an incoming radiance. It reflects an environment map with the cloth, as shown below.




https://michelllepan.github.io/cs184-proj-webpage/