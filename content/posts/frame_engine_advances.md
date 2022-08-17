+++
title = "Update on Frame engine (1/x)"
date = 2022-08-17T19:20:00+02:00
draft = false
categories = ["programming", "swift", "swiftui", "apple metal", "game engine", "frame engine"]
tags = ["programming", "swift", "swiftui", "apple metal", "game engine", "frame engine"]
description = "\"Making games is hard, y'know\""
disqus = false
+++

Today I wanted to share my advancements on `Frame`, a game engine I began almost two years ago, for Apple devices 
only.

In December 2020 I played a bit with Swift, SwiftUI, and Metal, on a custom game engine called "`Frame`".  
At that time I was not yet a father, and I had a lot of spare time (too much maybe) to spend on this project.  
I dropped the project a year after, in December 2021, as it became very difficult for me to focus and spend my 
spare time on this kind of difficult project.

As making games is hard, making game engines is 100x harder, trust me.

Recently, during an incredible sunny day, I re-opened the `Frame` project, looked at the code and, using the strength 
of a powerful and deep voice, said **"WhAt ThE F#$% Is ThIs CoDe?!"**.  
After one year and a half I became a much better Swift programmer, and I noticed quickly the core mistakes
I made, which "forced me" to step back from the project.  
So, during two week-ends and a few days of vacations, I get back on "`Frame`" to deliver a much better experience to
the game developer (actually 'me'), more features, better performance, and... more platforms support!

### Better code abstraction

The first thing I made was to make my Swift code **simpler to understand** and **better to maintain**.  
I removed a lot of duplicated things, introduced new code abstraction for better code management, make the `Scene` object
as a real code representation of a  _game scene_, etc.

The mistakes I made in the first version of the engine was the consequence of how I learned Swift and SwiftUI, and
how I understood the concepts at this time.

Now, as I became better using those tools, I am able to detect some anti-patterns and to fix them quickly.  
Pretty nice!

