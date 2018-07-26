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

Moreover, chorus, flanger, and phaser are all quite similar. Here are the equations for very basic implementations of each:
* chorus:
```
T[n] = Asin(2 &pi; f)
y[n] = ax[n] + bx[n-T[n]]
```

* flanger:

* phaser:
