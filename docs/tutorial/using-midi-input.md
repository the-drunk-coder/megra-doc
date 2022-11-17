# Using MIDI Input

As of version 0.0.6, Mégra has very (very!) rudimentary midi input. It only registers note-on events, and no velocity (or at least as of now there's not much you can do with that).

You can use it to trigger events "by hand" or move step sequencers forward by hand.

```lisp

(midi-callback 41 (once (saw 100))) ;; define a single-event callback

(defpart 'ba
  (xspread
    (apple :p 20 (haste 2 0.5) (pear (freq-mul 1.5) (sus 20)))
    (loop 'bu2 (bd) (~) (sn) (~))))

;; define callback to step part on note 37 
(midi-callback 37 (step-part 'ba))
```

You have to activate it using the `--midi-in <port-num>` at startup. You can check available MIDI ports using `--midi-ports`.
