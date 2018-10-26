+++
author = "Antonin"
date = "2016-03-30T01:40:25-04:00"
description = "Brainfuck > ALL"
tags = []
title = "Choose the one"

+++

<center>
<img src="/bpg/choose_the_one_cover.bpg" alt="Cover" width="500" height="500">
</center>

## Abstract

We, humans, need to communicate to better understand each other, and what we can do together.
This communication must be mutual.
Thanks to this one, we can better understand what we want to do, what we have to do and what to be done.
The communication can be start between 2 persons, or more, using a dialect called **language**.
The language contains **words**, made with at least one **symbol** (basically, a character).

It's the same thing with a computer.
A computer can't understand what you want if he can't understand you.
A computer only understand 2 symbols: 0 and 1, and it's impossible to write a program with these 2 symbols only.
So, we use an understandable language which can be translated into **bitcode** for the computer.
This understandable language is called a **programming language**.

As I said, **the communication must be mutual**.
When you have to communicate with a computer, you have to type a set of instructions on your keyboard, composed with words, and the machine will understand what you mean **if** you had composed correct instructions (checked by a **compiler** or an **interpreter**).
After that, the computer will compute and return to you the result, in your language (it can be a set of strings, integers or a simple message which tells you "It's ok God, I just finished the process!"). So, you have to choose the perfect programming language to have fun, while remaining perfectly understandable to the computer.
A question that developers (commonly young developpers) ask themselves is: "What is the perfect programming language to use in daily life?".

I will not fall into the trap of why I use this programming language instead of that other one (sorry trolls), but I will try to explain clearly how to **make your choice**.
At the end of the article, you will understand that there is no _best programming language to use in daily life_.

First of all, you have to find a good language to communicate with the person you want to discuss.
This language can be use by both of you, or only by yourself if the other people understand what you're saying but are not enough confortable to talk to you with the same one.
For programming, it's the same thing, except you can choose the language to _talk to your computer_.  
So, we can say you're a God, right? And you have to choose a programming language **adapted** to your task.
In computer science, the adaptation is one of the most important thing to think about.
So, ask yourself this question: "Does I really need to choose this programming language to do this, or can I choose an other one more pleasant and **efficient**?".

## The quest for efficiency

Efficiency can be interpreted in different ways:

