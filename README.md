# AndroidDSP

FPGA-based DSP with Android front-end

## What is this?
I want to make DSP-based audio processing more portable and more user-friendly. The end goal is a device you can just hook up to your phone, letting you put Steve Lukather's lush 80's rack in your pocket.

FPGAs are incredibly powerful and portable, making them a great platform for DSP. I want to couple that with an Android front-end to give users an intuitive GUI for effects-chain design.

There are already a bunch of Android-based guitar effects out there, but latency remains the Achilles heel of these apps. Offloading the signal processing to a dedicated chip can reduce latency.

## Brainstorm

Components:

- FPGA development board
- ADC
- DAC
- Android with USB-OTG capability
