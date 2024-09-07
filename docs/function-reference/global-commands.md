# Global Commands

## `bpm` - Set Default Transition Time

If no time values for the transition between events is given, 200 milliseconds are used, which corresponds to 150 beats per minute. Using the `(bpm)` command, this can be changed, i.e. if you specify 300 bpm, the default duration would be 100ms. Manually given values are not changed, they always superseed the defualt value. 

If you specify a tempo modifier with `tmod`, this is changed, of course.

---

## `clear` - Clear Session

Stops and deletes all present generators.

### Examples

```lisp
(sx 'some #t
  (cyc 'bear "bd ~ hats ~ sn ~ hats ~"))

(sx 'more #t :sync 'some
  (cyc 'bass "saw:100 ~"))

(clear) ;; clear everything
```
---

## `connect-visualizer`

There's an experimental, browser-based visualizer out there for Mégra, that'll be properly documented some day. 

You need some experience with `node` and `npm` (have them installed, etc). 
You can find the visualizer here: https://github.com/the-drunk-coder/megra-visualizer

As a parameter, you can pass `:exclude` followed by a list of tags to exclude certain 
generators from visualization: 

`(connect-visualizer :exclude 'foo 'bar) ;; exclude all generators whose tags contain 'foo or 'bar`

---

## `defpart`

Define parts, which are basically lists of generators that you can name.
Parts are not updated in the running sync contexts, that is, when you
modify the part, you have to re-evaluate the sync context as well.

### Syntax
`(defpart <part-id> <generator-list>)`

### Example

```lisp
(defpart 'drum-and-bass 
  (cyc 'drums "bd ~ sn ~")
  (nuc 'bass (saw 100)))
	
(defpart 'hats-and-mid 
  (cyc 'hats "hats hats ~ hats")
  (nuc 'midrange (saw 1000)))
	
(defpart 'treble-and-cym
  (cyc 'cym "cym ~ cym cym")
  (nuc 'treble (saw 2000)))

;; Define a score, here as a learned one, even 
;; though any other generator might be used.
(sx 'ga #t 
  (learn 'ta 
    :events 
    'a (ctrl (sx 'ba #t 'drum-and-bass))
    'b (ctrl (sx 'ba #t 'hats-and-mid))
    'c (ctrl (sx 'ba #t 'treble-and-cym))
    :sample "ababaabbababcabaababaacabcabac"
    :dur 2400))
```
---

## `defualt-duration` - set the global default duration

Same as `bpm`, but instead of beats per minute you specify a value in milliseconds.

---

## `del` - Configure Global Delay Parameters

Parameters: 

* `:damp-freq` - dampening frequency for the delay
* `:feedback` or `fb` - delay feedback
* `:mix` - default ratio
* `:time` or `:t` - delay time
* `:rate` or `:r` - delay rate 


## `export-dot` - Export to DOT File

Export a generator (or at least its underlying structure) to a DOT file that can be 
rendered with GraphViz.

### Syntax
`(export-dot <filename> <generator> or <keyword> and <tag list>)`

### Example
```
;; if a generator is provided, it will be exported as a DOT file directly
(export-dot "dotdotdot.dot"
	(cyc 'bu "saw ~ saw ~ saw ~"))
	
;; if the keyword "live" and a tag list are provided, all running generators matching the tag list will be exported
(sx 'ba #t 
  (cyc 'bu "bd ~ sn ~"))
  
(export-dot "babu" :live 'ba 'bu)

;; if a part is defined ... 
(defpart 'ga
	(cyc 'du "cym cym cym cym")
	(cyc 'ba "bd ~ sn ~"))
	
;; you can export it using the "part" keyword
(export-dot "partpart" :part 'ga)

```
---

## `global-resources` or `globres` - Global Resources for Lifemodeling algorithm

Specifies the pool of global resources for the primitive lifemodeling algorithm that is integrated in Mégra.

---

## `import-sample-sets` - Import a Sample Sets

See chapter "Using Samples"

---

## `latency` - latency between control- and audio threads.

Default is 50ms or 0.05 seconds. Value is passed in seconds.

If you start Mégra from the command line, and see 'late' messages, you might want to consider using a higher latency, otherwise you don't need to touch this.

---

## `load-file` - import another Mégra file

If you have some functions or instruments defined in a separate file, you can import it with the `load-file` command:

```lisp
(load-file "/path/to/file.megra3")
```
---

## `once` - Play an Event exactly once.

`(once (sine 300 :sus 200))` plays a sine event with a sustain of 200 ms exactly once. Pretty self-explanatory.

---

## `rec` - Record Session

Allows you to record your session directly from Megra.
The file will end up in the recordings folder that lies
next to the sketchbook folder.

Only one recording can be running at a time.

As a current limitation, the number of inputs can't be bigger 
than the number of outputs.

Also, even if there's only one input, the input recording file will have 
the same amount of channels as the output file.

Recording format is 32-bit float WAV.

### Example

```lisp

;; record with prefix, record input
(rec "pref" :input #t)

;; use default prefix, don't record input
(rec)

;; stop recording
(stop-rec)
```
---

## `rev` - Configure Global Delay Parameters

Parameters: 

* `:damp` - dampening frequency for the reverb (not for convolution reverb)
* `:mix` - default ratio
* `:roomsize` - delay room size (not for convolution reverb) 


---

## `step-part` - Evaluate Parts Step by Step

Define a part and evaluate it step by step. This exists mostly for debugging purposes.

### Example

```lisp
(defpart 'ba ;; <-- define some part
  (cyc 'hu "hats cym hats cym cym hats hats cym")
  (cyc 'du "bd ~ sn ~ bd bd sn ~"))

(step-part 'ba) ;; <-- step through ...
```

---

## `sx` - Event Sink

Short for `SyncconteXt`.

### Example

```lisp
(sx 'simple #t
  (nuc 'beat (bd) :dur 400))
  
;; stop the context by setting the flag to false:

(sx 'simple #f ;; <- false !
	(nuc 'beat (bd) :dur 400))

(sx 'simple2 #t :sync 'simple :shift 200
  (nuc 'beat2 (sn) :dur 400))
  
;; you can solo and mute by tag ...
  
(sx 'solo #t :solo 'bd ;; <-- solo all events tagged 'bd'
  (nuc 'snare (sn) :dur 400)
  (nuc 'hats (hats) :dur 400)
  (nuc 'bass (bd) :dur 400))

(sx 'solo #t :block 'bd ;; <-- block all events tagged 'bd'
  (nuc 'snare (sn) :dur 400)
  (nuc 'hats (hats) :dur 400)
  (nuc 'bass (bd) :dur 400))
  
;; If your generators go out of sync due to tempo changes or something,
;; you can explicitly re-sync them: 

(sx 'lol #t :resync #t
	(nuc 'fu (saw 100))
	(nuc 'fa (saw 200)))

;; remove the flag once things are in sync!-
```

---

## `stop-rec`

Stop recording, see above ...

### Syntax

```
(stop-rec)
```

## `tmod` - global time modifier

You can specify a global modifier for times, i.e. to adjust timing or avoid doing all the math in your head.

### Example

Imagine the following code:
```lisp
(sx 'ba #t
    (nuc 'fa :dur 200 (bd)))
```

This repeats the bassdrum with 200ms in between each beat. 

Now, if you specify a global time modifier `(tmod 2.0)`, then the time will always be multiplied by 2, thus,
you'll have a half-time effect.

You can use dynamic parameters on tmod, like `(tmod (fade 1.0 0.3))`, but the result can be quite unpredictable.


