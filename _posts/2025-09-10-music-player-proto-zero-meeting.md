---
layout: post
title:  Proto Zero, 1st Session
date:   2025-09-10 22:00:00 +0200
author: theo
categories:
  - music player
  - prototype
pin: true
image:
  path: assets/img/proto_zero_debounce2.png
---

## Preamble

Monday we had our first workshop-meeting. Four people attended, and we discussed the project, focusing mostly on hardware. Lots of questions are still open, but we all agreed to experiment more and let answers come as we go. As soon as we could, we started tinkering. It was clear that everyone was eager to get their hands dirty with a soldering iron rather than metaphorically with code.

Fair enough, we all already spend enough time in front of screens. I guess that building tangible things is a good remedy for the pains caused by the concept of "build" in software engineering. Software development borrows a lot of concepts from the concrete world but, literally, only produces invisible electric signals. Eventually there's some sort of physical feedback or, ultimately, an interactive artifact. But most developers will probably relate: programming is an abstract activity; and although good software design can be sublime and lead to mental wonder, it’s also purely an intellectual phenomenon. Physicists, poets might agree.

Anyway, if burned fingers and tin fumes can drive us to tinker bliss, we're definitively on the right track.

Therefore, I accept that if mixing code, Rust in this case, with electronics to output high-fidelity sound in a functional and elegant case is the project description, ultimately it’s maybe only for our group members to express themselves in electronic circuitry and coding excellence. So taking a step back and observing, I think that there is now a place and tools to create, we are able to free up some time for it, and _this is a better objective_. We'll each go at our own pace, and this blog must record every step so _anyone_ can follow, reproduce, fail, restart, and succeed!

Again, open source wins. We are able to share a point in the space-time continuum; now we must share good information beyond our actual meetings, so anyone can (almost) break free from the tougher physical constraints of our universe - catch up at their own pace.

## Prototype Zero, if you dare

The term _Prototype Zero_ isn’t a random pick. To paraphrase [this article](https://hackaday.com/2025/08/09/a-love-letter-to-prototype-zero/), this is...

> The Version that Even Your Own Sweet Mother Isn’t Allowed to See

And yet, here we are, sharing everything including the messy bits! We all seem to love knitting with wires and chips. Also, it’s a great way to learn, try things out, or just forget about how worldwide politics is turning bad.

We need more details than a picture, so I updated [the schematic](https://github.com/berlinrs/verdigris-music-player/blob/main/doc/schematic.pdf) in [the repo](https://github.com/berlinrs/verdigris-music-player/tree/main/doc), wrote a [BoM](https://github.com/berlinrs/verdigris-music-player/blob/main/doc/proto_zero_BoM.ods), and I wish you all good luck!

The space is open Mondays and Wednesdays evening to anyone, starting from 7 p.m. Just join the [Signal group](https://signal.group/#CjQKIPqJR5XPc66FgZ97MxTgu94A9pbxbTgh07e2SX4U-MsDEhBvb5jP1lbELZ8mEaLnNLpb) if you want to book a slot or ask for help.
{: .alert.alert-info}

## Open Questions, Bits of Answers

- It feels that a real color screen would make sense. Touch is an option, but it feels that some of use are attached to physical inputs like buttons, wheels
- Bluetooth A2DP is a clear requirement
- We should add an accelerator as it opens the design to many possibilities
- We'll try to meet at least once a month in person. More info and discussion to come in the Signal channel

## Debouncing

![Proto Zero with probes](assets/img/proto_zero_board_oscillo.webp)

The very evening we met, I struggled setting up the trigger on my oscilloscope, so we didn’t see detailed traces of the encoder signal. The problem is better described with an image, and now that I configured the trigger correctly, you can see that edges are clean, but the signal is bouncing like a ball.

![Prototype Zero Debounce](assets/img/proto_zero_debounce1.png)

This causes interrupts to be triggered more than once by edge changes and confuses the counter. I tried to solve it with software, because it’s partially possible, depending on the speed of rotation of the encoder, but installing a [Schmitt trigger](https://en.wikipedia.org/wiki/Schmitt_trigger) will simplify the process to get a clean [quadrature encoding](https://en.wikipedia.org/wiki/Incremental_encoder#Quadrature_outputs).

![Schmitt trigger](assets/img/schmitt_trigger.webp)

Happy tinkering!
