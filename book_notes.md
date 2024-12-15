# "The Book"

These are notes on "the Rust book," and specifically on [the enhanced version put together by Brown University](https://rust-book.cs.brown.edu/experiment-intro.html).

One goal of Rust is to enable both low-level and high-level programming in a single language (since languages often specialize in one or the other).

Another goal is to eliminate the tradeoff between speed and stability/safety. It does this largely through compile-time checks.

Useful tools:

- rustup (CLI tool for managing rust versions)
- Cargo (dependency manager)
- Rustfmt (linter)
- rust-analyzer (VSCode extension)

## Installation

To install rustup and Rust on a Unix system, use:

``` bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf -o rustup-init.sh
chmod +x rustup-init.sh
./rustup-init.sh
rustc --version
```

You also need a linker, a system tool used by compilers to join compiled files. You probably already have one, but if not, you can install xcode on macOS or GCC/Clang on Linux.

Windows users should install via the [downloadable executable](https://www.rust-lang.org/tools/install).

## Updates

``` bash
rustup update
```

## Documentation

``` bash
rustup doc
```

## Compiling code

Create a file named `main` with `.rs` extension and add a simple `main` function (which is required and is always the first code that runs in a Rust program).

### Function declaration

The basic function declaration syntax:

``` rust
fn main() {
    println!("Hello world!");
}
```

Lines inside the function body should end with a semicolon. The exclamation point indicates that `println` is a macro, not a function.

### Compiling with rustc

To compile and run our code:

``` bash
rustc main.rs
./main
```

Rust (obviously) is an "ahead-of-time compiled" language rather than an interpreted one, so the executable can be run on any computer (with a little help from `chmod`).

Usually we'll compile with `cargo`, not `rustc`.

## Project management with cargo

Cargo is the build system and package manager for Rust and should be installed along with `rustc`. To verify, use:

``` bash
cargo --version
```

### Initializing a project

You can create a new project with:

``` bash
cargo new projectname
```

This will create a folder, a basic `.toml` (Tom's Obvious, Minimal Language) file, a `src` subfolder with `main.rs`, and a git repository (unless you're already inside one.)

### Check that code compiles

To check that the code is compilable without actually compiling:

``` bash
cargo check
```

This validates your types and code syntax much faster than actually compiling.

### Compiling and running dev code

To compile and run a debug version with a single command:

```bash
cargo run
```

The default build is a debug build and will be placed in `./target/debug`. A lockfile will also be created in the project root.

### Compiling release code

To compile a release build, use:

``` bash
cargo build --release
```

## Rust basics

### Commenting

Comments are marked with double forward slash: `//`.

### Preludes (stuff you don't have to import)

[Some standard library stuff](https://doc.rust-lang.org/std/prelude/index.html), `std::prelude`, is available by default and doesn't need to be imported. Some libraries within `std` also have their own preludes.

### Importing libraries

You import dependencies with the `use` keyword. The standard library is `std`. For instance, import the standard input/output library with `use std::io;`.

Or, instead of explicitly importing `io` from `std`, we could simply use it in our code with the `std::` prefix, e.g., `std::io::stdin()`. The `::` operator is used in Rust to access "associated" objects and functions.

### Creating variables

To create a variable, we use the `let` keyword followed by variable name. By default variables are immutable, but `let mut` will create a mutable one.

Assignment is done with the equal sign (`=`).

Right of the assignment operator, we call the constructor of some type, such as `String::new()`. (Rust's standard `String` type is growable and UTF-8 encoded.)

### Handling console input

https://rust-book.cs.brown.edu/ch02-00-guessing-game-tutorial.html#receiving-user-input