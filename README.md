# AndroidDSP

FPGA-based DSP with Android front-end

## What is this?
I want to make DSP-based audio processing more portable and more user-friendly. The end goal is a virtual rack you can just hook up to your phone and your guitar, letting you put Steve Lukather's lush 80's rack in your pocket.

FPGAs are incredibly powerful and portable, making them a great platform for DSP. I want to couple that with an Android front-end to give users an intuitive GUI for effects-chain design.

There are already a bunch of Android-based guitar effects out there, but latency remains the Achilles heel of these apps. Offloading the signal processing to a dedicated chip can reduce latency.

## Brainstorm

Components:

- FPGA development board
- ADC
- DAC
- Android with USB-OTG capability

## Some audio effects building blocks
* Function generator (sine, tri, saw, square, white noise). Desirable adjustable parameters for the first 4 functions are amplitude, bias, frequency, and phase delay.

* Mix
```
z[n] = ax[n] + by[n]
```

* Delay

Here's the equation for a simple delay:
```
y_echo[n] = ax[n] + bx[n-T]
```

`a` and `b` determine the mix between the original signal `x[n]` and a delayed signal `x[n-T]`, where `T` is the delay time.

It would be cool/useful to have single-delay and multi-tap delay, with some way of setting the number of taps at runtime.

A number of other guitar effects are delay-based:
- reverb
- chorus
- flanger
- phaser
- stereo widener

Moreover, chorus, flanger, and phaser are all quite similar. They involve mixing the original signal with a delayed signal, where the delay time is determined by a low-frequency oscillator (LFO).

Here are the equations for very basic implementations of each:
* chorus:
```
T[n] = A + Bsin(2 pi f)
y[n] = ax[n] + bx[n-T[n]]
```
Typical chorus uses a delay varying from 40 ms to 60 ms, at a frequency around 0.25 Hz.

* flanger:
```
T[n] = A + Bsin(2 pi f)
y[n] = ax[n] + bx[n-T[n]]
```
Typical flanger uses a delay varying from 0 ms to 10 ms, at a frequency around 0.5 Hz.

* phaser:
```
T[n] = A + Bsin(2 pi f)
y[n] = ax[n] - bx[n-T[n]]
```
Note the sign difference! Phaser settings can be similar to flanger setting and chorus settings.

In all three cases, the LFO doesn't have to be sinusoidal; it can also be a triangle wave, a square wave, or a sawtooth wave.

## Tri-stereo chorus: the Holy Grail of guitar effects
I'm especially keen on replicating the Tri-Stereo Chorus (TSC) effect. After a little digging, I came across this note on the TSC's architecture in a [forum post](http://www.hugeracksinc.com/forum/viewtopic.php?f=1&t=98200&start=15):

```
1) Three separate Analog Delay Lines (Left, Center and Right).

2) Nine different Sinusoidal LFOs, three per channel.
Two fixed (PRESET) and one variable (MANUAL).
```

Another forum member clarified:
```
THAT sound that you're looking for is based on Manual mode,
so "simple" 3 delay line + one LFO per line.
```
