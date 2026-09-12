---
layout: page
title: Shadowband
permalink: /projects/shadowband/
---

*Android · Kotlin · built, shipped and maintained solo*

**Over 7,000 installs · 4.2 average rating**

<!-- IMAGE: app icon or a hero screenshot.
     assets/shadowband/hero.png -->
![Shadowband app running on a phone](/assets/posts/shadowband/hero.png)

**[Shadowband on Google Play](https://play.google.com/store/apps/details?id=com.ferguson.shadowband)**

## Why I built it

The paranormal has always been something that's interested me, and I've when I've tried apps and ITC devices in the past, I'm never fully sure exactly what's happening under the hood.  Why is it saying X?, What is happening to trigger the sound?, Most importantly, Am I being tricked here?
I wanted to know what was happening underneath.  In building Shadowband, I understand this a little more, but, honestly, I'm still not sure exactly what's happening beyond the app.  The app uses environmental readings, such as temperature, EMF, sound, proximity, vibration, even camera in some modes. I know now what the app is doing, but I'm honestly still not sure some times what the environment is :D
I get that electronics can impact EMF and cause fault positives, which I tried to rule out in the app, but as fo rother fluxuations, what exactly causes them? the weather? air moving around a building? Earth tremors? Ghost?  You're guess is as good as mine :)

I wanted to create an app that was free and fun for people to use, not something full of adds, or where you're having to pay for each module.  So, I created Shadowband :)


## What it does

The app has various modes:
The classic ITC (Instrumental Trans Communication) or Spirit Box - Shadowband. 
Camera enabled visual figure mapping, using AI assist (Mediapipe/ML_Kit) - Apparition Scanner. 
A REM Pod and trigger object/image movement sensor combined - Trigger REM Pod.  
I plotting grid for highlighting areas of interest - Energy Map
Somewhere to view all your logs and captures - Evidence Locker 
![Main screen of Shadowband showing sensor readings](/assets/posts/shadowband/screenshot-main.jpg)

## The interesting technical problem

The latest addition to the app is low frequency audio detection, which is more difficult than it sounds on
consumer hardware. Phone microphones aren't designed for it, so getting
anything trustworthy out of one means:

- **Heterodyning** to shift infrasound up into the audible range
- **50Hz notch filtering** to remove mains hum, which otherwise dominates
  everything at the low end inside any building
- **YAMNet TFLite classification**, gated behind an adaptive spectral
  baseline so it responds to genuine changes rather than firing constantly
  on ambient noise

![Spectral display showing low-frequency audio analysis](/assets/posts/shadowband/infrasound.png)

Other recent work: a camera crash fix, a BroadcastReceiver leak fix, a
Fahrenheit/Celsius toggle, and a High Gain mode.

## Interest and coverage

- **Newton News** — Covered the app in the October 2025 edition.
- **Creepy Blinders** Used by a local paranormal Group on investigations.

![Press Info](/assets/posts/shadowband/press1wide.png)


