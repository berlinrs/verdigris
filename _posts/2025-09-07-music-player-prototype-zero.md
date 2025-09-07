---
layout: post
title:  Music Player, Prototype Zero
date:   2025-09-07 23:00:00 +0200
author: theo
categories:
  - music player
  - prototype
pin: true
image:
  path: assets/img/proto_zero_1.webp
---

As announced in [this first post]({% post_url 2025-07-30-declaration-of-intent %}), the plan here is to build a platform where we all can learn a little hardware, a little Rust, whatever interests you!

Let me introduce [The Prototype Zero](https://github.com/berlinrs/verdigris-music-player): a fun work-in-progress that won’t fit in your pocket, might give you an electric shock, and is definitely _not water resistant_! It’s no iPod in looks, but it probably makes a great learning platform.

If you’re brave (or just curious), try replicating it and have some fun. There’s still plenty to improve — so maybe watch as things evolve, unless you enjoy a bit of risk...

Assembly was quick and painless, so I soon found myself wading into the part I knew least: **coding Rust for embedded**. And that’s tough! I started almost from scratch, some tutorials, a dash of LLM sparring, and... time and patience. Cue the emotional rollercoaster of fighting the borrow checker and wrangling lifetimes. Pretty standard beginner Rust stuff, I think.

The main struggle? Figuring out the right pattern: how to get shared access to the variables representing the hardware. Rust’s HAL prevents unsafe concurrent access, which describes best the reality of the hardware, but at first, it felt restrictive. I learned (a lot) by failing (a lot). I eventually found a path, inspired by projects like the [Chaos Communication Camp badge, the Flow3r](https://git.flow3r.garden/flow3r/flow3-rs). At first, I tangled with `Mutex<RefCell>` and `critical_section`, but, for my goals, that was overkill or simply wrong.

Is this code pretty? Nope! But perfect code would take months, and what matters now isn’t the destination, but _the journey_.

<video width="100%" controls>
    <source src='/verdigris/assets/vids/proto_zero_1.mp4'>
</video>

Doing all this work raises a few questions:

### Software

- Which pattern should we use? Maybe we sketch some UML or other visual drafts to map out code structure?
- Software debounce, or [Schmitt triggers](https://en.wikipedia.org/wiki/Schmitt_trigger)?
- Should we go all [Embassy](https://embassy.dev/)? Or is anyone curious about [Ariel OS](https://github.com/ariel-os/ariel-os) or something else?
- [PCM5100A](https://github.com/berlinrs/verdigris-music-player/blob/main/doc/datasheets/PCM5100A.pdf) and [PAM8901](https://github.com/berlinrs/verdigris-music-player/blob/main/doc/datasheets/PAM8901_8.pdf) chips need drivers, and I can’t find any. They’re I2C devices, so we could probably whip up new [embedded_hal](https://docs.rs/embedded-hal/latest/embedded_hal/)-compatible crates. The PCM turns audio data into analog sound (binary to wave signal); the PAM amplifies it for headphones. Anyone with driver-writing experience (or ready to learn) want to join in?

### Hardware

Fresh from writing a very little bit of this firmware, it _really feels_ like anything’s possible. So let’s dream. Can we make something that earns a place in our daily lives?

- The battery management could be better depending on what hardware we decide on.
- What features do we want? Line in/out? Hi-fi sound? Bluetooth A2DP? Touchscreen, high-res display, tactile buttons or wheels, alien detector — or just go wild and add it all?!
- What’s the ultimate design goal? That’s tightly tied to the features we want.

And a few group/organizational bits:
- Should we plan a group purchase? Who’s interested in building something (even if it’s simple) to tinker along?
- Do we want regular meet-ups?

Looking forward to our next meeting - tomorrow, the 8th of September. If you want to join, hop into the [Signal group](/verdigris/join) to get details about our (not public) location.

Last but not least: all this makes me think of the [Cult of Done Manifesto](https://medium.com/@bre/the-cult-of-done-manifesto-724ca1c2ff13) by Bre Pettis. Worth a read!
