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

The goal of this (first?) part is to draw the famous RGB triangle, on the m1 mac, using C++17.

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

## The shaders

I will use specific features of C++17, like `optional`, in order to improve the code and avoid to throw errors / exceptions everywhere...

```c++
/* shader_utils.h */
#ifndef _SHADER_UTILS_H
#define _SHADER_UTILS_H

#include <optional>

namespace ShaderUtils
{

    enum Type
    {
        FRAGMENT_SHADER_TYPE,
        VERTEX_SHADER_TYPE,
    };

    struct Program
    {

    private:
        /**
         * @brief The vertex shader ID, as an optional
         */
        std::optional<unsigned int> vertexShader = std::nullopt;

        /**
         * @brief The fragment shader ID, as an optional
         */
        std::optional<unsigned int> fragmentShader = std::nullopt;

        /**
         * @brief The GPU program ID (or shader ID), as an optional
         */
        std::optional<unsigned int> program = std::nullopt;

        /**
         * @brief Stores if the shader has been registered or not
         */
        bool registered = false;

    public:
        /**
         * @brief Constructor
         */
        Program();

        /**
         * @brief Destructor
         */
        ~Program();

        /**
         * @brief Register a shader
         *
         * @param shader_type The type: fragment or vertex
         * @param shader_source The source code as a string
         * @return true The shader has been successfully registered
         * @return false The shader has not been registered - error is logged
         */
        bool registerShader(const Type shader_type, const char *shader_source);

        /**
         * @brief Register the GPU program (or shader), after compilation
         * of the fragment and vertex shaders.
         *
         * @return true The shader has been successfully registered
         * @return false The shader has not been successfully registered - error is logged
         */
        bool registerProgram();

        /**
         * @brief Returns the GPU program ID object, as optional
         *
         * @return std::optional<unsigned int>
         */
        std::optional<unsigned int> getProgram() const;

        /**
         * @brief Returns if the GPU program object has been registered or not
         */
        bool programIsRegistered() const;
    };

}

#endif /* SHADER_UTILS_H */
```

For the implementation:

```c++
/* shader_utils.cpp */
#ifdef __APPLE__
/* Defined before OpenGL and GLUT includes to avoid deprecation messages */
#define GL_SILENCE_DEPRECATION
#include <GLFW/glfw3.h>
#endif

#include "logs.h" // Simple macros for DEBUG, INFO, ... messages
#include "shader_utils.h"
#include <optional>
#include <iostream>

ShaderUtils::Program::Program() {}

ShaderUtils::Program::~Program()
{
    if (vertexShader.has_value())
        glDeleteShader(vertexShader.value());
    if (vertexShader.has_value())
        glDeleteShader(fragmentShader.value());
    if (registered && program.has_value())
        glDeleteProgram(program.value());
}

bool ShaderUtils::Program::registerShader(const ShaderUtils::Type shader_type, const char *shader_source)
{
    int success = {};
    char errorMessage[1024] = {};

    if (shader_type == ShaderUtils::Type::VERTEX_SHADER_TYPE)
    {
        vertexShader = glCreateShader(GL_VERTEX_SHADER);
        const unsigned int vertexShaderValue = vertexShader.value();
        // Now, pass the shaders
        glShaderSource(vertexShaderValue, 1, &shader_source, NULL);
        // And now, compile them
        glCompileShader(vertexShaderValue);

        glGetShaderiv(vertexShaderValue, GL_COMPILE_STATUS, &success);
        if (!success)
        {
            glGetShaderInfoLog(vertexShaderValue, 1024, NULL, errorMessage);
            error("Vertex shader compilation error: " << errorMessage);
            return false;
        }
    }
    else
    {
        fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
        const unsigned int fragmentShaderValue = fragmentShader.value();
        // Now, pass the shaders
        glShaderSource(fragmentShaderValue, 1, &shader_source, NULL);
        // And now, compile them
        glCompileShader(fragmentShaderValue);

        glGetShaderiv(fragmentShaderValue, GL_COMPILE_STATUS, &success);
        if (!success)
        {
            glGetShaderInfoLog(fragmentShaderValue, 1024, NULL, errorMessage);
            error("Fragment shader compilation error: " << errorMessage);
            return false;
        }
    }
    return true;
}

bool ShaderUtils::Program::registerProgram()
{
    if (registered)
    {
        error("program is already registered");
        return false;
    }
    if (!vertexShader.has_value() || !fragmentShader.has_value())
    {
        error("cannot compile program without vertex and fragment shaders");
        return false;
    }
    int success = {};
    char errorMessage[1024] = {};
    const unsigned int vertexShaderValue = vertexShader.value();
    const unsigned int fragmentShaderValue = fragmentShader.value();

    program = glCreateProgram();
    const unsigned int programValue = program.value();

    glAttachShader(programValue, vertexShaderValue);
    glAttachShader(programValue, fragmentShaderValue);
    glLinkProgram(programValue);

    glGetProgramiv(programValue, GL_LINK_STATUS, &success);
    if (!success)
    {
        glGetProgramInfoLog(programValue, 1024, NULL, errorMessage);
        error("Shader linking error: " << errorMessage);
        return false;
    }

    // We can now delete our vertex and fragment shaders
    glDeleteShader(vertexShaderValue);
    glDeleteShader(fragmentShaderValue);
    glUseProgram(programValue);
    registered = true;

    return true;
}

std::optional<unsigned int> ShaderUtils::Program::getProgram() const
{
    return program;
}

bool ShaderUtils::Program::programIsRegistered() const
{
    return registered;
}
```

