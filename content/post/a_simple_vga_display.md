+++
author = "Antonin"
date = "2016-07-27T18:46:01-04:00"
description = "Back to the 60's"
tags = ["rust", "os", "arOS", "intermezzOS"]
title = "Display text easily in arOS, using a Rust macro"

+++

In my previous post, I talk about my joy to program my first operating system from scratch (arOS), using assembly and rust code.  

Steve Klabnik, initiator of the [intermezzOS](https://github.com/intermezzOS) project, hasn't explain his own solution to 
display some text on screen, easily and using Rust.

The current code to display some text on the screen is the get the address of the VGA buffer (`0xb8000`), and to add manually 
our characters, one by one...

So, actually, to display a simple "Hello world" in color green on backscreen black, the code is `0x026c026c02650248026f`.  
Ugly? Yes!

A solution proposed by Steve is to implement a macro which take as parameter a string and display on the screen that one.

Let's try!

This is the file `lib.rs` which contains my ugly code:

```
#![feature(lang_items)]
#![no_std]

#[lang = "eh_personality"]
extern fn eh_personality() {
}

#[lang = "panic_fmt"]
extern fn rust_begin_panic() -> ! {
    loop {}
}

#[no_mangle]
pub extern fn kernel_main() -> ! {
    unsafe {
        let vga = 0xb8000 as *mut u64;
        *vga = 0x026c026c02650248;
		let vga2 = 0xb8008 as *mut u64;
		*vga2 = 0x026f;
    }
    loop {}
}
```

First, let's try to save the memory address of the VGA's buffer.  
We want this address **global**.
So, we will use `static`.

Instead of `const`, `static` can be mutable - the advantage of this solution 
is to have only one instance of this memory address, and a fixed location in memory.

`static VGA: u64 = 0xb8000;`

Also, I want to store the color of the text and the background to display on screen.

`static COLOR: u64 = 0x02;`

`0x02` corresponds to have a black background color (`0`) and a green text (`2`).

Now, we have to define our Rust macro.  
This one take as parameter a &str pointer - so, an expression - and have to display on screen this one.

The skeleton of the macro (let's called it `k_println`) is following.

``` 
macro_rules! k_println {
    ($e:expr) => {    
	};
}
```

Now, we have to fill this macro.

We work on memory addresses, so we have to use `unsafe`.

``` 
macro_rules! k_println {
    ($e:expr) => {
		unsafe {
		}
	};
}
```

We know that `$e` is a string, and we want to display each character of this string.  
So, we have to iterate on those characters to put each one in the buffer of the VGA.

As my configuration is little endian, I must begin to enter the character (as an unsigned 64 bits value), and after that the color of the character to display.
Also, to know where to display the character on screen, I have to save the offset!

```
macro_rules! k_println {
    ($e:expr) => {
        unsafe {
            let mut offset : u64 = 0;
            for c in $e.chars() {
                *((VGA + offset) as *mut u64) = c as u64;
                offset += 1;
                *((VGA + offset) as *mut u64) = COLOUR;
                offset += 1;
            }
        }
    };
}
```

Great!

Now, our definition is `k_println`.  
So, we have to pass a new line (the hexadecimal value for this is `0x0A`) and let the following line to be displayed under this one (you have to 
update the VGA buffer pointer to the current pointer + 160 characters).

We just have to complete the code with the two following expressions, after the loop:
```
*((VGA + offset) as *mut u64) = 0x0A;
VGA = VGA + 160;
```

And... TADA!  
We just wrote a simple Rust macro to display easily a string in your new awesome operating system! :-D

Final code:

```
#![feature(lang_items)]
#![no_std]

static mut VGA: u64 = 0xb8000;
static COLOUR: u64 = 0x02;

macro_rules! k_println {
    ($e:expr) => {
        unsafe {
            let mut offset : u64 = 0;
            for c in $e.chars() {
                *((VGA + offset) as *mut u64) = c as u64;
                offset += 1;
                *((VGA + offset) as *mut u64) = COLOUR;
                offset += 1;
            }
            *((VGA + offset) as *mut u64) = 0x0A;
            VGA = VGA + 160;
        }
    };
}

#[lang = "eh_personality"]
extern fn eh_personality() {
}

#[lang = "panic_fmt"]
extern fn rust_begin_panic() -> ! {
    loop {}
}

#[no_mangle]
pub extern fn kernel_main() -> ! {
    k_println!("Hello, World!");
    loop {}
}

```

