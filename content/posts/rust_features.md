---
author: "Antonin"
title: "Conditional compilation and optin dependencies with Rust 'features'"
date: "2021-03-12T00:45:00+02:00"
tags: ["rust", "programming"]
description: "The good, the bad, and the rusty"
toc: true
readTime: true
---

I am a long time Rust user/fan - since 2014 - and I still have some troubles to follow the community in terms of RFCs, or accepted features to be included in an upcoming version of the language.
Most of the time, checking for the new features, I have the feeling that I don't need to use them for my current & past projects... like a purist C / C++ developer who uses the strict minimum of the features to make something great.  
And sometimes I am wrong.

I am working on a game engine since a while now - a Wolfenstein3D-like engine, with the goal to run Wolfenstein 3D on it and develop new mods for it "easily".  
I had to postpone the work since a while now and I recently rewrote (almost) everything from scratch after dumb mistakes, as I did not understood how a game engine is working, and more and more difficulties to introduce some new features in the project... like **_cross-platforms compatibility_**.  
To introduce this feature, we need to make feasible the usage of multiple OS specific low-level libraries (sound, graphics, etc...), which means different API usage, different API implementation, most of the time incompatible with each other... in the **same** common piece of code.  
So, we need an **API**, or interface, that can communicate with different backends (let's call them **packages**), based on the operating system platform - so, chosen before / during the compilation of the engine.

Rust proposes since the beginning some specific features for conditional compilation options (like the `cfg` attributes), but since the 1.18 they introduced a more powerful feature called `features`.

The repository that contains the demonstration is [available here](https://github.com/k0pernicus/rust-features-example).

## The problem

I have an awesome game called "guess a number", a textual game written fully in English, and I want to make it accessible for the International: French guys, Spanish, Italian, Greek, etc...  
More important, I want this game to be translated by **anyone**, based on the original English texts only...

So solution to this could be to make my game public, and let people contribute in including their own i18n **Rust module**, write a menu to let the user choose the language, etc...  
But I don't like this solution is that my game is **very** small (90 lines of code, including comments and many many blank lines), and I don't want to compile the game with **all** the languages into it, or let the user choose the language on runtime.  
I want the game to be **included** with the user's prefered language **only**: included at compile time.

This is where Rust `features` comes in.

## A brief introduction

Rust `features` allows [to include, at compile time, packages as optional dependencies, or a set of other features](https://doc.rust-lang.org/cargo/reference/features.html).  
And this is perfect for our usage, because I want to propose as many builds as available translations (including English).

To do this, in [our example](https://github.com/k0pernicus/rust-features-example), we need first to declare our different linguistic packages (or features):

```toml
[dependencies.english-linguistic-pkg]
version = "0.1.0"
optional = true
default-features = false
path = "english-linguistic-pkg"

[dependencies.french-linguistic-pkg]
version = "0.1.0"
optional = true
default-features = false
path = "french-linguistic-pkg"

[features]
english = ["english-linguistic-pkg"]
french = ["french-linguistic-pkg"]
```

A feature **is** a dependency - so you need to declare it anyway in the Cargo.toml file, even if you don't want to use it **by default** in your final build.
Here, we define two different dependencies: `english-linguistic-pkg` for English, and `french-linguistic-pkg` for French.
We need to declare those as **features**, in the `[features]` section.  
For simplicity, we create an alias for each of those: `english` and `french`, in order to simplify the declaration of which language package to use.  
As an example, you could use

```bash
cargo build --feature english
```

instead of 

```bash
cargo build --features english-linguistic-pkg
```

Also, I included both dependencies in the game crate.
Of course, those packages could be available in a dedicated repository, and fetch directly using an URL...

A benefit of using `features` is to use configuration conditional check to use the choosen package in the **same namespace**:

```rust
// in main.rs
#[cfg(feature = "english")]
pub use english_linguistic_pkg as i18n;
#[cfg(feature = "french")]
pub use french_linguistic_pkg as i18n;
```

Using configuration conditional checks, `i18n` is a namespace that can be linked to `english_linguistic_pkg` or `french_linguistic_pkg`.
This means, obviously, that both `english_linguistic_pkg` and `french_linguistic_pkg` must share the **exact** same contracts.

So, `print_message(i18n::SAY_HELLO)` can print "hello" if the English package has been selected, or "bonjour" if the French one has been chosen instead.

**One namespace to rule them all!**

## Let's build!

Ok, it's time to build our game!

Running `cargo build`, the panics:

```bash
error[E0433]: failed to resolve: use of undeclared crate or module `i18n`
  --> src/main.rs:46:27
   |
46 |             print_message(i18n::INVALID_INPUT, None);
   |                           ^^^^ use of undeclared crate or module `i18n`

error[E0433]: failed to resolve: use of undeclared crate or module `i18n`
  --> src/main.rs:66:23
   |
66 |         print_message(i18n::GUESS_A_NUMBER, Some(":"));
   |                       ^^^^ use of undeclared crate or module `i18n`

error[E0433]: failed to resolve: use of undeclared crate or module `i18n`
  --> src/main.rs:69:48
   |
69 |             Ordering::Equal => { print_message(i18n::PREDICTION_WIN, None); break; },
   |                                                ^^^^ use of undeclared crate or module `i18n`
...
```

Oops!

`i18n` has not been found because... we forgot to _link_ the linguistic package we want to build with!

Using the following command, we give the information to `cargo` that we want to include the English package with:

```bash
> cargo build --features english
   Compiling rust-features-example v0.1.0 (/Users/antonin/Devel/rust-features-example)
    Finished dev [unoptimized + debuginfo] target(s) in 0.69s
```

Wouhou, it works!

How about the French one?

```bash
> cargo build --features french
   Compiling rust-features-example v0.1.0 (/Users/antonin/Devel/rust-features-example)
    Finished dev [unoptimized + debuginfo] target(s) in 0.67s
```

### The 'default' features key

Now, let's come back to the first error: `use of undeclared crate or module i18n`.

It is possible to solve this issue setting a **default** package to use.
We can do that configuring the `default` key to the linguistic package we want to use **by default**:

```toml
[features]
...
default = ["english"]
```

Let's try to build the project, without specifying explicitly any package:

```bash
cargo build
   Compiling rust-features-example v0.1.0 (/Users/antonin/Devel/rust-features-example)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
```

Yes! It finally works!  
How about the French one now?

```bash
error[E0252]: the name `i18n` is defined multiple times
 --> src/main.rs:4:9
  |
2 | pub use english_linguistic_pkg as i18n;
  |         ------------------------------ previous import of the module `i18n` here
3 | #[cfg(feature = "french")]
4 | pub use french_linguistic_pkg as i18n;
  |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `i18n` reimported here
  |
  = note: `i18n` must be defined only once in the type namespace of this module
help: you can use `as` to change the binding name of the import
  |
4 | pub use french_linguistic_pkg as other_i18n;
  |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

Outch!
This is rude...

Looking at the error message, we understand that both English and French packages may be included in the final build.
The solution is simple, as we just have to set to disable the default feature(s):

```bash
cargo run --no-default-features --features french
```

After some thoughts, it comes to my mind that putting English as a `default` feature is not intuitive, especially if you will have to deal with `cargo` to build binaries with other linguistic packages...

## Advantages and disadvantages

As always in every language, a feature always comes with advantages and disadvantages...

I can notice several advantages here:  
1. no multiple useless packages inside the binary, as used **in compile time**, which can reduce the binary size,
2. really easy to plug a new translation: just update the `Cargo.toml` file, and include a conditional compilation option to use it in the project (in the main.rs, or lib.rs if you are working on a Rust library).

About the disadvantages, the developer has to produce as many binaries as available translations, and that you will have to maintain multiple packages... also, one modification in your English package and you have to ask to all to update their translation packages.

_**Note:**_ I would say that this feature miss some real examples in the rust doc, and [sometimes can be difficult to understand and to use for developers](https://kevinlynagh.com/rust-zig/).

## References:  
* https://doc.rust-lang.org/cargo/reference/features.html
