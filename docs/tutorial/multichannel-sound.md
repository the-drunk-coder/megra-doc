# Multichannel Sound

Mégra, as of version 0.0.5, supports the following modes:

* Stereo
* 4 Channel
* 8 Channel

The channel numbers can't be changed at runtime, they need to be configured at startup using the `-o` flag, with the options being `stereo`, `4ch` and `8ch`.

## Panning

Panning works a bit different depending on the output mode. In stereo mode, the panning range (using the `pos` argument for position) is in the range `[-1, 1]`, where -1 is all the way to the left and 1 is all the way to the right.

In multichannel modes, the `pos` argument simple takes the channel numbers beginning at 0.

## Reverb

When convolution reverb is used, the convolution will be performed on each output channel.

The standard reverb is a pairwise multichannel extension of classic Freeverb V2.


