# Using (and Finding) Samples

If you don't already have a sample set at hand, some samples (enough to follow the documentation and tutorial) can be found here: https://github.com/the-drunk-coder/megra-public-samples

Mégra currently supports samples in FLAC or (as of version 0.0.6) WAV format. Mégra uses **mono** samples, stereo samples are not supported. If you load a stereo file, it'll be **downmixed to mono**. If your samples are in a different samplerate that the one you started Mégra with, they'll be re-sampled to the current samplerate.

Place the samples in the folder (folder will be created at first start):

    Linux: ~/.config/megra/samples.
    Windows: C:\Users\<username>\AppData\Roaming\parkellipsen\megra\config\samples
    macOS: /Users/<username>/Library/Application Support/de.parkellipsen.megra/samples

Now you'll have a sound event for every sample. That means, if you have a folder called bd in the samples folder, you can call it like this:

```
;; call this in the Mégra editor (place cursor between outer parenthesis and hit "Ctrl+Return")
(once (bd))
```

You can also search by keyword ... if you have a sample called `jazz.flac` in your bd folder, you can call it like:

```
(once (bd 'jazz))
```

The filenames are separated into keywords by whitespace, dash, underscore and dot. Thus, if your sample is called `ultimate jazz-bassdrum.latest.flac`, they keywords will be `ultimate`, `jazz`, `bassdrum` and `latest`. 

If you don't provide any keyword, a random sample from the folder is chosen. Samples outside of the abovementioned samples folder can't be called.

As of version 0.0.6, you can provide a **custom sample folder** at startup using the `--sample-folder <path-to-folder> flag.

You can also load individual samples to a set by hand using 
```
(load-sample :set '<set> :path "<path-to-sample>")
```

These don't need to be in the samples folder, the path can point anywhere.

Additionally, you can load a folder with samples that follows the correct structure ... i.e. if you're already using TidalCycles, and have the Dirt-Samples collection
somewhere, you can import it using:

```
(load-sample-sets "/<path-to-parent>/Dirt-Samples")
```

**IF you're using the (default) Dirt-Samples**, keep in mind that sample naming in TidalCycles is very different (it uses numbers instead of keywords), 
so you might not be able to get fine-grained control over which samples are chosen.

Mégra currently doesn't allow function names that start with a digit, so in case there's folders that start with one (such as the *808hc* and so on), 
the set will be available with an underscore: `(once (_808hc))`.

As mentioned above, make sure you configure your audio system to the samplerate of your samples, otherwise loading samples will be slow due to resampling !

## Maximum Number of Samples

As of now, Mégra load all samples to memory, which means that it takes some time at startup. 

Up to Mégra 0.0.6, the maximum number of samples you can load is 3000, which might not be enough for some collections, i.e. the entire Dirt-Samples collection.

Starting with Mégra 0.0.7, you can increase the number using the `--max-sample-buffers` startup option.
