# Discrete Parameter Modifiers

## `brownian` - Bounded Brownian Motion 

Define a bounded brownian motion on a parameter.

### Parameters

* lower boundary (float)
* upper boundary (float)
* `:wrap` (boolean) (t) - wrap value if it reaches lower or upper boundary
* `:step` (float) (0.1) - step that the parameter will be incremented/decremented

### Syntax

```lisp
(brownian <lower boundary> <upper boundary> :wrap <wrap> :step <step-size>)
```

### Examples
	
```lisp
(sx 'some #t
    (cmp (pear (rate (brownian 0.8 1.2)))
         (nuc 'bass (saw 120 :dur 200))))
```

---

## `bounce` - Parameter Oscillator

Define oscillation on any parameter. The oscillation curve is a bit bouncy, not really sinusoidal.

### Parameters 

* upper limit - upper limit for oscillation 
* lower limit - lower limit for oscillation 
* `:cycle` - oscillation cycle length in steps

### Syntax

```lisp
(bounce <upper limit> <lower limit> :cycle <cycle length in steps>)
```

### Example

```lisp
(sx 'simple #t
  (nuc 'beat (bd) :dur (bounce 200 600 :steps 80)))
```

---

## `env` - Discrete Envelope

Define an envelope on any parameter. Length of list of levels must be one more than length of list of durations.
Durations are step based, so the absolute durations depend on the speed your generator runs at.

### Parameters

* `:v` or `:values` - level points on envelope path
* `:s` or `:steps` - transition durations (in steps)
* `:repeat` (boolean) - loop envelope 

### Syntax

```lisp
(env :values/:v <levels> :steps/:s <durations> :repeat <#t/#f>)
```

### Example

```lisp
(sx 'simple #t
    (cmp 
        (pear (lvl (env :v 0.0 0.4 0.0 :s 20 30)))
        (cyc 'beat "bd ~ hats ~ sn ~ hats ~")))
```

---

## `fade` - Parameter Fader

Fade a parameter (sinusoidal).

### Syntax

`(fade <from> <to> :steps <steps>)`

### Example
```lisp

;; fade cutoff frequency
(sx 'osc #t
    (nuc 'ill (saw 300 :lp-freq (fade 300 2200 :steps 20))))

;; same, but a different position 
(sx 'osc #t
    (cmp
     (pear (lpf (fade 300 2200 :steps 4)))
     (nuc 'ill (saw 300))))

;; fade duration
(sx 'osc #t
    (nuc 'ill (saw 300) :dur (fade 100 400)))

;; fade probablility
(sx 'osc #tt
    (cmp
     (pear :p (fade 0 100) (lvl 0.0))
     (nuc 'ill (saw 300))))
```

---

## `random-sample` or `rands` - Pick a Random Sample Every Time

Pick a random sample every time:

```lisp
(sx 'ba #t
  (pear
	(rands) ;; pick a random bassdrum every time
	(nuc 'tra (bd))))
```

---

## `sample-number` or `sno` - Sample Number

Pick a sample by its position in the sample folder.

```lisp
(sx 'ba #t
  (cmp
	(loop 'tro (sno 1) (sno 2) (sno 3)) ;; loop over the first three samples in the folder
	(nuc 'tra (bd)))
)
```

---

## `transpose` or `tpo` - Transpose 

Transpose by a number of half-notes (equal temperament tuning).

Works on both frequency and playback rate.

### Examples 

```lisp
;; transpose up a minor third
(sx 'ba #t
	(pear (tpo 3) (nuc 'da (violin 'a3))))
	
;; create a chord
(sx 'ba #t
	(xspread
        (pear (tpo 5))
        (pear (tpo 3))
		(nuc 'da (violin 'a3))))	
```

