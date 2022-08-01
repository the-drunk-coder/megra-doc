# Event Arithmetic

There's a certain arithmetic defined on Mégra sound events, that allow you to achieve a variety of effects.

For every parameter such as `:lvl`, `:rate` etc, there's parameter events defined that can be applied with certain operations.

I.e. if you have a sample event, and combine it with a `(rate-mul 1.5)` event, the original playback rate is multiplied by 1.5. You can execute that operation using the `pear` function (see "Modifying the Event Stream").

```lisp
(sx 'ba #t
	(pear 
	    (rate-mul 1.5)
	    (nuc 'da (violin :sus 400))))
```

The main operations defined for every parameter are replace (no suffix) and `add` `sub` `mul` and `div` (with suffix).

```lisp
(sx 'ba #t
	(pear 
	    (rate 1.5) ;; without suffix, the value is just replaced
	    ;; (rate-add 1.5) ;; value is added
		;; (rate-sub 1.5) ;; value is subtracted (might fail if value gets negative)
		;; (rate-mul 1.5) ;; value is multiplied
		;; (rate-div 1.5) ;; value is divided (no zeros here please !)
	    (nuc 'da :dur 600 (violin :sus 400 :rate 1.0)))) ;; just for clarity
```

Or with another parameter, like pitch frequency:

```lisp
(sx 'ba #t
	(pear 
	    (freq 330) ;; without suffix, the value is just replaced
	    ;; (freq-add 110) ;; value is added
		;; (freq-sub 110) ;; value is subtracted (might fail if value gets negative)
		;; (freq-mul 1.5) ;; value is multiplied
		;; (freq-div 2) ;; value is divided (no zeros here please !)
	    (nuc 'da :dur 600 (sine 220 :sus 400)))) ;; just for clarity
```

Until now, we've only considered simple scalar values combined with other simple scalar values. 

But what happens when we use dynamic parameters ? Let's look at the discrete case first: 

```lisp
(sx 'ba #t
   (pear
       (freq 330) ;; without suffix, the value is just replaced
       ;; (freq-add 110) ;; value is added
	   ;; (freq-sub 110) ;; value is subtracted (might fail if value gets negative)
	   ;; (freq-mul 1.5) ;; value is multiplied
	   ;; (freq-div 2) ;; value is divided (no zeros here please !)
      (nuc 'fa (sine (bounce 300 400))))
   )
```

You could write it the other way round:

```lisp
(sx 'ba #t
   (pear
	  (freq-mul 100) ;; value is multiplied
      (nuc 'fa (sine (bounce 3 4)))))
```

Or even like this:

```lisp
(sx 'ba #t
   (pear
	  (freq-mul (bounce 300 400)) ;; value is multiplied
      (nuc 'fa (sine 1))))
```

Here, the dynamic parameter is evaluated, and the arithmetic operation is performed on the evaluated value.

For continous modifiers, the situation is similar:

```lisp
(sx 'ba #t
  (pear 
    (nuc 'fa :dur 900 (sine (linramp~ 300 400 :t 0.6) :sus 600))))
```

could be written as: 

```lisp
(sx 'ba #t
  (pear (freq-mul (linramp~ 3 4 :t 0.6)) 
    (nuc 'fa :dur 900 (sine 100 :sus 600))))
```

or:

```lisp
(sx 'ba #t
  (pear (freq-mul (linramp~ 150 200 :t 0.6))
    (nuc 'fa :dur 900 (sine 2 :sus 600))))
```

Arithmetic operations only work if at least one parameter is a scalar ... this won't work properly, as it's hard to define meaningful arithmetic operations:

```lisp
(sx 'ba #t
  (pear (freq-div (linramp~ 300 400 :t 0.6)) 
    (nuc 'fa :dur 900 (sine (linramp~ 300 400 :t 0.6) :sus 600))))
```

So in the end it's just a partially defined arithmetic.