Using those helpers, our shaders registration take a few lines of code:

```c++
// in main.cpp, just before the `while` loop
    ...
    auto shader_utils = ShaderUtils::Program{};

    if (!shader_utils.registerShader(ShaderUtils::Type::VERTEX_SHADER_TYPE, basicVertexShaderSource))
    {
        glfwTerminate();
        return -1;
    }

    if (!shader_utils.registerShader(ShaderUtils::Type::FRAGMENT_SHADER_TYPE, basicFragmentShaderSource))
    {
        glfwTerminate();
        return -1;
    }

    if (!shader_utils.registerProgram())
    {
        glfwTerminate();
        return -1;
    }
    ...
```

As our current target is the basic RGB triangle, our shaders are very simple.

For the vertex shader source:

```vert
#version 410 core
layout (location = 0) in vec3 vertexPosition;
layout (location = 1) in vec3 vertexColor;
layout (location = 0) out vec3 fragmentColor;

void main() {
    gl_Position = vec4(vertexPosition, 1.0); // `w` is used for perspective
    fragmentColor = vertexColor;
}
```

, and the fragment shader source:

```frag
#version 410 core
layout (location = 0) in vec3 fragmentColor;
out vec4 finalColor;

void main() {
    finalColor = vec4(fragmentColor, 1.0);
}
```

For this, you can easily write down those shaders in dedicated files, and load them interactively via the command line.  
For this exercise, I am writing those shaders down into two dedicated strings, and use them non-interactively:

```c++
    ...
    const char *basicVertexShaderSource = "#version 410 core\n"
                                        "layout (location = 0) in vec3 vertexPosition;\n"
                                        "layout (location = 1) in vec3 vertexColor;\n"
                                        "layout (location = 0) out vec3 fragmentColor;\n"
                                        "void main()\n"
                                        "{\n"
                                        "    gl_Position = vec4(vertexPosition, 1.0);\n" // `w` is used for perspective
                                        "    fragmentColor = vertexColor;\n"
                                        "}\0";

    const char *basicFragmentShaderSource = "#version 410 core\n"
                                            "layout (location = 0) in vec3 fragmentColor;\n"
                                            "out vec4 finalColor;\n"
                                            "void main()\n"
                                            "{\n"
                                            "    finalColor = vec4(fragmentColor, 1.0);\n"
                                            "}\0";
    ...
```

## A story about vertices...

We have our main program, we have our shaders... now, let's draw the famous triangle!

The array will contains 6 elements, with the following labels `x, y, z, r, g, b`.  
The offset for a position will be `0`, and the offset for a color will be `3`.

I wrote down the basic specification in a specific library component:

