+++
title = "A strange strings comparison problem, in Go"
date = 2019-09-15T02:00:00+02:00
draft = false
categories = ["go"]
description = "Let's talk about untyped string constants"
tags = ["go", "typing", "implicit typing"]
disqus = false
+++

{{< figure src="/images/go_implicit_conversions_1.png" title="Thanks to https://github.com/egonelbre/gophers for this sketch" >}}

Go is known for it's strong type system, and explicit type conversion.

For example, if you have to compare two different values of different types, you will have a compiler error:

```golang
package main

import "fmt"

func main() {
   var x int = 0
   var y int32 = 0
   fmt.Printf("%+v", x == y) 
}
```

If you try to compile this code sample, you will have to deal with the following error, reached during compilation:

```bash
./main.go:8:24: invalid operation: x == y (mismatched types int and int32)
```

In order to make it work, you will have to convert `x` to `int32` (or `y` to `int` in order to not loose any precision - which is not the case in the code sample), which is called *explicit type conversion*.

This behaviour, in Go, is normal, because a well known design decision is **the absence of implicit type conversions**, like in C.

Great design, no?

Except that, even for awesome tools & rules, there *always* exist some exceptions.

## Type identity

Let's talk about the following code sample:

```golang
package main

import (
	"fmt"
)

type myString = string // Here, myString will be replaced by string

func main() {
	var x myString = "Hello"
	var y string = "Hello"
	fmt.Printf("%+v", x == y)
}
```

This code outputs "true", which is understandable because myString is a *type alias of string*.  
Some people still think that the compiler converts (implicitly) a `myString` value to a `string` value during compilation, which is not the case.  
Actually, the compiler will just replace each `myString` type name with `string` (think about a Vim's `:%s/myString/string/g` in the whole program) in order to compile the program.
So, for the bytecode, a `myString` type **is** a `string` type.

Now, if you remove the equals symbol at line 7 (when defining the type `myString`), you are defining a new type, related to a `string` one, but that is **explicitly different** than a `string` for the type system.

In this case, the code will not compile and the error will be almost the same as before:

```bash
./main.go:12:22: invalid operation: x == y (mismatched types myString and string)
```

Now, let's try something a bit different...

If I replace the line 

```golang
var y string = "Hello"
```

with 

```golang
var y = "Hello"
```

the problem still remains the same.  
Using type inference feature, the compiler will understand that `"Hello"` is a string, and will type it implicitly.

So, the following program must be the same than the previous version, right?

```golang
package main

import "fmt"

type myString string

func main() {
	var x myString = "Hello"
	fmt.Printf("%+v", x == "Hello")
}
```

Here, we did not declared `y` and instanciated it with `Hello`.
We let the compiler do it for us.

Let's run it to expect the same error and the program... suceeded !?  
And outputs `"true"`.

But... why?

## Untyped string constants

The first idea here is to ask ourselves if the compiler does not instantiates by itself `Hello` as a `myString` type, which is false:

```golang
fmt.Printf("%+v - type of 'Hello' is '%+v'\n", x == "Hello", "Hello")
```

outputs `true - type of 'Hello' is string`.

The second idea is to ask themselves if `"Hello"` is a variable, and not a constant value...

[This blog post](https://blog.golang.org/constants) from Rob Pike about constants in Go, explains clearly that **all unassigned text between double quotes are considered as constant values**.

So, the previous code is (almost) exactly like the following one:

```golang
package main

import "fmt"

type myString string

const y = "Hello"

func main() {
	var x myString = "Hello"
	fmt.Printf("%+v", x == y)
}
```

which still outputs `true`.

As Rob Pike explained in the blog post:

> This is an *untyped string constant* [...]. Yes, it's a string, but it's not a Go value of type *string*. [...] An untyped constant is **just a value, one not yet given a defined type that would force it to obey the strict rules that prevent combining differently typed values**.

Later in the same blog post:

> These untyped string constants are strings, of course, so they can only be used where a string is allowed, but they do not have type *string*. 

So, following those explanations, we can understand that if we define constants with concrete types, and not let the compiler infer those, we will not having this issue anymore.    
In this case, if we change the definition of `y` to this:

```golang
const y string = "Hello"
```

the comparison will fail at compile time, with the same compilation error message than the previous time(s).

{{< figure src="/images/go_implicit_conversions_2.png" title="Thanks to https://github.com/egonelbre/gophers for this sketch" >}}

## But... why do we need untyped string constants?

At first, it seems that untyped constants was a bad design move from the creators of Go, because you can use them to escape from Go's strong type system, which allows you to make some comparison mistakes if you don't pay enough attention.

**Spoiler**: it's not a bad move, it's a clever one.

Of course, the previous examples paid attention to untyped string constants, but you can obviously hack with untyped int constants, untyped float constants, and so on...

So, why do Go creators introduced the notion of untyped string constants?

Again, as Rob Pike explained in the blog post:

> It is this notion of an untyped constant that makes it possible for us to use constants in Go with great freedom.

Thoses statements prove actually two things:

1. untyped constants move is a **design decision for more flexibility** for Go;
2. and **typed constants don't use the same type system as variables**, in Go.

This [great article from ArdanLabs](https://www.ardanlabs.com/blog/2014/04/introduction-to-numeric-constants-in-go.html) provides a practical example to explain this Go feature in a numerical issue (look at section "One Practical Example").

## Conclusion

Inference of constants values **is different** than inference of variables values, as typed constants don't use the same type system as variables.

Untypes constants provide flexibility, and escape from Go's strong type system.

Be careful about untyped constant - they might be useful in certain cases, but I strongly recommend to restrict first the type of your constants, and to remove them later if you really need more flexibility in your code.
