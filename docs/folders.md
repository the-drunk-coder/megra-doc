# Folders

Mégra uses some special system folders for sketches (aka Mégra files), samples, and recordings. If they don't exist, they are created on the first startup.

## Sketchbook Folder

* Linux: `~/.config/megra/sketchbook`
* Windows: `C:\Users\<username>\AppData\Roaming\parkellipsen\megra\config\sketchbook`
* macOS: `/Users/<username>/Library/Application Support/de.parkellipsen.megra/sketchbook`

## Samples Folder

* Linux: `~/.config/megra/samples`
* Windows: `C:\Users\<username>\AppData\Roaming\parkellipsen\megra\config\samples`
* macOS: `/Users/<username>/Library/Application Support/de.parkellipsen.megra/samples`

## Recordings Folder

* Linux: `~/.config/megra/recordings`
* Windows: `C:\Users\<username>\AppData\Roaming\parkellipsen\megra\config\recordings`
* macOS: `/Users/<username>/Library/Application Support/de.parkellipsen.megra/recordings`

# Custom Base Folder

The three folders mentioned about are always in the default location, but if you want to use a different "base" folder, you can specify it at startup:

```
megra_rs --base <different/base/folder>
```

Make sure that folder exists.

The three abovementioned folders will be created in there if they don't exist yet.
