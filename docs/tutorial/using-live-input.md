# Using Live Input

With Mégra, you can use live input more or less like a sample. A buffer is recorded continously and can be used
like a regular sample. It's not perfect yet and sometimes there are glitches, but it's a lot of fun.

## Using a Live Input Buffer Directly - `feedr`

Using the `feedr` event, you can read directly from the incoming buffer. The buffer is continously overwritten,
which sometimes leads to glitches. Basically it's a delay line.

```lisp
;; read from beginning:
(sx 'ba #t
	(nuc 'fa (feedr :sus 200 :atk 100 :rel 100)))
	
;; modify initial read point:
(sx 'ba #t
	(nuc 'fa (feedr :sus 200 :atk 100 :rel 100 :start (bounce 0.0 0.5))))
```

All other parameters are the same as samples, such as playback rate, envelope, etc.

You can choose a live input buffer if you have multiple live inputs (specified at startup on the command line):

**BUFFERS ARE COUNTED BEGINNING AT 1**

```lisp
;; read from input 1:
(sx 'ba #t
	(nuc 'fa (feedr 1 :sus 200 :atk 100 :rel 100)))
	
;; read from input 2:
(sx 'ba #t
	(nuc 'fa (feedr 2 :sus 200 :atk 100 :rel 100)))
```

## Freezing the Live Buffer - `freeze` and `freezr`

You can also transfer the current contents of the live input buffer to a persistent buffer. The `freeze` command does that for you.

```lisp
;; copy from first input buffer to first freeze buffer
;; if you omit the number, the first one will be chosen
(freeze 1)

;; if you have multiple input buffers, you can define the source with the :in argument
(freeze 1 :in 2) ;; persist contents of second input buffer

;; you can update the contents while the generator is running !

;; read from first freeze buffer
(sx 'ba #t
	(nuc 'fa (freezr 1 :sus 200 :atk 100 :rel 100)))
```

If you want to add material to a running buffer, instead of overwriting it, use the `freeze-add` function:

```lisp
;; copy from first input buffer to first freeze buffer
(freeze)

;; add material to the first freeze buffer
(freeze-add) 

;; you can update the contents while the generator is running !

;; read from first freeze buffer
;; (no number given, so first buffer is default)
(sx 'ba #t
	(nuc 'fa (freezr :sus 200 :atk 100 :rel 100)))
```
