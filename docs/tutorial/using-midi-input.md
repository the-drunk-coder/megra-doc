# Using MIDI Input

As of version 0.0.10, Mégra has better support for MIDI input (no MIDI output so far). 

## Checking Available MIDI Ports 

You can check available MIDI ports using the `(list-midi-ports)` function, which will print the MIDI ports to stdout.

## Opening a MIDI Port

Use `(open-midi-port <port-num>)` with the port number according to the list.

## Defining MIDI Callbacks 

To handle incoming MIDI events, you have to define a callback function:

```
;; print the received values to stdout 
;; make sure to use the "midi" identifier like this,
;; variable names can be chosen freely ...
(callback "midi" (onoff key vel)
	(progn
		(print onoff)
		(print key)
		(print vel)
		))
```

For more complex handling, you can use the `match` function: 

```
(callback "midi" (onoff key vel)
	(match onoff
		144 (once (saw 100)) ;; play a sound for note-on events ..
		...
	))
```

## MIDI Helpers 

There's some functions to help you using incoming MIDI data:

* `mtof` converts a MIDI note to a frequency: `(mtof <midi-note> <concert-pitch>)`
* `mtosym` converts a MIDI note to a symbol, like `a2` etc ..
* `veltodyn` converts a MIDI velocity to a dynamic notation symbol such as `'mp` or `fff`

