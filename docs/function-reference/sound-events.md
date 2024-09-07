# Sound Events, Control Events and Parameters

## Filter Types

Some events have configurable filters (see below).

The filter types are:

* `'lpf12` - 12dB/oct lowpass filter
* `'lpf18` - 18dB/oct lowpass filter with distortion
* `'lpf24` - 24dB/oct lowpass filter
* `'hpf12` - 12dB/oct highpass filter
* `'hpf24` - 24dB/oct highpass filter
* `'butter2lpf` - 2nd order Butterworth lowpass filter
* `'butter4lpf` - 4th order Butterworth lowpass filter
* `'butter6lpf` - 6th order Butterworth lowpass filter
* `'butter8lpf` - 8th order Butterworth lowpass filter
* `'butter10lpf` - 10th order Butterworth lowpass filter
* `'butter2hpf` - 2nd order Butterworth highpass filter
* `'butter4hpf` - 4th order Butterworth highpass filter
* `'butter6hpf` - 6th order Butterworth highpass filter
* `'butter8hpf` - 8th order Butterworth highpass filter
* `'butter10hpf` - 10th order Butterworth highpass filter
* `'peak` - parametric eq band
* `'none` - no filter

## Oscillator Types

Some events have configurable oscillators (see below).

The oscillator types are:

* `'sine` - sine wave oscillator
* `'tri` - triangle wave oscillator (LF, non-bandlimited, out of tune)
* `'sqr` - square wave oscillator (LF, non-bandlimited, out of tune)
* `'saw` - sawtooth wave oscillator (LF, non-bandlimited, out of tune)
* `'rsaw` - reverse wave oscillator (LF, non-bandlimited, out of tune)
* `'wsaw` - wavetable sawtooth oscillator (LF)
* `'fmsqr` - fm squarewave oscillator (softer)
* `'fmsaw` - fm sawtooth oscillator (softer)
* `'fmtri` - fm triangle oscillator (softe)r
* `'cub` - cubic sine approximation
* `'white` - white noise
* `'brown` - brown noise
* `'wtab` - wavetable oscillator
* `'wmat` - wavematrix oscillator

## Envelope Types

Some events have configurable envelope types (see below).

The envelope types are:

* `'lin` - linear envelope segment
* `'log` - logarithmic envelope segment
* `'exp` - exponential envelope segment
* `'sin` and `'cos` - sinosoidal envelope segments

## Sample Events

Sample events allow you to play a pre-loaded samples, using keywords to specify the sample you need.

### Syntax 

```lisp 
(<sample-type> <keywords> <keyword parameters>)
```

### Example

```lisp
;; choose the 808 sample from the bd folder
(bd 'bd808 :lpf 1000 :rate 0.9)
```

You can change which samples to pick programatically, i.e. using the `keys` modifier.

```lisp
(sx 'ba #t
	(cmp 
		(loop 'fa (keys '808) (keys '909)) ;; switch back and forth between 808 and 909
		(nuc 'tra (bd))))
```

### Parameters

