+++
author = ""
date = "2017-09-19T00:00:00-00:00"
description = ""
tags = ["rust", "programming", "skills"]
title = "See you, Rust!"

+++

**During the summer of 2014**, I tried to learn the C++ programming language - primarily because I wanted to deal with OOP, using something other than Java (which was, unfortunately, the main programming language in my University). At this time, I used mainly two paradigms: imperative programming (with C), and functional programming (with OCaml) - it's not a coincidence if C and OCaml were my two favorite programming languages.  I borrowed the official book from Bjarne Stroustrup in my library (C++11) and, while reading the book, I became fascinated by two things: firstly by the great trade-off performance/abstraction provided by the language, which I missed with C; and secondly by the complexity of this programming language.
Actually, the real complexity of C++ striked me when I took a look, weeks after, at the third edition of the great ["_Effective C++_"](http://www.aristeia.com/books.html), from Scott Meyers. Reading some examples in the book, I realized that this programming language was not for me, because I used to think that I couldn't have any fun coding with.

Discussing with engineers when I contributed to FirefoxOS, they told me that Mozilla was working on a new programming language, with the aim to replace C++. Its name: **Rust**.

I gave up Rust a first time, **beginning of 2015**, when I tried to program a library to have fun with genetic algorithms.
Coding with Rust was fun (sometimes harshest due to the lack of documentation, or "stabilized" libraries), but it was not fun when, a few days after coding the library, the new version of Rust was released... and a lot of changes (even types/functions names) has been made.  
I was very disappointed with those changes, and I dropped.
However, I have promised myself to take a look at this language, as soon that it reached his first stable version.

Then in **May 2015**, came the first stable version of Rust.
I was excited, but did not have any real project to work with...
Three months after, I began my final year of my Master degree, in optimization algorithms, big data and machine learning.
As a "final" project, my collegues and I, we had to program a complex software to predict and manage accurately flight plans, in order to reduce the cost of the plane on the tarmac after landing, and to increase the profit of the airport.  
For this exercise, we hadn't get the chance to use third-parties libraries, only the standard library of the programming language we chosed.  
The vast majority of my colleagues choosed Java, and the others Python.

Java and Python for all.  
Except five poor guys.  
Two guys choosed Scala.  
One choosed C++.  
Another choosed C.  
And the last one choosed Rust.

I was this last guy.  
And, **Hell, it was really fun.**

So, **during three months**, I cranked out code to understand the complex mecanisms of Rust, in order to create the most efficient code as possible.  
Concerning performance, I came as the second - the winner was the C++ guy (using a 100% templates code).
But I was the winner, by far, concenring code exoticism.

To me, programming with Rust was more like a challenge, it was like a sentiment of liberation. I found the language I enjoyed the most to program with.  
From this time, I programed a lot of tools and libraries in Rust, most are in public access on my Github: a Spotlight clone for GNOME, a primitive operating system (_arOS_, for "another rust Operating System"), a manageable download accelerator (_Snatch_), a _Xamarin_-like to assess the energy consumption of Android smartphones (my final internship, at Université du Québec à Montréal), etc., and I contributed to a few Rust projects like _Cargo_, _Xargo_, _RedoxOS_, and others.  
Again, it was a lot of fun.

Thanks to those projects, I improved myself in Rust, and had the chance to be the principal reviewer of ["_Mastering Rust_"](https://www.packtpub.com/application-development/mastering-rust), a book from [Vesa Kaihlavirta](https://twitter.com/vegai).

**During my final internship, between February and August 2016**, I discovered two other programming languages: [Golang](https://golang.org) (thanks to Google Montréal, which promoted the language) and [Nim](https://nim-lang.org) (thanks to this [Reddit post](https://news.ycombinator.com/item?id=9050114)).  
Each language has it's own advantages and disadvantages - the tooling and the easy-going usage for Golang vs the lack of abstraction; the quality of the standard library and the features for Nim vs the lack of a stable version and a small community of programmers.  
I used Golang a few weeks, but I didn't found this language very funny to use... I still watch the [YouTube videos by Francesc Campoy](https://www.youtube.com/channel/UC_BzFbxG2za3bp5NRRRXJSw), which are really informative.
Thank you, Francesc!

**April 2017**, big change in my life.  
I started a PhD in Luxembourg, for three years.
As I began to work on a project which involves several people from a big company, I evolved professionaly to create POCs as faster as I can. No time to spend writing complex code, or implementing and maintaining a library - POCs became my life.
The learning curve for Rust stays high, and it will not easy to look for Rust interns, or interns which are willing to learn Rust for a three months job.
Also, unfortunately, the more the language evolves, and the more it become complex...
I understand that the community must build the perfect eco-system to help Rust developers developing their complex and low-level fantastic applications, but I think that the next big thing of Rust is to work on the learning curve.  

Since the beginning of my PhD, I wrote a lot of Python code (~80% of my time), and I began to work concretly with Nim since I began to read the great book ["_Nim in Action_"](https://www.manning.com/books/nim-in-action), by [Dominik Picheta](https://twitter.com/d0m96).  
I tried to contribute to [_rusty-machine_](https://github.com/AtheMathmo/rusty-machine) recently, but it's really difficult to contribute to a library when the principal maintainer is not available...  
Note that I don't blame the developer of _rusty-machine_ here, he made an awesome work in developing a fast and easy-to-use machine learning library.  
Also, I planned to write a library to parse and extract informations from PDF files, but I found less difficult to do it with Nim, since I am not friendly with Rust parsers like [_Nom_](https://github.com/Geal/nom) (actually, I still prefer [_Pom_](https://github.com/J-F-Liu/pom) than _Nom_).

**Today, September 18th**, I do not plan to use Rust anymore in my PhD thesis.  
I found Python and Nim really useful to develop rapidly POCs, to make several experiments on data, and to replace critical Python piece of code with Nim code - especialy for I/O and CPU bounds, or to deal with concurrency and parallel programming more efficiently.  

It's not a _good-bye_, just a _see you later_ "statement".  
I still hope that Rust will reduce its complexity, and that the community will stay as awesome as it is now :-)  

So, see you later Rust.  
I had a lot of fun with you!

_Thanks a lot to Geoffrey, my friend, for reading this blog post before it was published._
