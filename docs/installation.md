# Installation

There's three ways to install Mégra. Before starting, make sure you read the chapter about the audio configuration!

## Binaries

This is the easiest way if you're interested in using Mégra. As Mégra is a standalone, self-contained program, there's not much more setup to be done. Just download the latest version from the [release page](https://github.com/the-drunk-coder/megra.rs/releases/).

[Mégra 0.0.4](https://github.com/the-drunk-coder/megra.rs/releases/tag/0.0.4)

## With Cargo

If you already use `rust` and `cargo`, you can install the latest relase from crates.io.

In a terminal, type:
```
cargo install megra_rs
```
On Windows, you'll need the ringbuffer feature, so type:
```bash
cargo install megra_rs --features ringbuffer
```

Read the chapter about audio configuration to see what the ringbuffer feature does.

## From Source

Download or clone the repo. In repo folder, type ...

```
cargo run --release -- -e -o 2ch
```