| Parameter | Default                | Description                                                                              | Modulatable |
|-----------|:----------------------:|:----------------------------------------------------------------------------------------:|-------------|
| `:lvl`    | 0.5                    | envelope max level                                                                       | yes         |
| `:amp`    | 0.77                   | sampler (oscillator) amplitude                                                           | yes         |
| `:rate`   | 1.0                    | sample playback rate (negative values for reverse playback)                              | yes         |
| `:start`  | 0.0                    | start within sample file, ratio                                                          |             |
| `:atk`    | 1                      | gain envelope attack, in ms                                                              |             |
| `:dec`    | 1                      | gain envelope decay, in ms (using this turns the envelope into ADSR, otherwise it's ASR) |             |
| `:rel`    | 1                      | gain envelope release, in ms                                                             |             |
| `:sus`    | sample lenght, 10s max | gain envelope sustain, in ms                                                             |             |
| `:atkt`   | attack type            | (see envelope types)                                                                     |             |
| `:dect`   | decay type             | (see envelope types)                                                                     |             |
| `:relt`   | release type           | (see envelope types)                                                                     |             |
| `:pos`    | 0.0                    | stereo position (-1.0 left, 0.0 center 1.0 right) or channel number in multichannel mode | yes         |
| `:lpf`    | 19000                  | lowpass filter frequency                                                                 | yes         |
| `:lpt`    | 'lpf18                 | lowpass filter type (see filter types)                                                   |             |
| `:lpq`    | 0.4                    | lowpass filter q factor                                                                  | yes         |
| `:lpd`    | 0.0                    | lowpass filter distortion                                                                | yes         |
| `:hpf`    | 20                     | highpass filter frequency                                                                | yes         |
| `:hpt`    | 'hpf12                 | highpass filter type (see filter types)                                                  |             |
| `:hpq`    | 0.4                    | highpass filter q factor                                                                 | yes         |
| `:pff`    | 1000                   | peak filter 1 frequency                                                                  | yes         |
| `:pfq`    | 10                     | peak filter 1 q factor                                                                   | yes         |
| `:pfg`    | 0.0                    | peak filter 1 gain                                                                       | yes         |
| `:pft`    | 'peak                  | peak filter 1 type                                                                       |             |
| `:pff1`   | 1000                   | peak filter 1 frequency                                                                  | yes         |
| `:pfq1`   | 10                     | peak filter 1 q factor                                                                   | yes         |
| `:pfg1`   | 0.0                    | peak filter 1 gain                                                                       | yes         |
| `:pft1`   | 'peak                  | peak filter 1 type                                                                       |             |
| `:pff2`   | 1000                   | peak filter 2 frequency                                                                  | yes         |
| `:pfq2`   | 10                     | peak filter 2 q factor                                                                   | yes         |
| `:pfg2`   | 0.0                    | peak filter 2 gain                                                                       | yes         |
| `:pft2`   | 'none                  | peak filter 2 type                                                                       |             |
| `:rev`    | 0.0                    | reverb amount                                                                            | yes         |
| `:del`    | 0.0                    | delay amount                                                                             | yes         |
| `:dist`   | 0                      | distortion (simple cubic waveshaping)                                                    |             |
| `:tags`   | none                   | additional tags                                                                          |             |


## Live Buffer Events

Live buffer events allow you to play with the live input buffer. The live input buffer is continously recording the last 3 seconds 
(or more or less, if specified at startup) of input of the first input channel. Now there's severaly events that allow you to 
work with it.

### `feedr` - read the live buffer like a regular sample
Using `(feedr)`, you can read from a live buffer directly. 
The first number specifies the input buffer to read from (if there's more than one). If you don't provide the input buffer, the first one is chosen.

**BUFFER NUMBERS ALWAYS START AT 1**

All the parameters are the same as in the sample events above. Be careful 
about feedback if you have an open mic !

*Example*:
```lisp
(sx 'ba #t ;; read from first live buffer at varying starting points
  (nuc 'fa (feedr 1 :start (bounce 0.01 0.99))))
```

### `freeze` and `freeze-add` - freeze the live buffer
This event writes the current contents of a live buffer (as-is) to a persistent buffer specified by a number, like `(freeze 1)`, which writes the default input buffer (the first one) to a persistent buffer. You can omit the number if you want to use the default buffer, just write `(freeze)`.

If you want to add the contents of a live buffer to a persistent buffer, you can use `(freeze-add)`.

If you're using multiple input buffers, you can specify the source like `(freeze 1 :in 2)`, which writes the contents of buffer 2 to freeze buffer 1.

**BUFFER NUMBERS ALWAYS START AT 1**

If you use this in a `ctrl` event, you can periodically update the content of the frozen buffer.

```lisp
;; freeze once, to buffer 1
(freeze 1)

;; freeze once, to buffer 1 (that's the default buffer, if you want to use it you can omit the number)
(freeze)

;; add more sound material to the incoming buffer
(freeze-add)

;; freeze periodically, every 6 seconds
(sx 'ba #t 
  (nuc 'ta :dur 6000 (ctrl (freeze 1))))
```

### `freezr` - read from frozen buffers
This allows you to read from the buffer previously frozen with `freeze`. You can use it like any regular sample.
First argument specifies the buffer to be read from: `(freezr <bufnum>)`. The number can be omitted to choose the default buffer.

```lisp
(sx 'ba #t ;; granular sampling on freeze buffer 1 ...
  (nuc 'ba :dur 100 (freezr 1 :start (bounce 0.0 1.0) :atk 1 :sus 100 :rel 100)))
```

## Single-Oscillator Synth Events 

These are some very naive implementations of non-bandlimited waveforms (except for the sine wave). Some are pretty dirty and are not even tuned that well, but have a certain quality to them.

### Syntax

```lisp 
(sine|saw|sqr|cub|tri|fmtri|fmsaw|fmsqr|white|brown <pitch> <keyword parameters>)
```

### Example

```lisp
(sine 110) ;; with frequency
(sine 'a2 :rev 0.1) ;; with note name and reverb
```

### Types

| Type  | Description                                                         |
|-------|:-------------------------------------------------------------------:|
| sine  | simple sine wave                                                    |
| cub   | a sine like shape made of two cubic pieces (LFCub in SuperCollider) |
| tri   | a triangle wave                                                     |
| sqr   | a square wave                                                       |
| saw   | a sawtooth wave                                                     |
| fmtri | an FM approximation of a triangle wave                              |
| fmsqr | an FM approximation of a square wave                                |
| fmsaw | an FM approximation of a sawtooth wave                              |
| white | white noise                                                         |
| brown | brown noise                                                         |
| blit  | naive blit                                                          |


### Parameters

| Parameter | Default      | Description                                                                              |
|-----------|:------------:|:----------------------------------------------------------------------------------------:|
| pitch     | 100          | pitch - might be frequency in hertz or quoted note name                                  |
| `:lvl`    | 0.5          | envelope level                                                                           |
| `:amp`    | 0.6          | oscillator amplitude                                                                     |
| `:atk`    | 1            | gain envelope attack, in ms                                                              |
| `:dec`    | 1            | gain envelope decay, in ms (using this turns the envelope into ADSR, otherwise it's ASR) |
| `:rel`    | 100          | gain envelope release, in ms                                                             |
| `:sus`    | 48           | gain envelope sustain, in ms                                                             |
| `:atkt`   | attack type  | (see envelope types)                                                                     |
| `:dect`   | decay type   | (see envelope types)                                                                     |
| `:relt`   | release type | (see envelope types)                                                                     |
| `:pos`    | 0.0          | (see above)                                                                              |
| `:lpf`    | 19000        | lowpass filter frequency                                                                 |
| `:lpt`    | 'lpf18       | lowpass filter type (see filter types)                                                   |
| `:lpq`    | 0.4          | lowpass filter q factor                                                                  |
| `:lpd`    | 0.0          | lowpass filter distortion                                                                |
| `:hpf`    | 20           | highpas filter frequency                                                                 |
| `:hpt`    | 'hpf12       | highpass filter type (see filter types)                                                  |
| `:hpq`    | 0.4          | highpass filter q factor                                                                 |
| `:rev`    | 0.0          | reverb amount                                                                            |
| `:del`    | 0.0          | delay amount                                                                             |
| `:pw`     | 0.5          | pulsewidth (ONLY `sqr`)                                                                  |
| `:tags`   | none         | additional tags                                                                          |
| `:dist`   | 0            | distortion (simple cubic waveshaping)                                                    |
| `:bcbits` | 24           | bitcrusher bitrate                                                                       |
| `:bcdown` | 0            | bitcrusher downsampling factor                                                           |
| `:bcmode` | ???          | bitcrusher mode (floor, ceil, round)                                                     |
| `:bcmix`  | 0.0          | bitcrusher mix                                                                           |


## Multi-Oscillator Event

`mosc` - A synthesizer with up to four configurable oscillators, rest the same as single oscillator event.

**Syntax Example**:

```lisp
;; a synth with two sawtooth oscillators, spaced an octave apart
(once (mosc :osc1 'saw :osc2 'saw :freq1 100 :freq2 200))
```

| Parameter           | Default      | Description                                                                              |
|---------------------|:------------:|:----------------------------------------------------------------------------------------:|
| `:osc1` - `osc4`    | NONE         | oscillator 1-4 type                                                                      |
| `:freq1` - `:freq4` | NONE         | oscillator 1-4 frequency                                                                 |
| `:lvl`              | 0.5          | overall level                                                                            |
| `:amp1`- `:amp4`    | 0.6          | oscillator 1-4 amplitude                                                                 |
| `:atk`              | 1            | gain envelope attack, in ms                                                              |
| `:dec`              | 1            | gain envelope decay, in ms (using this turns the envelope into ADSR, otherwise it's ASR) |
| `:rel`              | 100          | gain envelope release, in ms                                                             |
| `:sus`              | 48           | gain envelope sustain, in ms                                                             |
| `:atkt`             | attack type  | (see envelope types)                                                                     |
| `:dect`             | decay type   | (see envelope types)                                                                     |
| `:relt`             | release type | (see envelope types)                                                                     |
| `:pos`              | 0.0          | (see above)                                                                              |
| `:lpf`              | 19000        | lowpass filter frequency                                                                 |
| `:lpt`              | 'lpf18       | lowpass filter type (see filter types)                                                   |
| `:lpq`              | 0.4          | lowpass filter q factor                                                                  |
| `:lpd`              | 0.0          | lowpass filter distortion                                                                |
| `:hpf`              | 20           | highpas filter frequency                                                                 |
| `:hpt`              | 'hpf12       | highpass filter type (see filter types)                                                  |
| `:hpq`              | 0.4          | highpass filter q factor                                                                 |
| `:rev`              | 0.0          | reverb amount                                                                            |
| `:del`              | 0.0          | delay amount                                                                             |
| `:pw1` - `pw4`      | 0.5          | pulsewidth (ONLY `sqr`)                                                                  |
| `:tags`             | none         | additional tags                                                                          |
| `:dist`             | 0            | distortion (simple cubic waveshaping)                                                    |
| `:bcbits`           | 24           | bitcrusher bitrate                                                                       |
| `:bcdown`           | 0            | bitcrusher downsampling factor                                                           |
| `:bcmode`           | ???          | bitcrusher mode (floor, ceil, round)                                                     |
| `:bcmix`            | 0.0          | bitcrusher mix                                                                           |

## KarPlusPlus

A Karplus-Strong approximation.

**Syntax**: 
```lisp 
(kpp <pitch> <keyword parameters>)
```
### Parameters 

| Parameter | Default             | Description                                                                              |
|-----------|:-------------------:|:----------------------------------------------------------------------------------------:|
| `:osct`   | NONE                | source (burst) oscillator type                                                           |
| `:ft`     | 'lpf18              | main filter type                                                                         |
| `:freq1`  | NONE                | source (burst )oscillator frequency                                                      |
| `:amp`    | 1.0                 | pre-waveshaper gain                                                                      |
| `:lvl`    | 0.5                 | overall level                                                                            |
| `:amp1`   | 0.6                 | source (burst) oscillator amplitude                                                      |
| `:delft`  | DUMMY               | dampening filter type for delay                                                          |
| `:delfb`  | DUMMY               | feedback for delay                                                                       |
| `:deldf`  | (depends on filter) | delay dampening frequency                                                                |
| `:atk`    | 1                   | gain envelope attack, in ms                                                              |
| `:dec`    | 1                   | gain envelope decay, in ms (using this turns the envelope into ADSR, otherwise it's ASR) |
| `:rel`    | 100                 | gain envelope release, in ms                                                             |
| `:sus`    | 48                  | gain envelope sustain, in ms                                                             |
| `:atkt`   | attack type         | (see envelope types)                                                                     |
| `:dect`   | decay type          | (see envelope types)                                                                     |
| `:relt`   | release type        | (see envelope types)                                                                     |
| `:pos`    | 0.0                 | stereo position                                                                          |
| `:pw1`    | 0.5                 | source (burst) pulsewidth (ONLY `sqr`)                                                   |
| `:tags`   | none                | additional tags                                                                          |
| `:dist`   | 0                   | distortion (simple cubic waveshaping)                                                    |
| `:rev`    | 0.0                 | reverb amount                                                                            |
| `:del`    | 0.0                 | delay amount                                                                             |
	
**Filter Parameters**

Depend on configured filter, i.e. if it's a lowpass:

| Parameter | Default | Description               |
|-----------|:-------:|:-------------------------:|
| `:lpf`    | 19000   | lowpass filter frequency  |
| `:lpq`    | 0.4     | lowpass filter q factor   |
| `:lpd`    | 0.0     | lowpass filter distortion |

Or a highpass:

| Parameter | Default | Description              |
|-----------|:-------:|:------------------------:|
| `:hpf`    | 20      | highpas filter frequency |
| `:hpq`    | 0.4     | highpass filter q factor |


Or a peak filter:

| Parameter | Default | Description           |
|-----------|:-------:|:---------------------:|
| `:pff`    | 1000    | peak filter frequency |
| `:pfq`    | 10      | peak filter  q factor |
| `:pfg`    | 0.0     | peak filter  gain     |


## Risset Event

A simple risset bell event.

**Syntax**: 
```lisp 
(risset <pitch> <keyword parameters>)
```

**Example** 
```lisp
(risset 2000) ;; with frequency
(risset 'a5 :rev 0.1) ;; with note name and reverb
```
### Parameters

| Parameter | Default | Description                                             |
|-----------|:-------:|:-------------------------------------------------------:|
| pitch     | 100     | pitch - might be frequency in hertz or quoted note name |
| `:lvl`    | 0.5     | gain level                                              |
| `:atk`    | 5       | gain envelope attack, in ms                             |
| `:dec`    | 20      | gain envelope decay, in ms                              |
| `:sus`    | 50      | gain envelope sustain, in ms                            |
| `:rel`    | 5       | gain envelope release, in ms                            |
| `:pos`    | 0.0     | see above                                               |
| `:lpf`    | 19000   | lowpass filter frequency                                |
| `:lpt`    | 'lpf18  | lowpass filter type (see filter types)                  |
| `:lpq`    | 0.4     | lowpass filter q factor                                 |
| `:lpd`    | 0.0     | lowpass filter distortion                               |
| `:rev`    | 0.0     | reverb amount                                           |
| `:del`    | 0.0     | echo amount                                             |
| `:tags`   | none    | additional tags                                         |

## Wavetable Synth

The wavetable synth (`wtab`) is a simple wavetable synth that allows you to create and manipulate a single wavetable "by hand" using the `:wt` argument.

Interpolation for the wavetables is 3rd-order hermite. All other parameters are the same as on the single-oscillator synth above.

### Example

Here's a simple sawtooth wavetable, where the 9-point wavetable is just the list of values after the `:wt` argument.

```lisp
(sx 'ba #t 
  (nuc 'ta (wtab 200 :wt 1 0.75 0.5 0.25 0 -0.25 -0.5 -0.75 -1)))
```

The cool thing is that every value can be a discrete dynamic parameter. This way, there's plenty of options to 
modify the overtone spectrum over time:

```lisp
;; the uneven ratio of the steps makes things more interesting
(sx 'ba #t 
  (nuc 'ta (wtab 200 :wt 
      (bounce 1 -1 :steps 90) 
      (bounce 0.75 -0.75 :steps 160) 
      (bounce 0.5 -0.5 :steps 210) 
      (bounce 0.25 -0.25 :steps 270) 
      0 
      (bounce -0.25 0.25 :steps 270) 
      (bounce -0.5 0.5 :steps 220)  
      (bounce -0.75 0.75 :steps 160) 
      (bounce -1 1 :steps 90))))
```

## Wavematrix Synth

The wavematrix (`wmat`) synth is a powerful synth in the style of "spectral morphing" synths such as Vital or Serum. 

It allows you to do more complex synthesis from 2D wavetables. Those could theoretically be written by hand as well,
even though it'd be quite tedious. Thus, you can generate the wavetables from a random wavefile using the `load-wavematrix`
function:

```
;; load wavematrix from a bassoon sample,
;; slice it into a 16x256 matrix
;; the default slicing algorithm tries to avoid 
;; artifacts as much as possible
(load-wavematrix 
  :key 'bass
  :path "PATH-TO/bassoon_a1.flac"
  :size 16 256
  :start 0.2)
```

Interpolation for the wavetables is 3rd-order hermite. All other parameters are the same as on the single-oscillator synth above.

### Example

```lisp
;; load wavematrix from a bassoon sample,
;; slice it into a 16x256 matrix
;; the default slicing algorithm tries to avoid 
;; artifacts as much as possible
(load-wavematrix 
  :key 'bass
  :path "PATH-TO/bassoon_a1.flac"
  :size 16 256
  :start 0.2)
  
;; make sure the table index (ti) is within range (0-15)
(sx 'ba #t 
  (nuc 'fu :dur 400 
    (wmat 100 :sus 300 :wm 'bass 
      :ti 4 ;; either specify table index by hand or use an lfo, ramp, etc
	  ;;(lfo~ :init 7 :range 5 :freq 0.2 :op 'add)
	  )))
```

## A Note about Note Names
Note names follow the Common Music 2.x convention, where `'a4` is the concert pitch of 440hz, above the *middle c* which is `'c4`. `'cs4` denotes a sharp note, `'cf4` denotes a flat note. The sharp/flat schema is consistent over all notes.

## A Note about Event Tags
Each events contain certain tags by default, such as the event type and the search tags in the case of sample events.
As you might have seen, you can add custom tags. All tags can be used to filter in the respective applicators or modifiers,
as well as to solo and mute in the sync context. Here's an example:

```lisp
(sx 'ba #t :solo 'bass ;; <-- you can solo or block based on the custom tags
	(nuc 'fa (bd :tags 'drums) (sn :tags 'drums) (saw 100 :tags 'bass)))

(sx 'ba #t
	(pear :for 'drums (rev 0.2) ;; <-- only apply reverb to drums
		(nuc 'fa (bd :tags 'drums) (sn :tags 'drums) (saw 100 :tags 'bass))))
```