*   the **platform** you want to program (generally your OS or your computer architecture),
*   the program's **goal**,
*   **libraries** you have to use to build it (this part is really important),
*   your ability to write code with a programming language you already **know**,
*   the programming paradigm the most **adapted** for your task,
*   the global **performance** of the programming language (you can visit the website [The Computer Language Benchmarks Game](http://benchmarksgame.alioth.debian.org/) if you are interested in performance),
*   and other subways like the time you have to program it...

This is a lot of tasks you have to take in care, but this list contains some basic things a computer developer have to ask himself before to code. Don't be afraid. After practice, answers will obviously come to you.

## The platform

Generally, modern programming languages are independant to the platform you're programming.
But, you can be drift along to choose a programming language instead of an other, perhaps more convenient for the platform you want to launch your computer program.
Today, there exists more and more different operating systems and system architectures that it's really hard to make a good choice, and to be the most general as possible.  
A good multi-platform programming language is typically _Java_, because the program is transformed into **bytecode** while he's compiling, which can be run into a platform which have the specific _Java Virtual Machine_ you programming for.
It's can be awful to develop a program on a Linux distribution, to deploy it on [Github](https://github.com), and to receive some negative impacts on this one because the program doesn't run on Windows...
Also, if you don't have the specific operating system on which your code failed, it will be hard to debug it.  
This example works with low-level programming languages like _C_ or _C++_. Also, some programming languages can be specific to a platform.
For example, Windows and the _C#_ programming language, even if some developers and companies are releasing their programming languages source code, to move them on new platforms (like _Swift_ or _.Net_).

## Goal of the program

You will not code a piece of program in a low-level programming language (like _C_) to write a textual game, right? If you don't have some sadomasochistic ideas, you will choose a more high-level programming language to not torture yourself with buffer overflow.
Also, don't hesitate to use some frameworks, especially web frameworks.  
For example, if you have to create a simple website and if you're not good with _PHP_ as well as _Python_, do not hesitate to take a look at [Django](https://www.djangoproject.com/).

## Libraries you have to use

Be decent with yourself: you will **NOT** only use your code in your program (except if the code is really simple, or if you are a programming language developer).
You're trying to write a game without graphic libraries? You're kidding me, right...? You're trying to write a program like [CryptoCat](https://crypto.cat/) without using Maths libs? You're kidding me, right...? You're trying to write a program without standard libraries? OMFG, YOU'RE KIDDING ME, RIGHT!?  
Hand on heart that 99.9% programs you will code or contribute will contains libraries.
If they are not good, not well documented or not efficient, you're screwed!
So, to choose a programming language by available efficient libraries is a very good point.

## Your ability to program with a programming language you already know

Let me give you an example: It's Sunday, 22h32, and you forgot you have to send your programming homework (a basic shell for example...) to your teacher until midnight.
Suddenly, you decide to learn a new programming language, and to program the shell with this programming language!
It's ok, you're good in programming and you decide that you will program a shell with... _Logo_!
Congratulations, this is the worst idea you ever had!  
This example take advantage of 2 very bad ideas: not using your ability to program with a language you already know, and not using the goal of the program to choose the language. 
In this example, one of the perfect language to implement a shell is _C_/_C++_, due to the low-level goal of the program and the popularity of those 2 programming languages.  
Don't forget: to not be perfect with a programming language is not bad!
You have to practice each time you have the time to be better with this one.
More and more you practice and more and more you will be better!  
Also, I support you to participate to open source and free softwares, or simply read the source code.
This simple task can makes you better programmer than ever.  
Last thing: if the task is really simple, don't forget to choose a programming language easy to use (please to see my example with _Django_ overhead).

## The programming paradigm

The programming paradigm is one of the important thing to take on board.
To write a simulator zoo without take care of objects programming can be really hard to maintain over time.
Also, it can be fun to write a compiler with a functional language like _Haskell_, _Ocaml_ or _F#_.
Also, famous programming language merge now many programming paradigms, like _Scala_ which merge object and functional paradigms, or _Ocaml_ where you can choose to program with objects, with functions or imperatively.  
The programming paradigm is also important if you free your source code.
Many programmers today use the objects oriented programming, and your project can be better understand and better known if you choose this one.

## Global performance of the programming language

Today, programmers search about performs programming languages for heavy or redundant tasks.  
In the book "[In the plex](http://books.simonandschuster.com/In-The-Plex/Steven-Levy/9781416596585)", [Steven Levy](https://en.wikipedia.org/wiki/Steven_Levy) give an example while explaining that the first web server wrote by [Larry Page](https://en.wikipedia.org/wiki/Larry_Page) and [Sergey Brin](https://en.wikipedia.org/wiki/Sergey_Brin), the founders of Google, used the _Python_ programming language.
This web server canno't serve more than 10 requests per second, due to the implementation of the code (very basic) and the usage of the _Python_ programming language.
So, one of the first engineer of the company had to rewrite entirely the web-server with a more efficient programming language, to take care about the number of queries per second the web-server can receive.  
Also, please to ask yourself what you really need: a compiled program (which will take care of compiler optimizations), or an interpreted program (like _Python_).
Interpreted programming languages are better to make such simple things like a text parser or an HTTP traceroute.
But, if you want to create a stronger program (a video-game from scratch for example) and if you take care about software optimizations, you have to choose a compiled programming language like _C_, _Java_, _Go_, etc.._.

## So what?

If you are a _newbie_ (no offense, this is not a disgrace), do not hesitate to conduct research about which is the best programming language **to begin**, but I invite you to try many programming languages and many programming paradigms to think about pros and cons and what is the language you **prefer**.
Today, programmers can be critical with modern programming languages (an example [here](http://yager.io/programming/go.html) for _Go_) because the programming language doesn't respect some older programming codes established by their senior (basically _C_ or _Java_).  
My advice is: don't limit yourself to these criticisms, and **try **those programming languages to **have your own opinion**!

_Cover image from [https://community.spiceworks.com/topic/576245-the-magic-of-programming-language](https://community.spiceworks.com/topic/576245-the-magic-of-programming-language)_
