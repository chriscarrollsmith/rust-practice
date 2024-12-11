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

Create a file named `main` with `.rs` extension and add a simple `main` function (which is required and is always the first code that runs in a Rust program). T

The basic function declaration syntax:

``` rust
fn main() {
    println!("Hello world!");
}
```

Lines inside the function body should end with a semicolon. The exclamation point indicates that `println` is a macro, not a function.

To compile and run it:

``` bash
rustc main.rs
./main
```

Rust (obviously) is an "ahead-of-time compiled" language rather than an interpreted one, so the executable can be run on any computer (with a little help from `chmod`).

Left off at: https://rust-book.cs.brown.edu/ch01-03-hello-cargo.html