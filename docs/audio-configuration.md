# Audio Configuration

As mentioned, the default version of Mégra only works at a fixed blocksize of 512, so if that is the version you installed, make sure your system blocksize is at 512. 

Any samplerate should work, but be aware that all samples you use will be resampled to the current samplerate if they don't match, which might increase loading time. I recommend using the samplerate your samples are stored in.

## Ringbuffer Versions

For both macOS and Windows, there are versions that have a `_ringbuffer` suffix. There versions can adapt to a wide range of audio buffer sizes, so if you're unsure what your system's audio buffer size (aka blocksize) is, use these. 

On Windows, it's necessary to use that version because the WASAPI audio API uses variable buffer sizes.

There's no such version for Linux because with JACK, it is very easy to set the right blocksize.


## Troubleshooting

### Windows

If Mégra doesn't start, try starting it from a command prompt. If it says something like "the current stream config isn't supported", try starting it with the option `--live-buffers 2` (or whatever you number of output channels is). Sometimes WASAPI doesn't support different numbers of input- and output channels.
