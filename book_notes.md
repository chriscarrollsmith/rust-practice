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

Lines inside the function body should end with a semicolon. The exclamation point indicates that `println` is an unscoped macro that gets expanded at compile time, not a scoped function that gets called at execution time. (Macros execute faster but take up more space in the compiled binary.)

### Compiling with `rustc`

To compile and run our code:

``` bash
rustc main.rs
./main
```

Rust is an "ahead-of-time compiled" language rather than an interpreted one, so the executable can be distributed and run without a language interpreter.

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

This will create a folder, a basic `.toml` (Tom's Obvious, Minimal Language) config file, a `src` subfolder with `main.rs`, and a git repository (unless you're already inside one.) Navigate into the project folder with:

``` bash
cd projectname
```

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

To compile an optimized release build, use:

``` bash
cargo build --release
```

You'll want to use a release build for any benchmarking you do.

## Rust basics

### Commenting

Comments are marked with double forward slash: `//`.

### Style

It's considered good style in Rust to put dot-notated method calls on a subsequent line, with a greater indent, than the object the method belongs to. E.g.,

``` rust
let mut buffer = String::new();
std::io::stdin()
    .read_line(&mut buffer)
    .expect("Failed to read line");
```

This creates an empty mutable `String` variable named `buffer`, then calls the `read_line` method on the standard input object, passing it a mutable reference to `buffer`. The `expect` method is called to handle any errors that might occur when reading input.

### Preludes (stuff you don't have to import)

[Some standard library stuff](https://doc.rust-lang.org/std/prelude/index.html), `std::prelude`, is available by default and doesn't need to be imported. Some libraries within `std` also have their own preludes.

### Importing libraries

You import dependencies with the `use` keyword. The standard library is `std`. For instance, import the standard input/output library with `use std::io;`.

Or, instead of explicitly importing `io` from `std`, we could simply use it in our code with the `std::` prefix, e.g., `std::io::stdin()`.

The `::` operator is used in Rust to access "associated" objects and functions.

### Creating variables

To create a variable, we use the `let` keyword followed by variable name. By default variables are immutable, but `let mut` will create a mutable one.

Assignment is done with the equal sign (`=`).

Right of the assignment operator, we call the constructor of some type, such as `String::new()`. (Rust's standard `String` type is growable and UTF-8 encoded.)

### Handling console input

The `std::io::stdin()` function returns an instance of the `std::io::Stdin` type (which is a handle for the standard input from the terminal).

This type has a `read_line` method that can be called using dot notation. You have to pass this method the memory address of a previously declared mutable `String` variable that it can write to as a buffer: `stdin_var.read_line(&mut buffer_var);`.

The `read_line` method writes the line to the buffer (as a side effect). It also returns a `std::Result` object, which is an Enum that takes a value of either "Ok" or "Err". As in C, the ampersand (`&`) indicates a "reference". The `Result` type has an `expect` method we can call, which terminates the program if `Result` is an "Err".

### String formatting

We can use curly braces in printed output to insert a variable placeholder: `println!("String value: {some_str}");`.

Alternatively, we can use empty braces and then provide comma-separated values to insert in sequence. Or we can combine the two syntaxes: `println!("{x} + {} = {}", y, x + y)`.

### Dependency management

To add or remove dependencies ("crates") in your manifest, you can use `cargo add` and `cargo remove`.

To update a dependency to the latest non-breaking version compatible with your manifest (i.e., only patch-level increments), use `cargo update`. For potentially breaking updates (major/minor-level increments), use `cargo update --breaking`.

### Traits

Some libraries have "traits" you may need to import to enable certain methods or behaviors. Like, to use ranges in random number generators, you need to `use rand::Rng`. (Rust's range syntax is: `start..=end`.)

For documentation on what traits are available from the dependencies in your project, you can run `cargo doc --open` to open consolidated documentation for all your project dependencies in a web browser.

### Comparing numbers

Comparable types have a `.cmp` method that takes as its argument an ampersand-prefixed "reference" to a variable to compare against. This method returns one of three enum values from `std::cmp::Ordering`: "Greater", "Less", or "Equal".

### Pattern matching

Instead of `if-else` conditionals, the Rust community prefers "pattern matching" with a `match` expression for better safety, readability, and compile-time "jump table" optimization. The comma-separated "arms" of a `match` statement include a pattern on the left, followed by `=>`, and then the outcome logic to execute if the pattern matches.

One common place to use this is in handling `Result` values, e.g., ` match fn_call() { Ok(value) => value, Err(e) => println!("Error: {e}") }`.

### Coercing one type to another

Instead of forcing the user to always create a unique new variable name, Rust allows shadowing (re-declaring the same variable name, typically with a different type).

We can coerce a string to an integer type (or perform other conversions) with the `parse` method. `parse` uses the type of the variable being assigned to to determine exactly what conversion logic to use. `parse` returns a `std::Result` object, which you must handle with `expect` or `unwrap` to get the converted value.

### Loops

You can create an infinite loop with `loop {}` and can break it with `break`.

## Common programming concepts as they apply to Rust

### Mutable variables

Variables in Rust (declared with `let`) are immutable by default, but you can make them mutable with `let mut`. If you try to assign a new value to an immutable variable, the compiler will helpfully flag that you should consider making it mutable.

### Constants

In addition to declaring variables with `let`, you can also declare constants with `const`. The convention in Rust is to use screaming snake case for constant names. Unlike variables, which can only be used in a function, constants can be declared in any scope, including global. You must always annotate the constant type.

You may never use `mut` with `const`; they are always immutable. They can be computed values, but the computation must be a constant expression; it can't use variables or values that might change at runtime.

### Shadowing

Interestingly, you aren't required to use `mut` to shadow a variable; you just have to declare it with `let`. For instance, `let x = 2; let x = x + 2;` will set `x` to `2` and then `4`, but in both cases the variable will be immutable after the assignment.

Also, variable assignments are scoped, so we can do `let x = 2` in an outer scope and then `let x = x + 2` in a subordinate scope, and the variable in the outer scope will continue to have an immutable value of `2` even as the variable gets shadowed in the child scope.

(Fun Rust fact: you can create a new child scope with curly braces (`{}`) even without attaching them to a loop or conditional.)

## Data types

### Scalar types

Integers: `i8`/`u8`, ... `i128`/`u128`, `isize`/`usize` (64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture)

    - You can use `_` as a separator to improve readability, e.g. for decimals stored as integers; it doesn't change the value
    - You can add a suffix to specify type (e.g., `57u8`) or a prefix to specify base (e.g., `0b1111_0101` for binary, `0o77` for octal, `0x1A` for hexadecimal)
    - The `b` prefix specifies a byte literal (`u8`)
    - The default `int` type in Rust is `i32`
    - You use `isize` and `usize` for indexing a collection, which will be the same size as your machine's address space
    - During dev, runtime integer overflow causes a panic (exit with error code); during release, it causes wrapping (reverts to 0)
    - Handle overflow with `wrapping_`, `checked_`, `overflowing_`, or `saturating_` methods
    - Integer division truncates toward 0
    - Two's complement storage means range for a given bit size n is -2^(n-1) to 2^(n-1)- 1 (e.g., `i8` is -128 to 127)

- Floating-point numbers: `f32` or `f64` (always signed)
- Booleans: `bool` (1 byte)
- Characters (UTF-8): `char` (specify with single quotes, as opposed to double quotes for strings; 4 bytes)

https://rust-book.cs.brown.edu/ch03-02-data-types.html#compound-types