# Recording

Mégra allows you to record your sessions directly, without having to route the audio to a separate program. 

All recordings will be stored in the recordings folder.

To start a recording, type:

```lisp
(rec "<prefix>")
```

The prefix is optional, if it's not provided, it'll just be "megra_recording".

In any case, Mégra will add the date and time of the recording.

Recording format is 32-bit float wave files.
