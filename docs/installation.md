# Installation

There's three ways to install Mégra. Before starting, make sure you read the chapter about the audio configuration!

## Binaries

This is the easiest way if you're interested in using Mégra. As Mégra is a standalone, self-contained program, there's not much more setup to be done. Just download the latest version from the [release page](https://github.com/the-drunk-coder/megra.rs/releases/).

[Mégra 0.0.6](https://github.com/the-drunk-coder/megra.rs/releases/tag/0.0.6)

On Linux and macOS, you'll probably need to make the file executable by calling `chmod +x <filename>` on the command line.

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
cargo run --release -- -o 2ch
```

## Troubleshooting

### Linux 

If the downloaded version doesn't work, it might be because the release version is compiled against a newer version of glibc. In this case, 
you can install it from source or use `cargo install`. It's recommended to install Rust following the instructions on `https://rust-lang.org`.



