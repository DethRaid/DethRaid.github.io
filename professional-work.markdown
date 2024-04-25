---
layout: post
title:  "Professional Work"
categories: portfolio professional
---
Here you'll find the things I've done professionally, from newest to oldest:

## Senior Advanced Rendering Engineer on an Unannounced Game (WB Games San Diego, 2023 - present)

I'm working on an unannounced game! These are always fun to talk about

We're using Unreal Engine 5. My role has revolved around profiling and rendering. I've helped guide conversations about which Unreal Engine 5 features we should use to achieve the artistic vision while running at 60 fps. I've spent a lot of time profiling the game with Unreal Insights, PIX, Nsight, and the Xbox Series and PlayStation 5 profilers. I've tuned various Unreal Engine cvars to make its features fit our gmae a little better, and made a few smaller engine changes to fix bugs and make features work how we need

## Rendering Engineer on Battlefield Mobile (EA Industrial Toys, 2021 - 2023)

<iframe width="560"
        height="315"
        src="https://www.youtube.com/watch?v=rTEiJSO5dO8"
        frameborder="0"
        allow="autoplay; encrypted-media"
        allowfullscreen></iframe>

We put Battlefield on a phone! We used Unreal Engine 4, which was fine. We had to do a lot of work to make the game run well on a wide range of Android devices

I made a tool to bake out proxy shadow-casting geometry - it created "tiles" out of the level, where each tile contained the triangles in that area of the map that were closest to the sun. Each tile could have triangles from multiple objects in the scene, and a lot of fully-occluded objects weren't represented in the tiles at all. This reduces both our drawcall count and the GPU time

I also made a bugfix to Unreal's GPU scene auto-instancing - it was using a large buffer to store per-instance data, but Mali GPUs could only access the first 65536 `float4`s in a buffer for some reason. I split the large buffer into smaller buffers, making a few back-to-back compute dispatches to update them. This worked on Mali, but failed on Adreno and Apple! So, we had to use smaller buffers with multiple compute dispatched on Mali, and a bug buffer with one compute dispatch for Adreno and Apple. Fun

I was working on a new lighting system when EA cancelled the game and shut down the studio. Unreal's mobile forward renderer has a few limitations. You can only have four point lights for each primitive, and those lights are calculated with the full GGX BRDF. This was limiting and slow, so we took another approach. We placed 3D textures around the level and baked point and spot lights into them, storing the combined irradiance. This let us have potentially infinte point lights affecting a single primitive very cheaply - three texture samples per pixel to look up the lighting information. This worked great for static lights, but we wanted dynamic lights as well. I added a clipmap of irradiance around the camera and copied the baked 3D textures into the clipmap each frame, and added in contributions from dynamic point lights. Usually each successively-larger clipmap has a side length of 2x the next-smallest, but I used a factor of 3x to get better view distance for less memory. I got this whole system running on a Pixel 2 the very day that we got the news the game was cancelled, so I never got a change to properly profile it - but anecdotally, it didn't noticeably hurt our framerate

I also added the backend for the in-game rendering settings. This was far more important that I had thought. A lot of YouTubers went into the settings menu and enabled everything - including antialiasing. This is perfectly reasonable, in most games enabling antialiasing improves image quality. That wasn't quite the case for us. We were using Unreal 4's TAA, but had turned the quality way down to get it running on phones. We didn't realize how many people would go into the settings and enable our low-quality TAA, leading to an impression that the game had bad graphics. Whoops. We were going to remove the setting the next update, but...

## Gameplay Engineer on Immortals of Aveum (Ascendant Studios, 2019 - 2021)

<iframe width="560"
        height="315"
        src="https://youtu.be/kxdzu7DrY_U?t=91"
        frameborder="0"
        allow="autoplay; encrypted-media"
        allowfullscreen></iframe>

I joined Ascendant Studios in 2019 as a gameplay engineer. 
