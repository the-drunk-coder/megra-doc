# Control Events

Control events allow you to schedule parts that you'd otherwise execute manually.

## Example

```lisp
;; Change between two loops 
(sx 'ba #t 
  (infer 'ta :events
    'a (ctrl (sx 'du #t (cyc 'bu "bd ~ sn ~"))) ;; <-- executing the sync context is automated
    'b (ctrl (sx 'du #t (cyc 'bu "cym cym cym cym")))
    :rules 
    (rule 'a 'b 100 1599)
    (rule 'b 'a 100 1599)))
```

## The `ctrl` Function

Executes any function, can be used to conduct execution of generators.

### Parameters

* function

### Syntax

```lisp
(ctrl <function>)
```

### Example

```lisp
;; define some parts
(defpart 'bass 	
		(nuc 'bass (saw 100)))
	
(defpart 'mid 
	(nuc 'midrange (saw 1000)))
	
(defpart 'treble 
	(nuc 'treble (saw 5000)))

;; Define a score, here as a learned one, even 
;; though any other generator might be used.
(sx 'ga #t 
	(learn 'ta 
		:events
		'a (ctrl (sx 'ba #t 'bass))
		'b (ctrl (sx 'ba #t 'mid))
		'c (ctrl (sx 'ba #t 'treble))
		:sample "ababaabbababcabaababaacabcabac"
		:dur 2400))
```