```c++
// in maths_utils.h
#ifndef _MATHS_UTILS_H
#define _MATHS_UTILS_H

namespace MathsUtils
{

    const unsigned int VERTEX_ELEMENTS_NB = 6;

    /**
     * @brief Reassemble all elements of a vertex in a dedicated type
     * Contains, in the following order: x, y, z (position, in 3D), r, g, b (RGB colors)
     */
    typedef float vertex[VERTEX_ELEMENTS_NB];

    ... // Some uninteresting functions for this exercise

    /**
     * @brief Returns the number of individual vertex arrays inside a vertices array
     *
     * @param vertices - an array of vertex
     * @return The number of the individual vertex arrays
     */
    const unsigned int getNbVertex(const vertex vertices[]);

    /**
     * @brief Sums and returns the number of individual elements stored in the vertices array
     *
     * @param vertices - an array of vertex
     * @return The sum of all individual elements stored in the vertices array
     */
    const unsigned int getNbElements(const vertex vertices[]);
}

#endif /* _MATHS_UTILS_H */
```

The implementation is straightforward:

```c++
// in maths_utils.cpp
#include "maths_utils.h"

const unsigned int MathsUtils::getNbVertex(const MathsUtils::vertex vertices[])
{
    return (unsigned int)(sizeof(*vertices) / MathsUtils::VERTEX_ELEMENTS_NB);
}

const unsigned int MathsUtils::getNbElements(const MathsUtils::vertex vertices[])
{
    return MathsUtils::getNbVertex(vertices) * VERTEX_ELEMENTS_NB;
}
```

So, we end with the following code:

```c++
    ...
    // Specify position attribute - 0 as offset
    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, MathsUtils::VERTEX_ELEMENTS_NB * sizeof(float), (GLvoid *)0);
    glEnableVertexAttribArray(0);

    // Specify color attribute - 3 as offset
    glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, MathsUtils::VERTEX_ELEMENTS_NB * sizeof(float), (GLvoid *)(3 * sizeof(float)));
    glEnableVertexAttribArray(1);

    glBindBuffer(GL_ARRAY_BUFFER, 0);
    ...
```

Now, we have to explicit say that:
* we want to use the GPU program,
* bind the vertex array,
* and draw the triangle.

In the while loop, we could just write:

```c++
    ...
    while (!glfwWindowShouldClose(window))
    {
        // Render
        glClearColor(0.5, 0.5, 0.5, 1.0);
        glClear(GL_COLOR_BUFFER_BIT);
        glUseProgram(shader_utils.getProgram().value());
        glBindVertexArray(VAO);
        glDrawArrays(GL_TRIANGLES, 0, 3);
        // Poll for and process events
        glfwPollEvents();
        // Swap front and back buffers
        glfwSwapBuffers(window);
    }
    ...
```

and, do not forget the delete the buffer right after:

```c++
    ...
    glDeleteBuffers(1, &VBO);
    glDeleteVertexArrays(1, &VAO);
    glfwTerminate();
    return 0;
}
```

Then, hit `cmake . && make`, launch the program and... tadaaaa!

{{< figure src="/images/rgb_opengl_triangle_m1.png" title="An RGB triangle, built with OpenGL 4.1 on a macbook air m1" scale="50%" >}}

The full code is available at: [https://github.com/k0pernicus/opengl-explorer](https://github.com/k0pernicus/opengl-explorer), commit `0e311828fb34b272809432254307e8561e951335`:

```bash
git clone https://github.com/k0pernicus/opengl-explorer
cd opengl-explorer
git checkout -b rgb_triangle 0e311828fb34b272809432254307e8561e951335
```

The m1 mac is an awesome machine, but lacks of continuity for OpenGL (especially as OpenGL 4.5 >= introduces a lot of new exciting features that make the code less verbose).  
As OpenGL 4.1 is still robust and as Apple did not completely removed OpenGL from the stack, it is still interesting to draw things with OpenGL... but Vulkan may be interesting at this point.

I don't know if I will continue on this kind of exercises, and go further on trasnformations for example... but do not hesitate to give me some feedbacks (by emails) if you want to know more about X or Y.
