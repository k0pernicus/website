+++
title = "Fun with data alignment"
date = 2022-11-18T22:00:00+02:00
draft = false
categories = ["programming"]
tags = ["programming", "c", "c++"]
description = "\"A lign orc rash!\""
disqus = false
+++

Whoever worked on system programming know that data alignment is very important to save memory space, 
or avoid runtime crashes casting from a specific type to another.  
After some months on porting games to console systems, I noticed how data alignment could be, sometimes, 
a source of errors and a lack of knowledge.  
I wanted to discuss about data alignment in this technical article.

**Note**: This is the technical stack I used for this article:
* MacBook Air M1 (base model, 2020),
* clang 14.0.0 (Apple version), with C++17 support.

### The problem

What is an alignment?
An alignment is an integer value representing **the number of bytes between successive addresses** at which a given struct, or object, can be allocated.

<!--
Just imagine a book's page: it has a fixed size, and can contains a maximum of `n` lines of size `x`.
They can contains a single line of size `y` (where `y <= x`), `n-1` lines of size `x`, or just no line at all...
If the size of a line is stricly lower than `x`, then there could be some padding to let the next line begin right below.  
This is a brief overview of how works the memory of a computer: the memory is composed of `n` blocks (or pages) of fixed size `x`, which is composed itself by data structures (lines), which are themselves composed by words.
-->

As an example, for basic types on a 64bits system (_we will consider the following examples running on a 64bits system_), an `int` type sizes 4 bytes, a `char` type sizes 1 byte, a `double` type size 8 bytes, etc.

But what about a `struct`?
A first approach to compute the size of a `struct` is to consider that the size of a `struct` is equals to the sum of its fields size.  
As an example, we could compute the size of the following `Example` C/C++ structure as 4 + 1 bytes, which results to 5 bytes in total. 

```c++
struct Example {
    int a; // 4 bytes
    char b; // 1 byte
}
```

However, `Example` is not 5 bytes but... **8** bytes, which is 3 bytes more than our hypothesis.  
To explain this behaviour, we will consecutively split the issue for five different structs and compare our expectations to the reality.

### How to compute struct sizes ?

#### DA01

```c++
struct DA01 {
    char a, b, c, d; // 4 * 1 byte
}
```

For `DA01` the maximum size taken by one field is **1 byte**.
We have 4 fields of 1 byte, which results to **4 bytes**. 

#### DA02

```c++
struct DA02 {
    char a; // 1 byte
    // Padding of 3 bytes
    int b;
}
```

For `DA02` the maximum size taken by one field is **4 bytes** (`b`).
We have a field of 1 byte and another of 4 bytes, which results to **8 bytes** as we have **3 padding bytes** to allocate
memory for `a`.

#### DA03

```c++
struct DA03 {
    char a; // 1 byte
    // Padding of 3 bytes
    int b; // 4 bytes
    char c, d, e; // 3 * 1 byte
    // Padding of 1 byte
}
```

For `DA03` the maximum size taken by one field is **4 bytes** (`b`).
We have a field of 1 byte and the second one of 4 bytes, and then three other 1 byte fields.
This results to **12 bytes** as we have **3 padding bytes** for `a`, and **1 padding byte** for `e`.

#### DA04

```c++
struct DA04 {
    char a, b, c, d; // 4 * 1 byte
    int e; // 4 bytes
}
```

For `DA04` the maximum size taken by one field is **4 bytes** (`e`).
We have 4 fields of 1 byte consecutively, which results to 4 bytes and **no padding** needed.
This results to **8 bytes**.

#### DA05

```c++
struct DA05 {
    char a; // 1 byte
    // Padding of 7 bytes
    float b; // 8 bytes
}
```

Finally, for `DA05`, the maximum size taken by one field is **8 bytes** (`b`).
We have a field of 1 byte at first, and we must insert paddings to get 8 bytes in total - so, a **padding of 7 bytes**.
This results to **16 bytes** in total.

#### Summary

| Struct | Supposed size (in bytes) | Real size (in bytes) |
|--------|--------------------------|----------------------|
| DA01   |      04                  |     04               |
| DA02   |      05                  |     08               |
| DA03   |      08                  |     12               |
| DA04   |      08                  |     08               |
| DA05   |      09                  |     16               |

The difference of size between `DA03` and `DA04`, which contain the same fields but using a different order, should alerts you how data alignment could be important to manage and save some space in your programs.

### The benchmarks

If you want to reproduce the results by yourself, this is the simple program I used:

