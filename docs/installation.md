# Installation

There are three ways to install Mégra. You can download the binary, install it using `Cargo` (Rust's package manager), or compile it from source.

## Binaries

This is the easiest way if you're interested in using Mégra. As Mégra is a standalone, self-contained program, there's not much more setup to be done. Just download the latest version from the [release page](https://github.com/the-drunk-coder/megra.rs/releases/).

LATEST RELEASE: [Mégra 0.0.10](https://github.com/the-drunk-coder/megra.rs/releases/tag/0.0.10)

On Linux and macOS, you'll probably need to make the file executable by calling `chmod +x <filename>` on the command line.

### How to Pick a Version

As you can see on the download page, for some operating systems, there is a version that has the suffix `_ringbuffer`.

The default, non-ringbuffer version of Mégra only works at a fixed blocksize of 512, so if you want to install that version, make sure you're able to set your system blocksize to 512. 

On Windows, it's necessary to use the ringbuffer version because the WASAPI audio API uses variable buffer sizes. There's no ASIO version currently.

## With Cargo

On Linux, I recommend this method ... install `cargo` from your distro's package manager, then install Mégra with `cargo`.

If you already use `rust` and `cargo` (on any system), you can install the latest relase from crates.io.

Due to some Rust features that are not fully finished, you need to compile against the `nightly` version of Rust.

In a terminal, type:
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



