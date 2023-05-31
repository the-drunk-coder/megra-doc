# Arithmetic Events

Every numerical parameter has an associated set of arithmetic events (see tutorial chapter "Event-Arithmetic").

Thus, the `rate` parameters has `rate-mul` (multiplication), `rate-add` (addition), `rate-div` (division) and `rate-sub` (subtraction)
associated.

```lisp
(sx 'foo #t
	(pear (freq-mul 1.5) ;; transpose the incoming event stream a (pure) fifth up by multiplying frequency
		(nuc 'bar (saw 'a2))))
```

```lisp
(sx 'foo #t
	(pear (rate-div 2) ;; transpose the incoming event stream an octave down by dividing playback rate
		(nuc 'bar (violin 'a3 :sus 200))))
```

## Special Events and Shorthands

### Keys 

Specify lookup keys for samples, works with `add` and `sub`.

```lisp
(sx 'ba #t
  (pear
	:p 10 (keys-add 'ff) ;; with a chance of 10%, use a more restricted search
	(nuc 'tra (piano 'a3))))
```

### `random-sample` or `rands` - Pick a Random Sample Every Time

Pick a random sample every time:

```lisp
(sx 'ba #t
  (pear
	(rands) ;; pick a random bassdrum every time
	(nuc 'tra (bd))))
```

---

### `sample-number` or `sno` - Sample Number

Pick a sample by its position in the sample folder.

```lisp
(sx 'ba #t
  (cmp
	(loop 'tro (sno 1) (sno 2) (sno 3)) ;; loop over the first three samples in the folder
	(nuc 'tra (bd)))
)
```

### Transpose

The transpose event multiplies the incoming frequency or playback rate by half-tones (currently there's only equal-tempered tuning).

#### Syntax

`(transpose <half-tones>)` or `(tpo <half-tones>)`.

#### Example:

```lisp
(sx 'foo #t
	(pear (transpose 4) ;; transpose the incoming event stream a major third up
		(nuc 'bar (saw 'a2))))
		
(sx 'foo #t
	(pear (transpose 4) ;; works the same on sample events ...
		(nuc 'bar (violin 'a3 :sus 200))))
```