```c++
#include <stdio.h>
#include <stdlib.h>

struct DA01
{
    char a, b, c, d;
};

struct DA02
{
    char a;
    int b;
};

struct DA03
{
    char a;
    int b;
    char c, d, e;
};

struct DA04
{
    char a, b, c, d;
    int e;
};

struct DA05
{
    char a;
    double b;
};

template <typename T>
void getSize(T s, const uint8_t expected_size, const char* const name)
{
    printf("* %s: expected %d bytes, got %lu bytes (alignment of %lu byte(s)) \n", name, expected_size, sizeof(s), _Alignof(s));
}

int main()
{
    {
        const DA01 s = {};
        getSize(
            s,
            sizeof(s.a) + sizeof(s.b) + sizeof(s.c) + sizeof(s.d),
            "DA01");
    }
    {
        const DA02 s = {};
        getSize(
            s,
            sizeof(s.a) + sizeof(s.b),
            "DA02");
    }
    {
        const DA03 s = {};
        getSize(
            s,
            sizeof(s.a) + sizeof(s.b) + sizeof(s.c) + sizeof(s.d) + sizeof(s.e),
            "DA03");
    }
    {
        const DA04 s = {};
        getSize(
            s,
            sizeof(s.a) + sizeof(s.b) + sizeof(s.c) + sizeof(s.d) + sizeof(s.e),
            "DA04");
    }
    {
        const DA05 s = {};
        getSize(
            s,
            sizeof(s.a) + sizeof(s.b),
            "DA05");
    }
    return 0;
}
```

If you execute the program, using any optimization (`-Oxxx`) compiler parameter you want, you would obtain the
following output:

```sh
* DA01: expected 4 bytes, got 4 bytes (alignment of 1 byte(s))
* DA02: expected 5 bytes, got 8 bytes (alignment of 4 byte(s))
* DA03: expected 8 bytes, got 12 bytes (alignment of 4 byte(s))
* DA04: expected 8 bytes, got 8 bytes (alignment of 4 byte(s))
* DA05: expected 9 bytes, got 16 bytes (alignment of 8 byte(s))
```

### Why does this matter ?

As you might know, the CPU accesses memory by a **single** memory word at a time.  
As long as the memory word size is, at least, as large as the largest data type supported, 
aligned accesses will **always** access a single memory word (excp). This may not be true for misaligned data accesses.

#### Performance

As I explained before the system tries to grab a value, and parse it, from its data alignment information.  
As an example, there is absolutely no issue for the following structure to parse it with a manual alignment of 1 byte.

```c++
struct Example {
    char a, b, c, d; // 4 fields, 4 reads.
}
```

However, let's consider the following structure
```c++
struct Example {
    int a; // 1 fields, but 4 reads if the data alignment is set to 1 byte.
}
```

As we know, for a x86/64 or arm 64 architecture, an `int` type has a size of 4 bytes.
If we specify a data alignment of 1 byte, this mean that the system will have to read **4 times** the fields in 
order to get the real value behind `a`, and make something with.  
This would be at most **4 times slower** than specifying a data alignment of 4 bytes for this `Example` structure, which could 
grab the entire value in only one read (and I did not count the data consolidation cost).

And I don't talk about misaligned continuous fields in a struct that can reduces significantively the reading and parsing
of the data stored in it, and might results to undefined behaviors.

Also, a correct alignment for your data structures may help very much the cache system.

#### Undefined behaviors

In system programming you could consider to cast one type to another... and so a structure or a basic type.

As an example, you could theoritically cast, or copy, a `uint8_t` type to a `uint32_t` type.  
However, the size is not correct, and so the data alignment.  
So, a **page fault** error could happen finishing the cast and reading the result value, a 32 bytes value from an 8 bytes aligned value... outch!

Undefined behaviors can also happen, which are most of the time much harder to find and solve than crashes.

This may happen very frequently when you try to copy a data structure from a memory place to another, which belongs to
 another hardware / chip.
This is frequent for systems with APU(s).

### Alignment specification

#### In the code

There are different ways to specify a data alignment for one, or all, data structure(s).

As an example you could specify a data alignment using pragmas:
* `#pragma pack`
* `#pragma data_align`
* `#pragma align`
* ...

You could also use the `alignas` keyword specifying the data structure, like this:
```c++
struct alignas(32) AlignAsExample
{
    float a[4]; // 4 * 8 bytes, which results to 32 bytes. Nice!
};
```

or using, for example, the `__declspec(align(#))` keyword.  
However, as the official Microsoft documentation states for MSVC:
> The compiler doesn't guarantee or attempt to preserve the alignment attribute of data during a copy or data transform operation.
You can take a look at this documentation [here](https://learn.microsoft.com/en-us/cpp/cpp/align-cpp?view=msvc-170).

This may be the principal method to use, as you may want to use a different data alignment per struct, and not for your 
entire program.

#### Via your compiler's options

Fortunately you can specify the alignment for each or all structs **in** the code, or via a compiler's setting.  
On my local `clang` compiler the option to specify the maximum alignment data size is `-fpack-struct`.

As an example, compiling the same code (from the "Benchmarks" section) with a `-fpack-struct` of **1 byte** outputs:

```sh
* DA01: expected 4 bytes, got 4 bytes (alignment of 1 bytes) 
* DA02: expected 5 bytes, got 5 bytes (alignment of 1 bytes) 
* DA03: expected 8 bytes, got 8 bytes (alignment of 1 bytes) 
* DA04: expected 8 bytes, got 8 bytes (alignment of 1 bytes) 
* DA05: expected 9 bytes, got 9 bytes (alignment of 1 bytes)
```
