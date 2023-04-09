# Making Sounds

Mégra is a language designed to organize sound, but before we can organize sounds, let's first see how we can make sounds (or sound events).

Mégra has a few integrated synthesizers and allows you to work with samples.

Let's explore the sound events using the `(once)` function, which plays a sound event exactly one time.

## Samples

Sample events are the staple of Mégra sounds. They're organized in a folder system that is explained in the "Using Samples" section above.

Without any arguments, a random sample will be chosen from the specified category (which contains all the samples in the respective folder):

```lisp
(once (violin)) ;; this will pick a random sample from the "violin" folder
```

You can provide keywords to pick specific samples:

```lisp
(once (violin 'a4)) ;; this will pick the "a4.flac" sample from the "violin" folder
```

Parameters can be set using keywords (which start with a `:`):

```lisp
(once (violin 'a4
	:rev 0.2   ;; reverb amount
	:rate 1.2  ;; playback rate (affects pitch)
	:lpf 4000  ;; low-pass filter cutoff
)) 
```

See the "Sound Events" section for all parameters and available synths.

## Synthesizers

The synthesizers are fairly simple, single-oscillator type synths with a pre-defined effects chain.

```lisp
(once (saw           ;; <-- sawtooth synth
	      300        ;; <-- frequency
	      :lpf 2000  ;; <-- low-pass filter frequency
	      :rev 0.2   ;; <-- reverb amount
	))
```
See the "Sound Events" section for all parameters and available synths.
