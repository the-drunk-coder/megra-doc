# Shorthand Syntax for `loop` and `cyc`

Both `loop` and `cyc` accept a string-based, shorthand syntax, in case you need to be extra fast.

Let's look at this loop:

```lisp
(sx 'foo #t
	(loop 'bar (bd) (hats) (bd) (sn)))
```

Not too terrible, but sometimes writing all the parenthesis out can be confusing.

Thus, in **shorthand syntax**, you can write the same thing like this:

```lisp
(sx 'foo #t
	(loop 'bar "bd hats sn hats"))
```

Same works with `cyc`:

```lisp
(sx 'foo #t
	(cyc 'bar "bd hats sn hats"))
```

In a `loop`, you can provide **custom durations**. The durations are preceded by a `/`, and they are ignored in `cyc`:

```lisp
(sx 'foo #t
	(loop 'bar "bd /400 hats sn /100 hats"))
```

You can provide **arguments**, both **positional** and **named** (eventhough that might not be
the shortest way to do this, using `pear` might be shorter), separated by a colon:

```lisp
(sx 'foo #t
  (loop 'bar "saw:100 saw:200 ~ saw:300:lpf=8000"))
```

You can also create **chords**: 

```lisp
(sx 'foo #t
  (loop 'bar "saw:100 [saw:200 saw:500] ~ saw:300:lpf=8000"))
```

There's even more abbreviations. If you want to write a melody without repeating the 
synth name every time, you can use **mapping**. Note that you need to specify the mapping
**before** the shorthand string. Also, as of version 0.0.8, this doesn't work with chords.

```lisp
(sx 'foo #t
  (loop 'ball :map 'saw "100 200 300 400"))
```

Lastly, you can define **event aliases**. Again, these need to be specified before the 
sequence string:

```lisp
(sx 'too #t
  (loop 'tra :events 'a (bd '909) 'b (sn 'dub) "'a 'b ~ ~ 'a 'b ~ ~'a 'b ~ ~"))
```

You can mix and match the two approaches, but make sure it doesn't get too messy. It all gets
evaluated into one long list of events:

```lisp
;; a very long loop
(sx 'too #t
  (loop 'tra 
	:events 'a (bd '909) 'b (sn 'dub) "'a 'b ~ ~ 'a 'b ~ ~'a 'b ~ ~" // event aliases work for the subsequent sequences
	(bd) (hats) (bd) (hats) ;; plain s-expressions will be added to the loop
	:map 'saw "100 200 300" ;; a mapped melody after that
	(bd) (hats) (bd) (hats)
	"bd /400 sn /400"
	))
```

