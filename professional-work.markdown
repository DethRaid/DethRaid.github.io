---
layout: post
title:  "Professional Work"
categories: portfolio professional
---
Here you'll find the things I've done professionally, from newest to oldest:

## Senior Rendering Engineer
### Miris, June 2025 - present

I'm working on Miris's spatial streaming solution, optimizing the renderer for low-end devices and also trying to improve visual quality. We're doing cool stuff

## Senior Advanced Rendering Engineer on an Unannounced Game
### WB Games San Diego, April 2023 - March 2025

I worked on an unannounced game! These are always fun to talk about

We're using Unreal Engine 5. My role has revolved around profiling and rendering. I've helped guide conversations about which Unreal Engine 5 features we should use to achieve the artistic vision while running at 60 fps. I've spent a lot of time profiling the game with Unreal Insights, PIX, Nsight, and the Xbox Series and PlayStation 5 profilers. I've tuned various Unreal Engine cvars to make its features fit our gmae a little better, and made a few smaller engine changes to fix bugs and make features work how we need

## Rendering Engineer on Battlefield Mobile 
### EA Industrial Toys, March 2021 - March 2023

<iframe width="560" height="315" src="https://www.youtube.com/embed/rTEiJSO5dO8?si=tmhq-THcqlcZrfcU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

We put Battlefield on a phone! We used Unreal Engine 4, which was fine. We had to do a lot of work to make the game run well on a wide range of Android devices. Unfortunately, EA decided to cancel the game and shut down the studio.

I made a tool to bake out proxy shadow-casting geometry - it created "tiles" out of the level, where each tile contained the triangles in that area of the map that were closest to the sun. Each tile could have triangles from multiple objects in the scene, and a lot of fully-occluded objects weren't represented in the tiles at all. This reduces both our drawcall count and the GPU time.

I also made a bugfix to Unreal's GPU scene auto-instancing - it was using a large buffer to store per-instance data, but Mali GPUs could only access the first 65536 `float4`s in a buffer for some reason. I split the large buffer into smaller buffers, making a few back-to-back compute dispatches to update them. This worked on Mali, but failed on Adreno and Apple! So, we had to use smaller buffers with multiple compute dispatched on Mali, and a bug buffer with one compute dispatch for Adreno and Apple. Fun.

I was working on a new lighting system. Unreal's mobile forward renderer has a few limitations. You can only have four point lights for each primitive, and those lights are calculated with the full GGX BRDF. This was limiting and slow, so we took another approach. We placed 3D textures around the level and baked point and spot lights into them, storing the combined irradiance. This let us have potentially infinte point lights affecting a single primitive very cheaply - three texture samples per pixel to look up the lighting information. This worked great for static lights, but we wanted dynamic lights as well. I added a clipmap of irradiance around the camera and copied the baked 3D textures into the clipmap each frame, and added in contributions from dynamic point lights. Usually each successively-larger clipmap has a side length of 2x the next-smallest, but I used a factor of 3x to get better view distance for less memory. I got this whole system running on a Pixel 2 the very day that we got the news the game was cancelled, so I never got a change to properly profile it - but anecdotally, it didn't noticeably hurt our framerate.

I also added the backend for the in-game rendering settings. This was far more important that I had thought. A lot of YouTubers went into the settings menu and enabled everything - including antialiasing. This is perfectly reasonable, in most games enabling antialiasing improves image quality. That wasn't quite the case for us. We were using Unreal 4's TAA, but had turned the quality way down to get it running on phones. We didn't realize how many people would go into the settings and enable our low-quality TAA, leading to an impression that the game had bad graphics. Whoops. We were going to remove the setting in the next update...

## Gameplay Engineer on Immortals of Aveum 
### Ascendant Studios, May 2019 - January 2021

<iframe width="560" height="315" src="https://www.youtube.com/embed/nzcw3Ommsxc?si=bQ65EXGcy5lbI86n&amp;start=91" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/kxdzu7DrY_U?si=KOA3JqvS2WogNQRV&amp;start=91" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I joined Ascendant Studios in 2019 as a gameplay engineer. At the time the game was built on Unreal 4, using the D3D11 RHI backend. 

I worked on some environmental objects, converting our exploding barrels to the gameplay ability system. I worked on some of the AI code for the Exalted Construct and Archon, both seen above. I helped the Exalted Construct target the player with its slam attacks and ensured that the pickups from its crystals fell where the player could grab them. I also worked on the Archon's beam attack, making it swing back and forth and harass the player.

I also did some performance work on the game. I made a trigger volume that changed rendering settings during cutscene, for things like decreasing shadow distance in areas without long sight lines. I also started looking into Unreal HLODs, trying to bring our drawcall count down. Unfortunately the studio's budget tightened, and I was let go.

## DevOps Engineer for the synthetic data creation toolset 
### Cruise Automation, October 2018 - May 2019

I worked as a dev ops DevOps engineer at Cruise Automation. I was on the team building their simulation environment, which generated synthetic training data for the autonomous cars. My specific role was centered around managing the Jenkins build server, and the automated tests we ran on the simulator. I added some tooling so that artists could update the tests when they made intentinoal art changes, added a Slack bot that posted build status to a specific channel, set up automated backups of the Perforce server, and helped scale the build servers as our capabilities expanded.

I learned that I really don't enjoy dev ops work. When the opportunity came to get back into gamedev, I jumped on it.

## Gameplay/Graphics Engineer on War Commander: Rogue Assult 
### KIXEYE, August 2017 - July 2018

<iframe width="560" height="315" src="https://www.youtube.com/embed/e8De_GOH-RI?si=rFBZAaszqwksYf6Z" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My first job... how young I was

We were using Unity 5 - we upgraded to 5.6 during my time at the company. We were working on a big world map update when I joined, I've embedded the guide to that update above. I did a little bit of a lot of things - I developed some Unity editor extensions to help designers build zones and assign icons for them, I fixed some bugs caused by older iPhones overflowing when adding numbers in shaders, I fought with (and defeated) the Adreno 330's shader compiler. I spent a lot of time maintaining the big world map shader - it drew the hex grid, icons for NPC and player bases, and colored zones based on who owned them. Some older devices had shader compilers that choked on this shader, so I had to rewrite parts of it to trick the compilers into working - but the shader that worked on older Adreno GPUs was horrifically slow on older iPhone GPUs. We needed different code paths for differnet GPUs, which Unity's shader variants system helped us achieve. My time at the company came to an end when they laid off half my team and sold the company.
