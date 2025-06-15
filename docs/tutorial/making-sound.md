# Making Sounds

Mégra is a language designed to organize sound, but before we can organize sounds, let's first see how we can make sounds (or sound events).

Mégra has a few integrated synthesizers and allows you to work with samples.

Let's explore the sound events using the `(once)` function, which plays a sound event exactly one time.

## Samples

Sample events are the staple of Mégra sounds. They're organized in a folder system that is explained in the [Using Samples](https://megra-doc.readthedocs.io/en/latest/using-samples/) section above. If you haven't read that section yet, this would be a good time to do so.

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
	:sus 200   ;; set sustain to 200ms (ASR envelope)
)) 
```

See the [Sound Events](https://megra-doc.readthedocs.io/en/latest/function-reference/sound-events/) section for available parameters and their defaults.

## Synthesizers

Mégra has a range of synthesizers available, starting from simple single-oscillator synths to multi-oscillator
synths, a karplus-strong approximation and some glitchy wavetable stuff.

```lisp
(once (saw           ;; <-- sawtooth synth
	      300        ;; <-- frequency
	      :lpf 2000  ;; <-- low-pass filter frequency
	      :rev 0.2   ;; <-- reverb amount
	))
```

See the [Sound Events](https://megra-doc.readthedocs.io/en/latest/function-reference/sound-events/) section in the function reference for all available synths and their parameters.

## Designing Complex Sounds and Instruments

Some instruments, such as the multi-oscillator synth or the KarPlusPlus synth, are fairly complex
and not so easy to use on-the-fly. 

You can design instruments by wrapping them in a function, i.e. to create a fatter sawtooth bass
with multiple oscillators and fixed envelope with 150ms sustain, you can define a function like: 

```lisp
(fun fatsaw (freq) 
  (mosc ;; <-- multi-oscillator synth
    :osc1 'saw :osc2 'saw 
    :freq1 freq :freq2 (lfo~ :r freq (mul freq 1.02) :f (div freq 5))
    :amp1 (env~ :l 0.0 1 0.2 0.0 :t (div 1 1000) (div 150 1000) (div 300 1000))
    :amp2 (env~ :l 0.0 1 0.6 0.0 :t (div 10 1000) (div (sub 150 9) 1000) (div 300 1000))
    :lpf (linramp~ (max 20 (min 15000 (mul freq 30))) 100 :t 0.2)
    :atk 1 :sus 150 :rel 220
    :tags 'fatsaw ;; <-- adding a tag allows you to filter the events further up the chain (in :solo and :block)
    ))
```

Then you can use the function just like any other sound event:

```lisp
(sx 'foo #t
	(cyc 'bar (fatsaw 100) (fatsaw 120) (fatsaw 90) (fatsaw 80)))
```

For the various options when defining functions, see the [Language Constructs](https://megra-doc.readthedocs.io/en/latest/function-reference/language-constructs/) section in the function reference.
