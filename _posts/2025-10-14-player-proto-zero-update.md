---
layout: post
title:  Proto Zero, -Hysteresis, +Async
date:   2025-10-14 01:00:00 +0200
author: theo
categories:
  - music player
  - prototype
pin: true
image:
  path: assets/img/proto_zero_debounce3.png
---

## Hysteresis

As [mentioned earlier]({% post_url 2025-09-10-music-player-proto-zero-meeting %}), the solution for bouncing signals is the Schmitt trigger, which eliminates the need for software debouncing.

![Air wired Schmitt trigger](assets/img/schmitt_trigger_air_wire.webp)

The chip has been added to the circuit with hard-core wire and is suspended in the air. It's fine, remember, it's just a proof-of-concept.

In this image, only the two encoder signals are filtered. Ideally, buttons should be filtered as well, but I've left them unfiltered here to provide an example of software debouncing in action.

As shown in the header and figures below, the _blue_ signal measured at the output of the Schmitt trigger features clean edges compared to the _pink_ signal, which is measured at the filter input. Note the signal inversion: this is how the filter works. Some engineers use two filters in sequence to restore the original polarity by double inversion. We don't need that here. Encoders work fine with inverted inputs; it's just a matter of configuring the code accordingly.

![Clean Signal before and after the Schmitt trigger](assets/img/proto_zero_debounce4.png)

## Embassy

This time, I'm a bit less ashamed to share [this update](https://github.com/berlinrs/verdigris-music-player). It's still early days, but all the peripherals are operational except for sound! Not ideal for a music player...

Right now, we've got a fully working display (no flickering thanks to the framebuffer), 2 buttons, an encoder, an RTC, and a battery management chip. Everything's wired up and running with actual code.

There's also an I2S example to stream sound to the DAC, but I haven't started decoding anything from the SD-card yet.

<video width="100%" controls>
    <source src='/verdigris/assets/vids/encoder_buttons_embassy.mp4'>
</video>

What's still missing:
- The SD card implementation (archived, working code is already in the repo)
- Some decoder(s)...
- Experimenting with DMA to transfer sound data to the DAC

Once these are done, we'll be able to start routing the circuitry and build a real prototype!
