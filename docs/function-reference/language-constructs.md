# Language Constructs

Apart from the sequencing language, Mégra has some "traditional" programming language constructs that might 
help when interfacing with the "outside world" or in other situations. Most a borrowed from other languages.

Some of these are not stable yet and might change as demands are refined ...

## Arithmetics

Mégra has some basic arithmetic support ...

### Syntax

* Multiplication: `(mul <arg0> ... <argN>)`
* Division: `(div <args>)`
* Addition: `(add <arg0> ... <argN>)`
* Subtraction: `(sub <args>)`
* Modulo: `(mod <args>)`
* Power: `(pow <args>)`
* Max on n: `(max <args>)` - returns arg with the maximum value
* Min on n: `(min <args>)` - returns arg with the minimum value
* Round: `(round n)` (non-lazy)
* Floor: `(floor n)` (non-lazy)
* Ceil: `(ceil n)` (non-lazy)

### Examples: 

```lisp
(add 1 2 3) ;; 6
(pow 2 2) ;; 4
(max 2 4 6) ;; 6
```

The arithmetics are (kinda) lazily evaluated, so you can modify global behaviours.

```lisp
(let tonic 100)

(sx 'ba #t
	(nuc 'root (saw tonic))
	(nuc 'fifth (saw (mul tonic 1.5))))
```

By changing `tonic` you can now change the chord while it's running ...


## `callback` - Define a Callback

Same as `fun`, but for MIDI- and OSC callbacks (see articles about MIDI and OSC to see how to define callbacks).

---

## `concat` - Concatenate Symbols or Strings 

### Syntax 

```
(concat <string_or_sym_1> ... <string_or_sym_N>)
```

The first argument determines the return type (if it's a symbol, then `concat` returns a symbol, if it's a string, then `concat` returns a string).

### Example

```
(concat 'foo "bar" 'baz) ;; returns symbol
(concat "foo" "bar" 'baz) ;; returns string
```
---

## `ev-param`, `ev-tag` and `ev-name` - Access Event Properties

You can extract certain event parameters, i.e. to send them over OSC.

### Example

```
(ev-param :dur (saw 100)) ;; extract duraion
(ev-name (saw 100)) ;; extract event name
(ev-tag 0 (saw 100)) ;; extract first tag 
```

---

## `fun` - Define Functions

### Syntax

```
(fun <function-name> (<arg0> .. <argN)
	<function-body>
)
```

The line between function and macro is very vague in Mégra, i.e. you can do `(fun (name) (fun (concat "to-" name) () (send-to name)))`,
where in the child function definition `name` will be replaced.

### Example

```
(fun square (a)
  (mul a a))

(print (square 2))
```
---

You can also use this to create sound events with certain parameters: 

```
(fun doublesaw (freq)
	(mosc :osc1 'saw :osc2 'saw :freq1 freq :freq2 (add freq 2)))
	
(sx 'foo #t
	(loop 'bar (doublesaw 100) (doublesaw 110) (doublesaw 90) (~)))
```


Furthermore, you can use this feature to create more abstract generator functions:

```lisp
(fun x3 (a)
	(vec a a a))
	
(sx 'ba #t
	(cyc 'foo (x3 (saw 200))))
```

Or even entire generators: 
```lisp
(fun x3 (name a)
	(cyc name a a a))
	
(sx 'ba #t
    (x3 'boo (saw 200))
	(x3 'doo (saw 300)))
```

If you want to define a function without parameters, the intuitive way to do this would be just leaving the parenthesis
empty, but in lisp-fashion you can also pass a `#f` flag:

```lisp
;; either
(fun noparam ()
	(once (saw 100)))

;; or
(fun noparam #f
	(once (saw 100)))
```

You can use the `@rest` and `@all` placeholders if you want to pass on arguments to another function: 

```lisp
;; pass on all arguments from 'foo' to 'saw'
(fun foo () (once (saw @all)))

;; arguments are passed on
(foo 300 :rev 0.2)

;; a chord function for 'cyc' and 'loop'
(fun chord ()
	(vec (vec @all))) ;; pass on all args

(sx 'baa #t
	(loop 'fa (chord (saw 100) (saw 350))))
	
;; pass on rest, freq is a mandatory positional arg
(fun bar (freq)
  (once (saw freq @rest)))

(bar 300) ;; frequency is mandatory
(bar 400 :rev 0.2) ;; the rest is optional
```

