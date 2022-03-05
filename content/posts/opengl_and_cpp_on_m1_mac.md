+++
title = "The case of OpenGL, in C++, on m1 mac"
date = 2022-03-05T00:00:00+02:00
draft = true
categories = ["programming", "c++", "computer graphics", "opengl"]
tags = ["programming", "c++", "computer graphics", "opengl"]
description = "COMBO!"
disqus = false
+++

Since a long time now I was interested in learning computer graphics and "do stuff" with computer graphics APIs.  
I began to work on a very simple Game engine in Swift / Metal last year but I began to switch to a Windows machine
a few months ago, and Swift / Metal is not compatible with Windows (and will never be I think).

Here, I wanted to go further in exploring OpenGL and / or Vulkan, for multi-platform GL.  
I choosed OpenGL because I am prototyping with GameMaker Studio 2, and writing shaders for it.  
Shaders in GMS2 use [GLSL](https://www.khronos.org/opengl/wiki/Core_Language_(GLSL), the _OpenGL Shading Language_, 
for OpenGL.

I am using modern C++ (C++17) for this project, mainly because I really like modern C++.

###  Why not Metal with C++ ?

Even if [Apple Metal](https://developer.apple.com/metal/) is great (actually, the API is pretty awesome), it is unfortunately specific to Apple products, and I wanted to work on something with platforms that **do not** include _only_ Apple.  
Also, Metal is still a pretty young API and the documentation is not quite right sometimes...

### And why not Vulkan?

I am not opposed to Vulkan at all: performances are great, great ideas behind, and multiplatforms. 
However, Vulkan is still young and is **way more** complicated to learn than learning OpenGL.  
For this project I wanted to do simple graphics computing before, and it seems OpenGL is still supported in a large 
range of operating systems (despite the fact that Apple deprecated the version >= 4.2 a few years ago to introduce
Metal).

## Pre-requisites

### Compile and install GLFW

In this project, we will use GLFW.

GLFW is an Open Source API for creating windows, contexts and surfaces, receiving input and events, for OpenGL, 
OpenGL ES (outch...), and Vulkan on desktop.
What does it means it that GLFW is a **helper** library that starts the OpenGL "context" (a running copy of OpenGL, tied to a window) for us, and talks to a lot of platforms / operating systems in the same way.  
The current version of GLFW, writing this article, is the third: GLFW3.

In homebrew, GLFW3 is not universal (so, not compatible with m1 macs).  
An issue on Github is still opened about this: [https://github.com/patriciogonzalezvivo/glslViewer/issues/219](https://github.com/patriciogonzalezvivo/glslViewer/issues/219).

#### Compile it yourself

To use it, you will need to compile it yourself.  
For this, you will need to install `cmake`: `brew install cmake`.

Once `cmake` has been installed, you can install your arm64 compatible version of glfw3:  
```bash
# Remove GLFW
brew uninstall glfw

# Build GLFW from source
git clone https://github.com/glfw/glfw.git && \
cd glfw && \
cmake -DCMAKE_OSX_ARCHITECTURES=arm64 . && \
make && \
sudo make install
```

You can tweak the `make install` statement in order to install the libraries in your prefered space.  
I choosed here the default location, which is `/usr/local/lib`.

#### Pass the architecture via homebrew

Another way to install it is to install it directly from source via homebrew:

```bash
brew install --from-source glfw3
```

but please to check first that `brew` does not link by default to rosetta2 (and then produce x86_64 binaries, 
which are not compatible with arm64).

## The code

In a file named `test_opengl.cpp`:

```c++
#ifdef __APPLE__
// Defined before OpenGL and GLUT includes to avoid deprecation messages
#define GL_SILENCE_DEPRECATION
#include <GLFW/glfw3.h>
#endif

#include <iostream>

int main(int argc, char **argv)
{
    if (!glfwInit()) return -1;

    std::cout << "GLFW lib has been initialized..." << std::endl;

    return 0;
}
```

The line to compile the project:

```bash
g++ helloworld.cpp -o test_opengl -std=c++17 -framework Cocoa -framework OpenGL -framework IOKit -lglfw3
```

Exciting `test_opengl` you should get a `Initialized...` message on your stdout, and a quick open/closed window.  
Initialisation worked, yay!

Also, we force the usage of modern C++, here `c++17` specs.

### About GL_SILENCE_DEPRECATION

Apple deprecated OpenGL since macOS Catalina, as Apple wanted to force their own tech: Metal.  
OpenGL has been designed 25 years ago and lacks a lot of features now (and does not match right the modern
hardware architecture: GPU pipeline has changed, unsupported multithreaded operation, etc.).  
So, compiling the project without this flag will show a ton of warnings that 
`OpenGL is now deprecated on Apple devices / platforms`.

Defining `GL_SILENCE_DEPRECATION` allows us to remove a ton of this kind of flags and focus on real problems.

To know more about why Apple deprecated OpenGL (and introduced Metal), please to take a look [at this talk from WWDC2018](https://developer.apple.com/videos/play/wwdc2018/604/).

### The case of including the .frameworks everywhere...

When checking the [online documentation of GLFW3](https://www.glfw.org/docs/latest/build_guide.html#build_link_osx), it
is required to include official Apple frameworks for Cocoa, OpenGL, and IOKit:

>  If you still wish to build manually, you need to add the required frameworks and libraries to your command-line yourself using the -l and -framework switches.

### A more complex example

Now, let's create a window, and wait for the user to close it using his prefered signal, or by pressing the Escape key:

```c++
#ifdef __APPLE__
// Defined before OpenGL and GLUT includes to avoid deprecation messages
#define GL_SILENCE_DEPRECATION
#include <GLFW/glfw3.h>
#endif

#include <iostream>

const size_t WIDTH = 640;
const size_t HEIGHT = 480;
const char* WINDOW_NAME = "Test OpenGL";

/*
 * Callback to handle the "close window" event, once the user pressed the Escape key.
 */
static void quit_callback(GLFWwindow *window, int key, int scancode, int action, int _mods)
{
    if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
        glfwSetWindowShouldClose(window, GLFW_TRUE);
}

int main(void)
{
    GLFWwindow *window;

    if (!glfwInit()) {
        std::cerr << "ERROR: could not start GLFW3" << std::endl;
        return -1; // Initialize the lib
    }

    // Minimum target is OpenGL 4.1
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 1);
    window = glfwCreateWindow(HEIGHT, WIDTH, WINDOW_NAME, NULL, NULL);
    if (!window)
    {
        std::cerr << "ERROR: could not open window with GLFW3" << std::endl;
        glfwTerminate();
        return -1;
    }
    // Close the window as soon as the Escape key has been pressed
    glfwSetKeyCallback(window, quit_callback);
    // Makes the window context current
    glfwMakeContextCurrent(window);

    const GLubyte* renderer = glGetString(GL_RENDERER);
    const GLubyte* version = glGetString(GL_VERSION);
    std::cout << "Renderer: " << renderer << std::endl;
    std::cout << "OpenGL version supported: " << version << std::endl;

    // Now we have a current OpenGL context, we can use OpenGL normally
    while (!glfwWindowShouldClose(window))
    {
        // Render
        glClear(GL_COLOR_BUFFER_BIT);
        // Swap front and back buffers
        glfwSwapBuffers(window);
        // Poll for and process events
        glfwPollEvents();
    }

    // ... here, the user closed the window
    glfwTerminate();
    return 0;
}
```

On my mac, it runs flawlessly and write on the standard output:  
```
Renderer: Apple M1
OpenGL version supported: 4.1 Metal - 76.3
```

## Improve the build system

In order to avoid copy/pasting complex command lines to build an executable, but also to improve our build system with system conditions (like if the OS is Microsoft Windows, if macOS, if the kernel is linux, etc...), we will need something more robust.  
For this, we will create our own CMakeFiles.txt file, which contains all the rules to build our own executable.

On macOS, you will need to install `cmake` (and make) using `brew`: `brew install cmake make`.

```cmake
cmake_minimum_required(VERSION 3.22 FATAL_ERROR)

project(myproject VERSION 1.0)

add_definitions(-std=c++17)
set(CXX_FLAGS "-Wall")
set(CMAKE_CXX_FLAGS, "${CXX_FLAGS}")
set(CMAKE_BUILD_TYPE Debug)

add_executable(myapp main.cpp)

find_package(glfw3 3.4 REQUIRED)
find_package(OpenGL REQUIRED)

target_include_directories(myproject PUBLIC ${OPENGL_INCLUDE_DIR})

target_link_libraries(myproject "-framework Cocoa")
target_link_libraries(myproject "-framework OpenGL")
target_link_libraries(myproject "-framework IOKit")
target_link_libraries(myproject glfw ${OPENGL_gl_LIBRARY})
```

Now, `cmake . && make` will produce a binary called `myapp`, which displays the same black window than before.  
Nice!

This setup will simplify a lot of things later, especially for conditional builds and testing (because, yes, we can execute some tests using `cmake`).
