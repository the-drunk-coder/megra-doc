# Installation

There are three ways to install Mégra. You can download the binary, install it using `Cargo` (Rust's package manager), or compile it from source.

## Binaries

This is the easiest way if you're interested in using Mégra. As Mégra is a standalone, self-contained program, there's not much more setup to be done. Just download the latest version from the <a href="https://github.com/the-drunk-coder/megra.rs/releases/" target="_blank">release page</a>.

LATEST RELEASE: <a href="https://github.com/the-drunk-coder/megra.rs/releases/tag/v0.0.14" target="_blank">Mégra 0.0.14</a>

On Linux and macOS, you'll probably need to make the file executable by calling `chmod +x <filename>` on the command line.

**NOTE:** The pre-build versions don't need you to configure your system's audio buffer size, at the cost of a slightly higher latency. For low-latency versions, see the "With Cargo" install method.

There's no ASIO version currently for Windows.

## With Cargo

On Linux, I recommend this method ... install `rust` and `cargo` using the instructions found on <a href="https://rust-lang.org" target="_blank">https://rust-lang.org</a> if you don't already have them.

Then, you can install the latest relase from crates.io.

Due to some Rust features that are not fully finished, you need to compile against the `nightly` version of Rust.

Also on Linux, you'll need to install some dependencies, depending on your distribution:

* `jack2` or `pipewire-jack`, depending on what you use as a sound server.
* `libjack-dev` (Debian-based) 
* `libasound2-dev` (Debian-based) or `alsa-lib` (Arch-based)


The, in a terminal, type:
```
rustup default nightly
```

Then, again in a terminal, type:
```
cargo install megra_rs
```

On **Windows**, you'll need the ringbuffer feature, so type:
```bash
cargo install megra_rs --features ringbuffer
```

The ringbuffer version might have a slightly higher latency, but is way less complicated to use on any system.
Please read the chapter about audio configuration to see what the ringbuffer feature does.

### Low-Latency Build

As of version 0.0.6, you can build Mégra with lower latency by using a smaller blocksize of 128. This will
work only on Linux or macOS, but not with WASAPI on Windows (probably with ASIO, but I never tried that). 
To do so, install or build with the feature `low_latency` enabled:

```bash
cargo install megra_rs --features low_latency
```

## From Source

Download or clone the repo. In repo folder, type ...

```
cargo run --release -- -o 2ch
```

## Troubleshooting

### Linux 

If the downloaded version doesn't work, it might be because the release version is compiled against a newer version of glibc. In this case, 
you can install it from source or use `cargo install`. It's recommended to install Rust following the instructions on `https://rust-lang.org`.

### Windows

If Mégra doesn't start, try starting it from a command prompt. If it says something like "the current stream config isn't supported", try starting it with the option `--live-buffers 2` (or whatever you number of output channels is). Sometimes WASAPI doesn't support different numbers of input- and output channels.


### MacOS

Mégra will always look for an INPUT device, but some Apple devices don't have that (such as a Mac Mini). In that case Mégra might not start. This shouldn't be much of a problem with laptops, or with external interfaces that have inputs (will be fixed in the future).