## `insert` - Insert Into a Hash Map (also see `map`)

### Syntax

```
(insert <map_name> <key> <value>)
```

Map must exist!

### Example

```
(let foo (map (pair "some" "thing")))

(insert foo "another" "thing")

(print foo)
```

---

## `let` - Define Global Variables

### Syntax

```
(let <name> <value>)
```

Note that `name` can be a symbol or just a random identifier ..

### Example

```
(let freq 100)

(sx 'ba #t
	(nuc 'fu (saw freq)))
```

---

## `map` - Define a Hash Map (Unstable!)

### Syntax 

```
(map (pair <key1> <value1) ... (pair <keyN> <valueN>))
```

### Example 

```
(let foo (map (pair "some" "thing") (pair "another" "thing")))

(print foo)
```

---

## `mapper` - Map Events to Other Events (Unstable!)

The `mapper` function can be used to map events to whatever, i.e. take a note event and turn it into an OSC control event.

### Example:

```lisp
(fun to-piano (event)
    ;; send note converts to ctrl event 
    (ctrl 
      (osc-send 'osc-client "/control/me"
         (ev-param :note event)
         (to-string  (ev-param :dur event)))))
		 
(sx 'ba #t
  (cmp
	(mapper to-piano)
	(nuc 'tra (note 44 4)) ;; midi note 44, quarter
  ))
	  
```

To access certain parameters of an event, there's `ev-param`, `ev-tag` and `ev-name`:

```
(ev-param :dur (saw 100)) ;; extract duraion
(ev-name (saw 100)) ;; extract event name
(ev-tag 0 (saw 100)) ;; extract first tag 
```

---

## `match` - Conditional Branching (Unstable!)
A vague mashup of Common Lisp's `cond` and Rust's `match` ... not sure where it's going, yet ...

### Example

Match the incoming argument precisely:

```lisp
(fun decide (a)
  (match a
    0 (print "ping")
    1 (print "pong")))

(decide 1)
;; (decide 0)
```

You can also specify conditions (all branches that match will be executed):

```lisp
(fun decide (a)
	(match a
		(> 10) (print "ping") ;; larger than 10
		(< 12) (print "pong") ;; smaller than 12
		))
		
(decide 13) ;; pin
;; (decide 11)
;; (decide 9)
```

Available conditions: 

* `(< x)` lesser x
* `(<= x)` lesser or equal x
* `(> x)` greater x 
* `(>= x)` greater or equal x
* `(istype x)` argument is of type x 
  * use `'num` to match numerical types
  * use `'str` to match string type
  * use `'vec` to match vector type
  * use `'sym` to match symbol type
  * use `'map` to match map type

```lisp
(fun va (x)
	(match x
		(istype 'str) (once (sine 600))))
		
(va 10) ;; nothing
(va "10") ;; beep
```

There's currently no default or fallback condition that is executed if no other matches.

---

## `pair` - a Pair of Things

(only used for hash map definition so far, see there ...)

---

## `print` - Print Debug Output

### Example

```
(print "hi")
(print (nuc 'fa (saw 100)))
(print (mul 2 5))
```

---

## `progn` - Execute Several Statements at Once

Very much borrowed from Common Lisp ...

### Syntax 

```
(progn
	<statement0>
	...
	<statementN>)
```
### Example 

```
;; start two contexts at the same time ...
(progn
  (sx 'baz #t (nuc 'foo (saw 200)))
  (sx 'bam #t (nuc 'bar (saw 320))))
```

---

## `push` - Push an Element to a Vector (also see `vec`)

### Syntax 

```
(push <vec_name> <value>)
```

Vector must exist!

### Example 

```
(let hi (vec "h" "e" "l" "l" "o"))

(push hi "jane")

(print hi)
```

---

## `to-string` - Turn Stuff into Strings

This is helpful if you want to send something as a string argument over OSC.

## `vec` - Define a Vector

### Syntax 

```
(vec <val1> ... <valN>)
```

### Example

```
(let hi (vec "h" "e" "l" "l" "o"))

(print hi)
```
---

