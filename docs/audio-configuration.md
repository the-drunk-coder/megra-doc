# Audio Configuration

As mentioned, the default version of Mégra only works at a fixed blocksize of 512, so if that is the version you installed, make sure your system blocksize is at 512. If you installed the low-latency version, make sure your blocksize is at 128.

Any samplerate should work, but be aware that all samples you use will be resampled to the current samplerate if they don't match, which might increase loading time. I recommend using the samplerate in which the majority of your samples are stored. The tutorial sample set is stored at 44100hz.



