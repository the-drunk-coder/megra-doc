# Continuous Modulators

## `linramp~` - Linear Ramp

A simple linear ramp between two points.

### Parameters

* start level (positional)
* end level (positional)
* `:t` or `:time` - ramp time

### Example

Fade frequency of sine oscillator from 100hz to 3000hz over it's 4 second duration. Note 
the difference between sustain (milliseconds) and ramp time (seconds).

The `:t` argument can also be written with the full word `:time`.

```
(once (sine (linramp~ 100 3000 :t 4) :sus 4000)) 
```

---

## `logramp~` - Logarithmic Ramp

A logarithmic ramp between two points.

### Parameters

* start level (positional)
* end level (positional)
* `:t` or `:time` - ramp time

### Example

Fade frequency of sine oscillator from 100hz to 3000hz over it's 4 second duration. Note 
the difference between sustain (milliseconds) and ramp time (seconds).

The `:t` argument can also be written with the full word `:time`.

```
(once (sine (linramp~ 100 3000 :t 4) :sus 4000)) 
```

---

## `expramp~` - Exponential Ramp

An exponential ramp between two points.

### Parameters

* start level (positional)
* end level (positional)
* `:t` or `:time` - ramp time

### Example

Fade frequency of sine oscillator from 100hz to 3000hz over it's 4 second duration. Note 
the difference between sustain (milliseconds) and ramp time (seconds).

The `:t` argument can also be written with the full word `:time`.

```
(once (sine (linramp~ 100 3000 :t 4) :sus 4000)) 
```

---

## `env~` - Multi-Point Envelope

Multi-point envelope. Each envelope segment can be specified as linear, exponential or logarithmic.

### Parameters

* `:l` list of levels 
* `:t` list of times (in seconds, not milliseconds)

### Example

```lisp
;; ADSR envelope on oscillator amplitude
(once (sine 300 :amp (env~ :l 0.0 1.0 0.5 0.5 0.0 :t 0.1 0.2 0.4 0.3)))
```

---

## `lfo~` - Sine LFO

A sinusoidal Lfo.

### Parameters

* `:r` or `:range` - the oscillation range 
* `:f` or `:freq` - the oscillation frequency (in hz)

### Example

```lisp
(once (lftri (lfo~ :r 100 1000 :f 1) :sus 3000))
```

---

## `lftri~` - Trinangle LFO

Same as `lfo~`, but with a triangle wave.

---

## `lfsqr~` - Squarewave LFO

Same as `lfo~`, but with a square wave.

---

## `lfsaw~` - Sawtooth LFO

Same as `lfo~`, but with a sawtooth wave.

---

## `lfrsaw~` - Reverse Sawtooth LFO

Same as `lfo~`, but with a reverse sawtooth wave.

---
