--- 
layout: post
title: "Personal Work"
tags: portfolio personal
---

Here you'll find my personal projects, from newest to oldest:

## Android Renderer

![Sponza with a light propagation volume](/img/AndroidRendererSponzaLPV.png)

It's a renderer... for Android devices. You already knew that

I worked on this between by time at EA and WB Games. It's a mobile-first rendering engine designed to take advantage of the unique features of mobile GPUs. It's built in C++, using the Vulkan graphics API.

This project implements a deferred renderer with light propagation volumes for global illumination. I'm using Vulkan subpasses to make the deferred rendering fast. I do the expected thing of keeping the gbuffers in on-chip memory, and I do a maybe-unexpected thing of using subpasses to keep the reflectance shadow maps in on-chip memory.

If you're not familiar with LPVs, here's the basics: You render the scene from the light's perspective, storing not just depth but the object's color and normal as well. This is called a "reflectance shadow map" (RSM) and can be thought of as a thin gbuffer. Then, you examine the RSM to find the brightest pixel in an area. Write the color and normal of that to a buffer - this is called a virtual point light (VPL). You convert each VPL to spherical harmonics and inject it into a 3D texture - this 3D texture is the titular light propagation volume. You can run a few passes over the 3D texture to propagate light outward, then you can just sample the LPV at each pixel's location.

As mentioned above, I use Vulkan subpasses to keep the RSM in on-chip memory. Vulkan subpasses have a limitation: Subsequent subpasses can only read the local pixel from the previous pass - they can't examine a 4x4 region of a texture and select the brightest point, for instance. However, some phones support fragment shader subgroup ops - allowing fragment shader threads to send data to _each other_. So, each fragment shader thread can sample the local pixel, compare its sample to other samples in the same subgroup, and select the brightest one. This works on newer phones, but if I wanted to ship this project for real I'd need a fallback for older phones. Probably flushing the RSMs to VRAM and sampling them with a compute sahder. Bleh.

Injecting VPLs is easy. I'm using a geometry shader to push each VPL to the right point in the 3D texture, and the rasterizer handles atomically adding the colors together. This does not cause any measurable problems on my test hardware.

That's about as far as I got before I started working at WB Games

## Sanity Engine

![Lumberyard Bistro, pathtraced!](/img/SanityEnginePathtracedBistro.png)

An engine made to increase my sanity!

This project started when I was at Ascendant Studios. I needed something in my life that wasn't Unreal Engine, so I started making this. It's a PC rendering engine with dreams of running a real game, made using C++ and DirectX 12

The renderer is a forward renderer with ray traced sky and sun lighting, and no GI. I'm just sending ray queries from the pixel shaders, it's fine. There's no support for point or spot or mesh lights. I'm pretty sure I have GPU-driven culling in this project, but it's been a while since I worked on it so I don't quite remember

## Nova Renderer

A renderer that did not increase my sanity 

This project was born out of my time working with the Minecraft shaders mod. The mod was somewhat limiting, and I wanted to do more - so why not make a whole new renderer for Minecraft?

This project would have been a lot more succesful if I hand't switched technologies constantly. I started this a few months before the release of Vulkan, so I used OpenGL 4.5 initially - then switched to Vulkan when it launched. I kept coding myself into corners with Vulkan, so I switched to DirectX 12. I rewrote most of the Vulkan code, but this Rust language thing was getting more popular and seemed pretty nice...

At one point I had this project hooked into Minecraft and rendering fully-opaque blocks in chunks. If I had stuck with it from that point it'd have been something cool, but changing technologies every few months meant I never made much progress. I learned a lot about how to _not_ manage a project, at least

## DRAGON

![DRAGON v1 Shadows and Metals](/img/dragon_v1_ult.png) ![DRAGON v1 Fresnel](/img/fresnel1.png)

This is a collection of shaders for the Minecraft shaders mod. The name stands for DethRaid's Awesome Grpahics On Nitro - yeah, I came up with the name first and figured out what it stood for later :P

I was able to implement some cool features into Minecraft. DRAGON was the first shader pack that implemented physically-based shading, the first to implement image-based lighting, and I had contact-hardening shadows tuned to the in-game size of Minecraft's sun

DRAGON v1 was pretty simple, then I started working on v2. Between improvements to the shaders mod and improvements to my knowledge, I was able to do a lot more:

![Improved PBR](/img/PBR-improved-full.png)
![Screen-space raytraced light](/img/raytraced_light_full.png)
![SColored Shadows](/img/shadows-colored-full.png)
