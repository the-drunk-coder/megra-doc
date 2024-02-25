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

## `fun` - Define Functions

### Syntax

```
(fun <function-name> (<arg0> .. <argN)
	<function-body>
)
```

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

## `match` - Conditional Branching (Unstable!)
A vague mashup of Common Lisp's `cond` and Rust's `match` ... not sure where it's going, yet ...

### Example

```
(fun decide (a)
  (match a
    0 (print "ping")
    1 (print "pong")))

(decide 1)
;; (decide 0)
```

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

