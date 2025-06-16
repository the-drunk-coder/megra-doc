# Using (and Finding) Samples

## Tutorial Sample Set 

If you want to use the tutorial sample set, all you need to do is to run the command `(import-sample-set 'tutorial)` the first time you start Mégra a single time.

This will download a minimal set of samples (about 128MB) that will get you through the tutorial. It includes nice piano samples.

## Custom Sample Sets

If you want to use your own sample set, you can either:

* add them to the default folder (see structure below)
* point Mégra to a sample folder at startup
* load samples on demand while Mégra is running

## Maximum Number of Samples

As of now, Mégra **loads all samples to memory**, which means that it takes some time to load them at startup. That also means that there's a limitation on how many samples you can load for one session. If your sample set is huge and maxes out your device's RAM, you'll need to select a smaller subset. 

Currently, Mégra needs to know in advance a maximum number of samples you want to (potentially) load. The default is 3000 samples.

This might not be enough for some collections, i.e. the entire Dirt-Samples collection from TidalCycles.

You can increase the number using the `--max-sample-buffers` startup option when starting Mégra.

## Sample Format

Mégra currently supports samples in FLAC or WAV format. Per default, Mégra uses **mono** samples. If you load a stereo file, it will be **downmixed to mono** when it is loaded. In case you want to use stereo samples, you need to enable this feature at startup using the `--use-stereo-samples` option.

If your samples are in a different samplerate that the one you started Mégra with, they'll be re-sampled to the current samplerate.

## Default Folder, Sample Set Structure and Naming Convention

The default folder for samples (folder will be created at first start) can be found at:

    Linux: ~/.config/megra/samples.
    Windows: C:\Users\<username>\AppData\Roaming\parkellipsen\megra\config\samples
    macOS: /Users/<username>/Library/Application Support/de.parkellipsen.megra/samples

In there, you'll find several sub-folders. By convention, Mégra will read each folder as a possible sound event.
That means, if you have a sub-folder called `bd` in the samples folder, you can call it as a sound event like this:

```
;; call this in the Mégra editor (place cursor between outer parenthesis and hit "Ctrl+Return")
(once (bd))
```

### Using Keywords for Sample Lookup

You can call specific files from the folder by keyword. For example, if you have a sample called `jazzy.flac` in your `bd` folder, you can call it like this:

```
(once (bd 'jazzy))
```

The filenames are separated into keywords by whitespace, dash, underscore and dot. Thus, if your sample is called `ultimate jazz-bassdrum.latest.flac`, the available 
keywords will be `ultimate`, `jazz`, `bassdrum` and `latest`. 

If you don't provide any keyword, a random sample from the folder is chosen. If a keyword matches multiple samples, one of the possible matches will be chosen at random.

For example, 

```
(once (piano 'a3))
```

will choose a piano sample with pitch `a3`, but random velocity.

```
(once (piano 'a3 'ff))
```

on the other hand will only match one sample in the `piano` folder, the one with pitch `a3` and velocity `ff`, so the result will be deterministic.

### Exclude Samples

If your sample set is large, and you don't want to use all the sample folders in your collection, you can put a file named `exclude.txt` in the sample collection folder. 

In that file, you can specify sample folders to be excluded (one folder per line). I.e., if you don't want to load bassdrum samples, put a line `bd` in `exclude.txt`. You can comment out excluded folder (thus include them) by commenting them out with the `#` character.

### Custom Sample Folder 

You can provide a **custom sample folder** at startup using the `--sample-folder <path-to-folder>` flag.

### Loading Samples at Runtime

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

**IF you're using the (default) Dirt-Samples**, keep in mind that sample selection in TidalCycles is very different 
(TidalCycles indexes on sort order instead of keywords), so the level of control you have over which samples are chosen will depend on the filenames, and 
whether they can be parsed as keywords. Otherwise, you would need to know the entire file name to call a specific sample.

Mégra currently doesn't allow function names that start with a digit, so in case there are folders that start with one (such as the *808hc* and so on), 
the set will be available with an underscore: `(once (_808hc))`.

As mentioned above, make sure you configure your audio system to the samplerate of your samples, otherwise loading samples will be slow due to resampling!
