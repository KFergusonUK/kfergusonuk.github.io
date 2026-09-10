---
layout: page
title: Shadowband
permalink: /projects/shadowband/
---

*Android · Kotlin*

An Android app for paranormal investigation — and, less glamorously, an
excuse to do real signal processing work on a phone.

## Recent work

Camera crash fix, BroadcastReceiver leak fix, Fahrenheit/Celsius toggle,
High Gain mode, and experimental infrasound detection.

## The interesting problem

Genuine low-frequency audio detection on consumer hardware: heterodyning to
bring infrasound into the audible range, 50Hz notch filtering to kill mains
hum, and YAMNet TFLite classification gated behind an adaptive spectral
baseline so it isn't just triggering on ambient noise.

<!-- Whatever you think of the subject matter, this is a legitimately
     good technical write-up waiting to happen, and it shows range
     next to the highways work. -->
