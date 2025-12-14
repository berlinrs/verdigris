---
layout: post
title:  Workshop stories
date:   2025-12-14 23:00:00 +0200
author: theo
categories:
  - misc
  - hakkaatachi
pin: true
image:
  path: assets/img/workshop_stories_cover.webp
---

![Hakkaatashi soldered by a girl with steady hands](assets/img/hakkaa_kid.webp){: w="400" .right}
![Hakkaatashi soldered by adults](assets/img/hakkaa_adults.webp){: w="400" .right}

During the last 2 months, we met for a few sessions, always involving some Embedded and Rust. Since the last post, we have met three times, developed ideas and built things, and did both with code. One of the things is the [Hakkaa Board](https://github.com/hakkaatachi/hakkaa-firmware), courtesy of Ulf and Christian. It's a small board designed to introduce people to SMD soldering and Rust. It contains a tilt sensor, a few LEDs and an ESP32-S3 RISC-V single-core processor.

We all had fun soldering and programming the microprocessor — it's a very accessible sort of game. The board is fun and the PCB silkscreen design is amazing! Soldering the LEDs upside down presents its own small challenges, as they must shine through the FR4. I was pleased to see participants of all ages spending hours concentrating on tasks that bring us into a peaceful world. It's one of those magical places where we can escape to while soldering, painting, knitting, reading, building, etc. — away from social networks and in a parallel reality.

Shorter days and cold weather obviously help.

![Prototype Zero Cousin](assets/img/proto_zero_leonie_poti.webp){: w="400" .right}

Meanwhile, Leonie build a second Prototype Zero of the music player and had some very elegant HID ideas and cool construction solutions to hold the potentiometer!

<hr style="clear: both;">

Then Ulf sent me another fun PCB. It's a board targeting demoscene exercises. It has a screen, sound and buttons. It could be a portable console, but I think his initial idea focused more on sound and colour than video games. In fact, the gamepad can be detached, leaving a very sharp, bright and colourful display and a small speaker that works perfectly for playing synthesiser sounds.

He sent me everything: The PCB and all the parts. But not the code! Because there _was_ none available. I suppose that was the real gift, though: giving me the opportunity to put my knowledge to the test after spending about a year learning Rust on my free time.

Until then, my only experience had been coding Rust on the relatively well-documented Raspberry Pi Pico platform for the music player. I did expect to find some similarities with the ESP32 platform.

More precisely, I was curious to see how different it was and how well it worked. Espressif is probably one of the biggest players now when it comes to the DIY and maker communities. So I expected to find a mature framework with support for all peripherals, such as unusual buses and DMA.

It's not what happend. I've been surprised to find that a lot of the peripheral support was unstable when using the full rust stack. Then I struggled to find the right documentation. The API is there, but examples are scarce. It all started with adjusting the cargo file so that all versions worked together, enabling the right platforms and adding the unstable flag to some libraries. On this platform, it felt like the Rust support was still in its infancy.

This encouraged me to read the source code more closely and think about the code in front of my eyes instead of challenging search engines or glorified chatbots. I guess this is where the Rust toolkit and text editor shine best, then it's up to the brain to do the rest.

Another thing I missed a bit was patterns and examples. There are the usual programming patterns, of course, but there are also many details and idioms related to the language and embedded systems. These were hard to find because, I suppose, there is still little data compared to other languages. Even Embassy provides examples for using peripherals that are limited to an unrealistically small context compared to a whole project.

Anyway, [the most I have been able to do so far is update the screen with double buffering](https://gitlab.com/theoreichel/demo-board). This setup uses almost all the available memory, leaving only a small amount of SRAM for the sound system or the user program.
This doesn't seem like much, but the entire structure is in place, as is most of the layer required to play sound through I2S. Regarding the button, I'm not looking forward to working on the charlieplexing – it would have saved me the hassle of coding some polling logic if it had just five buttons instead of six (because Embassy <3 interrupts, and for a good reason).

In theory, the two libraries ([Embedded Graphics](https://docs.rs/embedded-graphics/latest/embedded_graphics/), [FrameBuf](https://docs.rs/embedded-graphics-framebuf/latest/embedded_graphics_framebuf/) and the ESP32 HAL driver) provide a way of transferring the framebuffer to the screen via SPI and DMA. In practice, however, I was unable to find the correct code to pass the buffer address and enable a fast DMA process. Currently, some computation is required to transfer data chunks to the driver, which should then use DMA for transfer.

<video width="100%" controls>
    <source src='/verdigris/assets/vids/demo-board-screen-test.mp4'>
</video>

<hr>
On another day, Marc and Eli were more interested in experimenting with hardware and physical tools than coding. I provided the parts to build an Arduino from scratch based on an old bare PCB, which nowadays falls into the 'old-timer collector' category. However, the board is [still documented](https://docs.arduino.cc/retired/boards/arduino-serial). They diligently built two boards with components that feel old-fashioned today. Gorgeous!

![Arduino Old-Timer](assets/img/arduino1.webp){: w="350" .left}
![Arduino Old-Timer 2](assets/img/arduino2.webp){: w="350" .left}
![Arduino Old-Timer 3](assets/img/arduino3.webp){: w="350" .left}
![Arduino Old-Timer 5](assets/img/arduino5.webp){: w="350" .left}

<div style="clear: both;"></div>

This reignited my old passion for Arduino. AVR are so easy to get started with, and they require few additional components to work.

![Bare minimum arduino schematic](https://content.arduino.cc/assets/arduino%5Frs232%5Fv2.png)

While they were building the boards, I prepared the [AVR](https://book.avr-rust.org/003.1-the-avr-unknown-gnu-atmega328-target.html) environment. This left me with a few minutes to prepare an example of a blinking LED on AVR. Getting started with the toolkit, Cargo and the AVR-ISP was straightforward. However, the documentation was sparse again, and [the HAL interface](https://github.com/Rahix/avr-hal?tab=readme-ov-file) differs from that of their ARM and RISC-V cousins, mostly for a good reason: it's a smaller 8-bits platform than the 32-bits ESP or Pico. Anyway, it worked like a charm.

There's something very satisfying about writing that I got Rust running on three different kinds of MCU. It reminds me of the saying: Who has a hammer sees every problem as a nail. Now I see at least the head of two nails still above the surface of my modest knowledge: ST and Nordsemi.

ST has a strong presence in the industry, and NordSemi seems to be well supported and popular in the Rust community. I should try that sooner rather than later, and if anyone wants to join me on this journey, or share some experience, please let me know in the chat!
