# Recording

Mégra allows you to record your sessions directly, without having to route the audio to a separate program. 

All recordings will be stored in the recordings folder.

To start a recording, type:

```lisp
(rec "<prefix>")
```

The prefix is optional, if it's not provided, it'll just be "megra_recording".

To stop the current recording, type `(stop-rec)`.

If you want to record the input of you session as well, type 

```lisp
(rec "<prefix>" :input #t)
```

This will add a additional file with the input.

In any case, Mégra will add the date and time of the recording.

Recording format is 32-bit float wave files. The maximum number of channels is recorded, that is, if you have 8 output channels and 2 input channels, the resulting files will be multichannel WAV files with 8 channels.
