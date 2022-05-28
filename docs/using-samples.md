# Using (and Finding) Samples

If you don't already have a sample set at hand, some samples (enough to follow the documentation and tutorial) can be found here: https://github.com/the-drunk-coder/megra-public-samples

Mégra currently only supports samples in FLAC format (wav is in the making).

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

There's currently no way to specify custom sample folders, but you can also load individual samples to a set by hand using 
```
(load-sample :set '<set> :path "<path-to-sample>")
```. 

These don't need to be in the samples folder, the path can point anywhere.

As mentioned above, make sure you cofigure your audio system to the samplerate of your samples, otherwise loading samples will be slow due to resampling !
