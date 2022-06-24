# Continuous Modulators

## `linramp~` - Linear Ramp

A simple linear ramp between two points.

## Parameters

* start level (positional)
* end level (positional)
* `:t` or `:time` - ramp time

## Example

Fade frequency of sine oscillator from 100hz to 3000hz over it's 4 second duration. Note 
the difference between sustain (milliseconds) and ramp time (seconds).

The `:t` argument can also be written with the full word `:time`.

```
(once (sine (linramp~ 100 3000 :t 4) :sus 4000)) 
```

## `logramp~` - Logarithmic Ramp

A logarithmic ramp between two points.

## Parameters

* start level (positional)
* end level (positional)
* `:t` or `:time` - ramp time

## Example

Fade frequency of sine oscillator from 100hz to 3000hz over it's 4 second duration. Note 
the difference between sustain (milliseconds) and ramp time (seconds).

The `:t` argument can also be written with the full word `:time`.

```
(once (sine (linramp~ 100 3000 :t 4) :sus 4000)) 
```

## `expramp~` - Exponential Ramp

An exponential ramp between two points.

## Parameters

* start level (positional)
* end level (positional)
* `:t` or `:time` - ramp time

## Example

Fade frequency of sine oscillator from 100hz to 3000hz over it's 4 second duration. Note 
the difference between sustain (milliseconds) and ramp time (seconds).

The `:t` argument can also be written with the full word `:time`.

```
(once (sine (linramp~ 100 3000 :t 4) :sus 4000)) 
```

## `env~` - Multi-Point Envelope

Multi-point envelope. Each envelope segment can be specified as linear, exponential or logarithmic.

## `lfo~` - Sine LFO

## `lftri~` - Trinangle LFO

## `lfsqr~` - Squarewave LFO

## `lfsaw~` - Sawtooth LFO

## `lfrsaw~` - Reverse Sawtooth LFO
