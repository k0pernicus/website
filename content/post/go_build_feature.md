+++
title = "The `go build` constraints"
date = 2018-10-26T18:56:18+02:00
description = "Or how to make your program OS agnostic... at compile time"
draft = false
toc = false
categories = ["go", "feature", "compilation"]
tags = ["go", "build", "feature", "os"]
+++

<center>
<img src="/bpg/go_build_constraint.bpg" alt="Go Build Illustration" width="400">
<p>Credits to [@ashleymcnamara](https://twitter.com/ashleymcnamara).</p>
</center>

Recently, I started to use [termui](https://github.com/gizak/termui) in order to build and run
a modular dashboard on the terminal.  
This dashboard will display my daily todo list, some informations about my git projects/repositories,
some daily news, etc...

I use everyday: a macbook pro, and a GNU/Linux laptop - so, two different operating systems.

Even if macOS and GNU/Linux share similar parts, those systems are strictly differents, and 
I will have to compile the dashboard for each one.  
No problem with cross-compilation, Go can handle that very quickly and simply via a compiler built-in option.  
Nice!

However, I quicky realized that I will have a trouble in specifying interruption handler path, for each of those 
operating systems.

Indeed, for termui, Linux can simply handle the letter key `q` to quit the program manually.
For macOS, unfortunately, we must to complete the command using the full path to keyboard command `/sys/kbd/q`.

As a result, the full path does not works on my GNU/Linux system, and vice-versa.

Multiple solutions can be used in order to solve this problem, but my solution here is to define 
a constant for each keyboard command, for GNU/Linux and macOS, and let the compiler decide which one to 
use according to the current operating system.  
At this moment, I discovered the **build constraints** feature of Go.

## What is `build` ?

`build` is a Go package, available in the standard library.  
The main goal of the package is to gather information about Go packages, and includes several options and 
features in order to do that.

## How a Go project is organized?

Go is a compiled programming language.  
The Go compiler takes as input a source code, and return binary (or machine) code (after several internal 
processes) for the architecture you specified (or you are using).

In order to produce the binary code, the compiler needs several iterations to build some parts of the projects, as 
binary objects, to reassemble them later in order to produce the main binary.

Here, in order to use specific code for a given hardware architecture, we want to introduce *compiler conditions*.  
For the compiler, those conditions will permit to build specific binary objects according to a given architecture, and produce
a final binary code fully compatible with the given architecture.

Those compiler conditions are presents in Go under the name of "build tags", or "build constraints".

## A concrete example

This is the tree of our Go project:
```bash
go/
  src/
    myproject/
      kbd_linux.go
      kbd_macos.go
      main.go
  bin/
    ./myproject
```

For those directives, or compiler conditions, Go does not have a preprocessor, a macro system, or a C-like #define declaration.
Instead, Go is using a naming system convention, supported by the go tool.

So, compiler conditions will be defined as comments, in the first lines of the go file.

We will include the build constraints in `kbd_linux.go` and `kdb_macos.go` for, respectively, GNU/Linux and macOS, 
and let the compiler decides which one to use using the `// +build` directive.

Let's build our specific GNU/Linux build tag file.  
In `kbd_linux.go`, we can write:  
```golang
// +build linux

package main

// Route correctly for Linux machines
const QUIT = "q"
```

So, the `QUIT` keyboard event path will point to the simple path "q".

For macOS, in `kbd_darwin.go`, we can write:  
```golang
// +build darwin

package main

// Route correctly for darwin machines
const QUIT = "/sys/kbd/q"
```

Here, we define the same constant `QUIT` but we point it to another kernel path.

You can notice we *defined* the same constant in each file.  
This is not an issue at all because the compiler will choose one of those files to
build the binary, at compile time.  
So, `QUIT` will *never* be erased or overrided at any time by the compiler.

Finally, in `main.go`:  
```golang
package main

import (
  "fmt"
  ui "github.com/gizak/termui"
)

func main() {
  err := ui.Init()
  if err != nil {
    panic(err)
  }
  defer ui.Close()

  p := ui.NewPar(":PRESS q TO QUIT")
  p.Height = 3
  p.Width = 50
  p.TextFgColor = ui.ColorWhite
  p.BorderLabel = "Text Box"
  p.BorderFg = ui.ColorCyan

  ui.Render(p)

  ui.Handle("q", func(ui.Event) {
    // At compilation time,
    // QUIT will be replaced by the value
    // corresponding of the correct source code file
    // according to the current operating system.
    fmt.Println(QUIT)
    ui.StopLoop()
  })

  ui.Loop()

}
```

As the compiler will choose one of the `kbd_<>.go` file, it will create an internal
package for the `main.go` source code file, and substitutes the `QUIT` constant to the `kbd_<>.go` one.

To check which go files will be included in the binary, you can use `go list` to get the names of the files that would be compiled: `go list -f '{{.GoFiles}}'`.

Simple and with a negligible loss of performance in the compilation, this solution
permits to specify architecture conditions to build a specific binary, while ensuring
a minimum of security in its Go app.

## To Go further

* [Package build - online official documentation](https://golang.org/pkg/go/build/)  
* [justforfunc: Versions, build constraints, and ldflags](https://www.youtube.com/watch?v=-XSlev-d7UY)
