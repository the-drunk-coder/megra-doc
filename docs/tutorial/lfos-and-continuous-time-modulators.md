# LFOs and other Continuous Time Modulators

As of version 0.1.0, Mégra has continous-time modulators for certain parameters (not for all of them).

So, instead of using discrete-time dynamic parameters:

```lisp
(sx 'ba #t 
	(nuc 'fu (saw (bounce 100 200)))) ;; bounces frequency between 100 and 200 hz
```

you can now write things like"


```lisp
(sx 'ba #t 
	(nuc 'fu (saw (linramp~ 100 200 :time 0.2)))) ;; fade frequency from 100 to 200 over the course of the 200ms event
```

Continous-time modulators have the `~` suffix (borrowed from the PD/Max languages), 
time values are in seconds (as opposed to milliseconds in the discrete-time domain)

You can use the modulators in a nested fashion:


```lisp
(sx 'ba #t 
	(nuc 'fu :dur 1000 (sine (lfo~ :range 100 200 :freq 0.8) :sus 800))) ;; modulate frequency with fixed frequency of 0.6 hz
	
(sx 'ba #t 
	(nuc 'fu :dur 1000 (sine (lfo~ :range 100 200 :freq (linramp~ 0.6 0.2 :t 0.8))))) ;; slow down modulation frequency
```
