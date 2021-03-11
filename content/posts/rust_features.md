+++
title = "Conditional compilation and optin dependencies with Rust 'features'"
date = 2021-10-03T12:00:00+02:00
draft = true
categories = ["rust", "programming"]
tags = ["rust", "programming"]
description = "The good, the bad, and the rusty"
disqus = false
+++

I am a long time Rust user, since nearly 2014 to be honest, and I still have some troubles to follow the community in RFCs or new features included in the latest version(s) of the language...

Sometimes, I don't feel I need them *right now*, or in the future **at all**.
So, I will put the features annuncements in a chunk of my head, in order to check and understand why they have been introduced later...

I am working on a game engine since a while now - a Wolfenstein 3D like engine, with the goal to run Wolfenstein 3D on it and develop new mods for it "easily".
I postponed the work a while now, and recently rewrote (almost) everything from scratch after dumb mistakes, because I did not understood how works a game engine, or getting more and more complexity in introducing some features in the project... like _cross-platforms compatibility_.  
To introduce this feature, we need to make possible the use of multiple operating system libraries (sound, graphics, etc...) with low-level execution on it - different API usage, different API implementation, so they are not 100% compatible with each other - in the same common source code.
So, we need an API, or interface, that can communicate with different backends (let's call them **modules**), based on the operating system platform - so, chosen before / during the compilation of the engine.

Rust introduced since a while now some specific features for conditional compilation options (like the `cfg` attributes), but this time we will check about a more powerful feature that has been introduced since Rust 1.18, called `features`.

The repository that contains the demonstration is [available here](https://github.com/k0pernicus/rust-features-example).

## The problem

I have an awesome game called "guess a number", and I want to internationalise it.  
More important, I want that this game could be translated by anyone, based on the original English texts...

I could make my game public, let people contribute to it in making their own i18n **Rust module**, and merge once it's ready.

The issue is that my game is **very** small (90 lines of code, including comments and many many blank lines), and I don't want to compile my game with all the available languages, let the user choose the language on runtime, and let him play right after...  
I want the game to be **included** with the user's prefered language **only**: included at compile time.

Rust has a feature for that, called `features`.

## A brief introduction

`features` allows [to include, at compile time, packages as optional dependencies, or a set of other features](https://doc.rust-lang.org/cargo/reference/features.html).  
And this is perfect for our usage: we want our user to build the game with its own main language (English, French, Spanish, ...) **only**.

In [our example](https://github.com/k0pernicus/rust-features-example), we need first to declare our different packages (or features):

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

As we specify and use the linguistic package at compile time, we can now use it in our code and use the **same namespace** using a configuration conditional check:

```rust
// in main.rs
#[cfg(feature = "english")]
pub use english_linguistic_pkg as i18n;
#[cfg(feature = "french")]
pub use french_linguistic_pkg as i18n;
```

Using this code snippet, `i18n` is a namespace that can be linked to `english_linguistic_pkg`, or `french_linguistic_pkg`, depending of what the user chosed building the game.

So, `print_message(i18n::SAY_HELLO)` can print "hello" if the English package has been selected, or "bonjour" if the French one has been selected instead.

**One namespace to rule them all!**

## Let's build!

Ok, it's time to build our game!

Running `cargo build`, the compiler panics:

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

What happened?  
`i18n` is an alias to a crate (or package), that the user provides building the project.
Here, `i18n` is not found, because... we forgot to link the linguistic package we want to use!

Using the following command, we give the information to `cargo` that we want to use **explicitly** the English package:

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

Nice!

### The 'default' features key

Now, let's come back to the previous error...  
If we do not specify any feature, the project fails to compile.

It is possible to solve this issue in setting a **default** package to use, if the user did not provided as argument a linguistic package.
We can do that setting the `default` key to the linguistic package we want to use by default:

```toml
[features]
...
default = ["english"]
```

Let's try to build the project without specifying any feature:

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

Ouch!
This is rude...

Looking at the error message, we understand that both English and French packages may be included in the final build.
The solution is simple, as we just have to set to disable the default feature(s).

```bash
cargo run --no-default-features --features french 
```

After some thoughts, it comes to my mind that putting English as a `default` feature is not intuitive, especially when you will have to deal with cargo to build binaries with other linguistic packages...

## Advantages and disadvantages

As always in every language, a feature always comes with advantages and disadvantages...

I can notice several advantages here:  
1. no multiple useless packages inside the binary, as compiled in compile time (no runtime for this),
2. reduce the size of the binary,
3. really easy to plug a new translation: just update the Cargo.toml file, and include a conditional compilation option to use it in the project (in the main.rs, or lib.rs if you are working on a Rust library).

About the disadvantages, the developer has to produce as many binaries as available translations, and that you will have to maintain multiple packages... one modification and you will have to ask to all the contributors to update their own translation.

Also, I would say that this feature miss some real examples in the rust doc, and [sometimes can be difficult to understand and to use for developers](https://kevinlynagh.com/rust-zig/).

References:  
* https://doc.rust-lang.org/cargo/reference/features.html