In the first version of `Frame`, which was a simple 2D rendering tool, this is my codebase 
(using [cloc](http://cloc.sourceforge.net)):
```sh
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Swift                           18            170            249            741
Metal                            2              8             19             28
-------------------------------------------------------------------------------
SUM:                            20            178            268            769
-------------------------------------------------------------------------------
```

For the new version of `Frame`, which is basically a 2D/3D rendering tool with proper 3D objects and multi-platforms support:
```sh
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Swift                           14            142            204            791
Metal                            1             14             13             59
C/C++ Header                     1              7             11             24
-------------------------------------------------------------------------------
SUM:                            16            163            228            874
-------------------------------------------------------------------------------
```

which is "only" 100 more lines in total, not bad at all!

The C/C++ header is to use only one representation of data structures to pass in the shaders, 
instead of duplicating the data structures each time.

### The camera object

In the previous version of `Frame`, there was no proper camera - only a fixed "eye" at origin (0, 0, 0) that displays 
everything around him with a limit of (1, 1, 1).

The camera is now an object, and contains a proper Field of View (FOV) property, and a limit (the `far limit`) to avoid declaring and rendering very far objects from the camera (something that could exists at runtime, but so tiny in 
the scene that you can't see it very well).

The camera position and angle can be updated using native gestures through SwiftUI:
* drag to move the camera in the scene (actually, move the scene objects around the camera...),
* pinch-in and pinch-out to zoom-in and zoom-out in the scene,
* and use the rotation gesture to rotate the camera around a particular 3D object.

Actually, the camera is a special Swift 3D object (see next section) that does not contain any mesh or texture, and
not rendered.

An example of the interaction in a very basic 3D scene that contains 5 differents objects:
1. the camera,
2. the light bubble, which lights up and lights down the objects of the scene,
3. three different 3D boxes, each with a proper texture. 

{{<youtube id="NkvK1In-568" autoplay="false">}}

### 3D objects and textures support

For 3D objects I can now render dynamically any `.obj` file for an abstract 3D object, as soon as it packaged in the app (which is not really useful for a game engine, I admit it).

The 3D object can move its own position in the scene, its euler angles, etc.
There is no limit for it.

Any 3D object derived from a `BaseObject3D` protocol, which derived itself from `ObservableObject`:

```swift
protocol BaseObject3D: ObservableObject {
    /// The position of the object in the world space
    var position: simd_float3 { get }
    /// The visualization angle of the object in the world space
    var angle: simd_float3 { get set }
    /// A name to describe the object
    var tag: String { get }
    
    /// Function to update the attributes of the BaseObject3D, each
    /// time the render function must be called
    func update()
}
```

The `BaseObject3D` is very simple, but I will make it a bit more comfy to add new attributes and 
information, like the meshes to use, the texture to load and to render, etc...

I added support for textures, and created another shader to display them correctly.  
I still have some issues to render a texture on rounded 3D objects (like a sphere), but this will be fixed 
anytime soon.

Also, I implemented a first version of a `Light` object, to light up (and down) 3D objects that belong to
the rendered scene.  
There are still big improvements to make for the `Light` object but the basic version works quite well.

![An example of the light bubble in a 3D scene](images/frame_advances_light_bubble.jpg)

### The renderer

This can be part of the [first section]({{< relref "#anchor" >}} "Better code abstraction") but the scene and the 
rendering code were really intricated and very difficult to maintain without touching of each other.  
Normally, in a modern and "generic" engine, this should not happens: the renderer should run without any internal 
knowledge of the scene to render: just vertices, colors, and textures to draw.

The first version of the renderer was not great, and difficult to maintain.  
I also made some abstractions to avoid containing the renderer in the internal `Coordinator` object of the engine view,
which should be a `NSViewRepresentable` derived object.

### Better performances

Now, running the full engine, creating and rendering a simple 3D scene is taking 24MB of RAM **maximum** and, 
of course, the rendering is done 60 frames per second.

Simplifying the code and the rendering reduced drastically the RAM taken by the app, from 100MB to 24MB.
There are new things to be done in order to increase the performances, but I am happy about the improvements for now.

The renderer takes, in average, 4ms in debug mode, so I can render this 
basic scene at 250 FPS... in **debug mode**, and for each platform.

### Natively on iOS, iPadOS, and macOS

The main drawback of my initial project was the support of multiple Apple platforms.  
At the beginning I imagined `Frame` as a game engine to run on macOS, to developer games for macOS.
However, more and more I worked on `Frame` and more and more I was convinced to run the engine on the iPad and develop games on the iPad directly could be a really nice feature.

Now, `Frame` is supported natively[^1] on iOS, iPadOS, and macOS.

The devices I test all along the changes were:
1. my mac (_macbook air m1_ - base model),
2. my iPad (_iPad 9th gen_ - base model),
3. two iPhone simulators: _iPhone SE_ (for compatibility with older iPhone models) and _iPhone 13_.

The debugger of the engine (the right part of the renderer) is not displayed correctly in the simulator, but it 
renders well on real hardware.

![Frame on macOS](images/frame_advances_plat_macOS.jpeg)

![Frame on iPadOS](images/frame_advances_plat_iPadOS.jpeg)

![Frame on iOS](images/frame_advances_plat_iOS.jpeg)

[^1]: Using the same Swift / SwiftUI code, and one click (on _Xcode_) to build and deploy 
on the platform of your choice.

### The annoying part: Xcode

I use Apple devices as a developer since more than 12 years now, and I am noticing each year the decline of Apple 
to develop appealing and stable tools for developers... like _Xcode_.

_Xcode_ is a messy developer tool, greedy for computing resources, to write code for Apple devices.

Some annoying features from _Xcode_ are still present since a few years now, like _Xcode Preview_, which does not reload 
correctly, or some random build errors (clean + build again fixes the issue), or some cache issues, and very annoying
 simulator issues to emulate the iPad app on the macbook through Catalyst (pinch-in and pinch-out works half of the time).

Those issues are present since I worked on the first version of `Frame` - almost two years ago - and I am frustrated to use
a buggy (developer) tool to develop a big project.  
I never thought I would say this but, concerning developer tools, Microsoft delivers much better developer tools for their 
platform, like _Visual Studio 2019_ or _Visual Studio 2022_...

### Conclusion and future

I made big improvements over two weeks of (hard) work on `Frame`, and I am very happy with the changes for this 
new version.  
Of course `Frame` is absolutely not ready to create a very simple game using it, but it's a new beginning for me and 
I am excited to see what I will do next on it.
I know it will take me, at least, another two years to make very simple games using it, building and delivering 
as packages on the iOS and macOS App Store. 

The list of features to implement keeps to grow:
* keyboard handler for iPadOS and macOS,
* multiple shaders support,
* better UI/UX,
* import external (not-in-the-app-bundle) 3D objects,
* integrate first 3D objects interaction(s) with the fingers and/or the mouse, 
* let the developer modify and integrate its own Metal shaders in a dedicated native text editor,
* collision support,
* Metal 2 and Metal 3 support,
* etc.

The code of `Frame` will be kept private until I am done with it, and ready for interested people 
to take a look and use it.
