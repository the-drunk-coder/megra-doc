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

### `mtof` - Midi Note to Frequency

A classic ... needs midi note and base frequency as arguments:

**Example:** `(once (saw (mtof 43 440)))` is the same as `(once (saw 97.99885))`

### `mtosym` converts a MIDI note to a symbol, like `a2` etc ..

`mtosym` converts a MIDI note to a symbol, like `a2` etc ..

**Example:** `(mtosym 43) ;; -> 'g3`

This is helpful if you have a sample set of a tuned instrument, such as a piano, where the individual samples are stored in 
a format like: `<pitch>_<dyn>.wav`, i.e. `a2_ff.wav`. At least that's the way I store my piano samples, and with the help of 
this converter, I can play them with a MIDI keyboard.

```lisp
;; test single note:
(once (piano (mtosym 43) (veltodyn 50)))

;; create a piano sampler (if you have the samples organized the same way as described above):

(open-midi-port 1)

(callback "midi" (onoff note vel)
  (match onoff ;; ignore note off for now ...
    144 (once (piano (mtosym note) (veltodyn vel)))))

```

### `veltodyn` - MIDI Velocity to Dynamic Symbol

`veltodyn` converts a MIDI velocity to a dynamic notation symbol such as `'mp` or `fff`.

**Example:** `(veltodyn 50) ;; -> 'mp` `(veltodyn 127) ;; -> 'ff`

For the use of this function, see example above under `mtosym`

