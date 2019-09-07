+++
title = "Struct embedding trick to avoid duplicate code"
date = 2018-11-06T21:50:24+01:00
description = ""
draft = false
toc = false
categories = ["go", "programming trick"]
tags = ["go", "struct embedding", "interface"]
disqus = false
+++

When you are writing go code, and try to make it more flexible, most of the time you are looking for interfaces.
A go interface is a good solution to make your code more flexible, or scalable,
and is also a way to achieve polymorphism.

As the official go documentation discusses about interfaces, interfaces are "named collections of method signatures".
So, to implement an interface on different structs, you have to implement
each interface's method for a given struct.

Now, let's imagine a problem...
I want to invent an RPG game, and I need two different classes of players:
* orc,
* elf.

Those classes can be grouped in an interface called "player", for example:

```go
type player interface {
  name() string
  getInformations() string
  strike(p *player) (int, error)
  stricken(p *player) (int, error)
}

type orc struct {
  name string
  age int
  health int
  size int
}

type elf struct {
  name string
  age int
  health int
  vision int
}
```

If I want to implement this interface for `orc` and `elf`, I have to implement
`name`, `getInformations`, `strike` and `stricken` methods for both `orc` and `elf`, which is like duplicating code (except for `getInformations` as our struct contains different
usefull informations about a given Player class).

A nice trick here is to consider that the redundant code may be included in an interface, via an embedded struct (the example has been reduced to make it more understandable):
```go
type namer struct {
  name string
}

type get interface {
  getName()
}

type orc struct {
  namer // Instead of the name string type, we include directly the namer struct
  age int
  health int
  size int
}

type elf struct {
  namer // Same for the elf type
  age int
  health int
  vision int
}

// We implement only one method, for namer
func (n *namer) getName() {
  fmt.Println(return n.name)
}

func main() {
  o := orc{namer: namer{"Orc"}, age: 12, ...}
  e := elf{namer: namer{"Elf"}, age: 12, ...}
  o.getName() // It works, as an orc type contains the struct namer that respects
              // the deal with the get interface
  e.getName() // As well with the elf type :)
}
```

Using this trick, we don't have to implement twice the same method for `orc` and `elf`, and create duplication code, which is pretty nice if you have to add 10 new structs, and a lot of new methods in the
`get` interface ;)
