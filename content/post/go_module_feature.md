+++
title = "`go mod`: manage all your dep as a single unit"
date = 2018-11-03T20:04:54+01:00
description = "A simple example of how to use Go modules"
draft = false
toc = false
categories = ["go", "module", "package software"]
tags = ["go", "software development"]
+++

Generally, when you want to package your go app, you are creating different 
packages, inside the same project.  
The problem is, if you want to use a single internal package, you simply can't, 
because you have to import the *entire* package in order to user a single and
very simple feature inside.

But, sometimes, you don't want to use a single extenal package.
Indeed, you just want to use a certain number of packages to do a task, and all compatible between them.  
And this become more complex...  
Also, do not forget that, when importing a Go project using `go get`, it will
not check the package dependencies, or the package versions...  
So, you need an external tool to define and *save* the package versions your
Go app is using.

Since Go 1.11 (as a beta version), a new feature pointed to the end of his nose
in order to help you in creating, and managing, easily a group of packages
as a single unit: the Go modules.  
Also, [vendoring will be "deprecated" soon to give way to Go modules soon](https://www.reddit.com/r/golang/comments/9ai79z/correct_usage_of_go_modules_vendor_still_connects/).

Let's talk about this feature.

## The context

Go has a lot of external tools in order to manage dependencies: vendors, go dep... 
but those tools are not very efficient, and raised a lot of problems (and usability 
exception) since their public availability.  
The needs, for Gophers, was to build an internal command in order to fetch and install specific dependencies in order to build a Go package.

At this time, Go modules was born.

Go modules appeared in Go 1.11 as a beta feature, and has been released as a 
stable feature in Go 1.12.

In order to preserve the consistency and the maintanability of Go projects,
`go mod` will check the `Gopkg.lock`, if it exists, in order to retrieve 
very fast the required libraries for your Go app.

## The concept

In order to define the different "components" of the Go module, a `go.mod` file
is listing other required modules, at the root of the defined module:
```
/myGoModule
  go.mod
  main.go
```

If `main.go` is using `bar` and `foo` specific Go packages, `go.mod` will list
those packages in order to fetch, download, and build them.

For this example, in `go.mod`:
```
module github.com/unnamedUser/myGoModule // Define the package name

require (
  github.com/unnamedUser/bar v0.0.1 // I want the 0.0.1 version of bar...
  github.com/unnamedUser/foo v0.0.3 // ...and the specific 0.0.3 of foo!
)
```

You can observe here that:
1. the module is named as your Go package;
2. go packages have been imported automatically (last version) in the `require` code block;
3. indirect go packages are explicited (in order to verify the entier Go app later);
4. the package version of each Go package is setted (and can be modified **but you 
have to respect the consistancy of the dependencies versions**).

So, if another user is trying to import/download/build your app, `go mod` will
import the specific versions of your in-app packages.  
Very nice!

## A simple example

To illustrate how to use `go mod`, we will make a very simple exercise.

Here, we want to create an HTTP server, that consumes a single route, `/proverb`,
which returns a Go proverb as a string.
In order To do that, we will import the following packages:
* `quote`, in order to retrieve a Go proverb,
* `color`, because we see the life in TRUUUUUUUUE COLOOOOOR...

Ok, let's create our Go package.

```bash
> mkdir -p /tmp/go_tests/httproverb
> cd /tmp/go_tests/httproverb
```

First, let's initialize the 
`go.mod` file:  
``` bash
> go mod init github.com/k0pernicus/httproverb
go: creating new go.mod: module github.com/k0pernicus/hello
```

Using this command, go will create and initialize a `go.mod` file that contains
only the name of the module, as we did not create our Go app.

Let's create the main application, in `httproverb.go`:   
```go
package main

import (
  "fmt"
  "log"
  "net/http"
  "rsc.io/quote"
  "fatih/color"
)

func hello(w http.ResponseWriter, r *http.Request) {
  fmt.Fprintf(w, "<h1>Hello user</h1><p>Tip: Why don't you try /proverb ? :)</p>")
}

func proverbHandler(w http.ResponseWriter, r *http.Request) {
  goProverb := quote.Go()
  // Log the proverb (just to use color from fatih...)
  color.Green("This is my new proverb: ’%s’", goProverb)
  // Returned from the server
  fmt.Fprintf(w, goProverb)
}

func main() {
  http.HandleFunc("/", hello)
  http.HandleFunc("/proverb", proverbHandler)
  log.Fatal(http.ListenAndServe(":8080", nil))
}
```

The architecture of our project is very simple because it contains only two files:
`go.mod` and `httproverb.go`.

Now we written the code of our Go app, let's build it!
```bash
> go build
go: finding github.com/fatih/color v1.7.0
go: downloading github.com/fatih/color v1.7.0
package main
go: finding github.com/mattn/go-isatty v0.0.4
go: finding github.com/mattn/go-colorable v0.0.9
go: downloading github.com/mattn/go-isatty v0.0.4
go: downloading github.com/mattn/go-colorable v0.0.9
# github.com/k0pernicus/hello
```

If you want to see where are stored those dependencies, you can list the files in
the go mod cache, in `$GOPATH/pkg/mod/`.  
In you are wondering why `go mod` is using is own cache, you can check the answer 
from Ross Cox in [this article from Dave Cheney](https://dave.cheney.net/2018/07/14/taking-go-modules-for-a-spin).

After playing a bit with the Go app, check the `go.mod` app:  
```bash
module github.com/k0pernicus/httproverb

require (
	github.com/fatih/color v1.7.0
	github.com/mattn/go-colorable v0.0.9 // indirect
	github.com/mattn/go-isatty v0.0.4 // indirect
	rsc.io/quote v1.5.2
)
```

Great!  
Go has successfully listed our dependencies into the `go.mod` file, without any help.

Go contains a lot of subcommands in order to manage easily and properly the
dependencies, like:
* `graph` (print module requirement graph),
* `tidy` (remove unused modules),
* `why` (explain why packages or modules are needed for this Go app),
* and many others (`go mod help`).

## To go further

If you want to take a look at a better/live example, please check a look at
the [httpstat](https://github.com/davecheney/httpstat) package from 
[Dave Cheney](https://dave.cheney.net/), a very famous Gopher.  
Otherwise, if you want to know more about Go modules, please to read
[the official proposal](https://go.googlesource.com/proposal/+/master/design/24301-versioned-go.md) from the Go team.
