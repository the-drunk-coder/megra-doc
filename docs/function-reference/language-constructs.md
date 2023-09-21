# Language Constructs

Apart from the sequencing language, Mégra has some "traditional" programming language constructs that might 
help when interfacing with the "outside world" or in other situations. Most a borrowed from other languages.

Some of these are not stable yet and might change as demands are refined ...

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

