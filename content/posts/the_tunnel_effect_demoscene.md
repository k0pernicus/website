---
author: "Antonin"
title: "The Tunnel Effect"
date: "2023-08-28T22:45:00+02:00"
tags: ["programming", "swift", "apple metal", "demoscene", "shader", "frame engine"]
description: "Complex but Easy"
toc: false
readTime: true
---

## The demoscene

The tunnel effect is a pretty old-school effect in graphics programming, coming directly from the demoscene.
If you take a look at [pouet.net](https://www.pouet.net) you may find many (many many) examples of a Tunnel effect
for different hardware configurations, like [this one](https://www.pouet.net/prod.php?which=78044) using 3D raycasting.

In this collection of web demo effects you can find and experiment with this effect: 
[https://seancode.com/demofx/](https://seancode.com/demofx/).

In one of the most well-known demos ever, ["Second Reality" from Future Crew](https://www.pouet.net/prod.php?which=63), 
the team presented a very nice Tunnel effect in... **1993**, _**years**_ before any graphics card in a personal computer.
You can take a look at this effect, running on the CPU, [on YouTube](https://youtu.be/8G_aUxbbqWU?si=hRY4tsqnsgZA2Z4p&t=143).

{{<youtube id="8G_aUxbbqWU" autoplay="false">}}

The key word for running this on any PC at this time was "pre-calculation", as the pixel distances and angles of the tunnel
are pre-computed, and the runtime is basically moving around those tables at each frame.  
Pre-calculation is what makes this demo running so fast and so smoothly at 60 frames per second, on a very old PC's CPU.

If you are interested in the CPU computation, you can take a look at [this excellent blog post](https://lodev.org/cgtutor/tunnel.html).

## Running the code

Developing on ["Frame Engine"](/posts/frame_engine_advances_2023), I wondered how long it would take to rewrite this 
effect using a modern graphics API like Metal.

First, as I transformed my 2D engine to full-3D, I forgot to include some very basic 2D meshes like a surface... so,
I wrote a very basic one as an OBJ file:

```obj
mtllib 2D_surface.mtl

v 0.000000 1.000000 0.000000
v 0.000000 0.000000 0.000000
v 1.000000 0.000000 0.000000
v 1.000000 1.000000 0.000000
vt 0.000000 0.500000 0.000000s
vt 0.000000 0.000000 0.000000
vt 0.500000 0.000000 0.000000
vt 0.500000 0.500000 0.000000
# 4 vertices

f 1/1 2/2 3/3 4/4
# 1 element
```

Once I have the surface, let's write the fragment shader.

The Metal fragment shader is actually very simple:

```c++
#define ANIM_DISTANCE 0.45
#define ANIM_ANGLE    0.25

float4 tunnel(float2 worldPosition,
              texture2d<float, access::sample> colorTexture,
              sampler colorSampler,
              constant FragmentUniforms& uniforms) {
    // Compute the angle for the effect
    float  angle = atan2(1.0 + worldPosition.y, 1.0 + worldPosition.x) / PI; 
    // More the pixel is near the center and more it is distant for the effet
    float  distance = sqrt(dot(position, position));                    
    // Compute the UV of the texture to render
    float2 uv = float2(angle, ANIM_DISTANCE + (ANIM_ANGLE / distance));
    // Return the color
    return colorTexture.sample(colorSampler, uv);
}
```

To make a movement, you can multiply for example the **ANIM_DISTANCE** by your timer, like this:

```c++
float2 uv = float2(angle, cos(uniforms.time) * ANIM_DISTANCE + (ANIM_ANGLE / distance));
```

Now that we have the metal shader, let's take a look at the sampler...  
We want to repeat the texture motif for `s` and `t`, we do want **linear** filter near the eye, and **nearest** filter
far the eye.

Associated to the texture, we could set the sampler like this:

```swift
let samplerDescriptor = MTLSamplerDescriptor()
samplerDescriptor.normalizedCoordinates = true
samplerDescriptor.minFilter             = .nearest
samplerDescriptor.magFilter             = .linear
samplerDescriptor.sAddressMode          = .mirrorRepeat
samplerDescriptor.tAddressMode          = .mirrorRepeat
samplerDescriptor.maxAnisotropy         = 2
self.samplerState = device.makeSamplerState(descriptor: samplerDescriptor)
```

Using this setting, and a few textures, you should get the following results:

![First example of the Tunnel effect, rendered in the Frame Engine editor](/images/tunnel_ex_0.png#small)

![Second example of the Tunnel effect, rendered in the Frame Engine editor](/images/tunnel_ex_1.png#small)

The animated version is available on YouTube: {{<youtube id="GqaDGEvxQ5E" autoplay="false">}}.
