# Modifying Structure

The sequence generators we have seen so far were relatively simple. Mégra allows to create more complex structures, either immediately at the time you create a structure, or iteratively at runtime.

This becomes especially interesting in combination with layering (see chapter "Layering Sound").

# Modifying Structure at Creation Time

Let's take a look at the loop generator: 

```lisp
(sx 'simple #t
	(loop 'loopy (bd '909) (hats) (sn 'fat) (hats)))
```

How about we add a chance that each event get repeated ? That could be done with the `infer` generator we've seen before, but that would be tedious. There's a shorthand:

```lisp
(sx 'simple #t
	(rep 80 3
		(loop 'loopy (bd '909) (hats) (sn 'fat) (hats))))
```

The `rep` function will add an 80 percent chance of repetition, with a maximum of 3 repetitions per event. In the background, this will construct a more complex markow chain.

You can also "grow" a generator at creation time. The grow algorithm duplicates events and expands the underlying Markov chain following certain principles (see the reference documentation for details)

```lisp
(sx 'ba #t
  (cmp ;; <-- the "cmp" or "compose" operator makes it easier to comment out parts
    (rep 30 3) ;;
    (grown 12 0.2 :method 'loop) ;; grow 12 iterations, max deviance of 0.2 
    (loop 'loopy (bd '909) (hats) (sn 'fat) (hats))))
```

# Modifying Structure at Runtime

You can also modify structures, or their execution speed and order, at runtime. Same as for sound, this can be done
over discrete time steps or probabilistically.

For a full list of modifiers that can be applied, see the "Generator Modifiers" section in the function reference.

## apple - probability-based

```lisp
(sx 'ba #t 
	(apple :p 20 (haste 2 0.5) ;; <-- with a probability of 20%, speed up for 2 steps, cut the duration in half
		(loop 'go (sqr 120) (sqr 180) (~) (sqr 80))))
```

This only modifies the execution speed. You can also modify execution order.
In the following example, we skip ahead 2 steps with a chance of 9%, or rewind 2 steps with a chance of 10%

```lisp
(sx 'ba #t 
	(apple :p 10 (rewind 2) :p 9 (skip 2) 
		(loop 'go (sqr 120) (sqr 180) (~) (sqr 80))))
```

Finally, you can modify the generator itself by adding new information based on its history:

```
(sx 'ba #t
	(apple :p 10 (grow 0.5 :method 'flower) 
		(cyc 'ta (sqr 'c2) (sqr 'a3) (sqr 'e3) (sqr 'f4)))) ;; <-- you can use note names !
```

The 'grow' function takes a sound event from the generator, adds a certain amount of variation (the first argument, wheren 0.0 means no variation and 1.0 means a lot of variation).

Let it run for a while and then check what it looks like:

```
(export-dot "grown" :live 'ba 'ta)
```

## every - step-based

The step-based version actually fills both roles:

```lisp
(sx 'ba #t 
	(every :n 12 (haste 2 0.5) ;; <-- every 12 steps, speed up for 2 steps, cut the duration in half
		(loop 'go (sqr 120) (sqr 180) (~) (sqr 80)")))
```
