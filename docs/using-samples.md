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

There's currently no way to specify custom sample folders at startup, but you can also load individual samples to a set by hand using 
```
(load-sample :set '<set> :path "<path-to-sample>")
```

You can also load a folder with samples that follows the correct structure ... i.e. if you're already using TidalCycles, and have the Dirt-Samples collection
somewhere, you can import it using:

```
(load-sample-sets "/<path-to-parent>/Dirt-Samples")
```
Mégra currently doesn't allow function names that start with a digit, so in case there's folders that start with one (such as the *808hc* and so on), 
the set will be available with an underscore: `(once (_808hc))`.

These don't need to be in the samples folder, the path can point anywhere.

As mentioned above, make sure you cofigure your audio system to the samplerate of your samples, otherwise loading samples will be slow due to resampling !
