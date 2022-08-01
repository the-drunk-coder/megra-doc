# Using Wavetables

Mégra has an integrated wavetable synthesizer that is complex enough to have its own chapter in the tutorial.

## Simple Wavetables

Simple wavetables use just a collection of numbers that you (as of now) must enter by hand. 

Here's a simple sawtooth wavetable, where the 9-point wavetable is just the list of values after the `:wt` argument.

```lisp
(sx 'ba #t 
  (nuc 'ta (wtab 200 :wt 1 0.75 0.5 0.25 0 -0.25 -0.5 -0.75 -1)))
```

The cool thing is that every value can be a discrete dynamic parameter. This way, there's plenty of options to 
modify the overtone spectrum over time:

```lisp
;; the uneven ratio of the steps makes things more interesting
(sx 'ba #t 
  (nuc 'ta (wtab 200 :wt 
      (bounce 1 -1 :steps 90) 
      (bounce 0.75 -0.75 :steps 160) 
      (bounce 0.5 -0.5 :steps 210) 
      (bounce 0.25 -0.25 :steps 270) 
      0 
      (bounce -0.25 0.25 :steps 270) 
      (bounce -0.5 0.5 :steps 220)  
      (bounce -0.75 0.75 :steps 160) 
      (bounce -1 1 :steps 90))))
```

## More Complex Wavetables

In the style of "Spectral Morphing", "Waveterrain", "Wavematrix" synthesizers or whatever you want to call them,
the `wmat` synth allows you to do more complex synthesis from 2D wavetables. Those could theoretically be written by hand as well,
even though it'd be quite tedious. Thus, you can generate the wavetables from a random wavefile:

```lisp
;; load wavematrix from a bassoon sample,
;; slice it into a 16x256 matrix
;; the default slicing algorithm tries to avoid 
;; artifacts as much as possible
(load-wavematrix 
  :key 'bass
  :path "PATH-TO/bassoon_a1.flac"
  :size 16 256
  :start 0.2)
  
;; make sure the table index (ti) is within range (0-15)
(sx 'ba #t 
  (nuc 'fu :dur 400 
    (wmat 100 :sus 300 :wm 'bass 
      :ti 4 ;; either specify table index by hand or use an lfo, ramp, etc
	  ;;(lfo~ :init 7 :range 5 :freq 0.2 :op 'add)
	  )))
```


