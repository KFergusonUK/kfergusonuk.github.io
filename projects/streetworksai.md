---
layout: page
title: streetworksai.co.uk
permalink: /projects/streetworksai/
description: "An AI assistant for street works guidance — a custom-trained chatbot built on a hand-assembled corpus in 2023, later rebuilt on an API."
---

*An AI assistant for street works guidance · [streetworksai.co.uk](https://www.streetworksai.co.uk)*

## Before it was easy

I started this in 2023, when ChatGPT was a few months old and wiring an API
to a prompt was becoming the obvious thing to do. I deliberately didn't do
that. I wanted to understand what was happening underneath a language model
rather than just call one, so I built the chatbot itself — Python, with
NumPy, TensorFlow and NLTK.

The harder half was the corpus. Over several months I assembled a custom
reference from the Red Book (Safety at Street Works and Road Works Code of
Practice), the Inspections Code of Practice, GeoPlace NSG data entry
conventions and best practice, Traffic Signs Manual Chapter 8, permit scheme
statutory guidance, and the questions I get asked over and over.

![The 2023 chatbot answering street works questions on the Traffic Management Act, lead-in tapers, street naming and numbering, ESUs and type 4 streets](/assets/streetworksai/early-output.jpg)

It worked. Not at ChatGPT's level, and it struggled where street and road
works guidance overlaps with subtly different meaning — which, if you work
in this field, you'll recognise as the hardest part of the domain rather
than a failure of the model.

## Why I moved to an API

I got it to roughly GPT-2 level — coherent, domain-grounded, genuinely
useful on the questions it had seen. That was the ceiling for one person
working evenings against a corpus they'd assembled by hand.

GPT-4 had landed in March 2023, while I was still building. By the time
4o-mini arrived in the summer of 2024, the gap wasn't just large, it was
widening faster than any individual could close it — and the frontier
version was cheap enough to call from an API.

So I stopped. Not because the project failed, but because continuing would
have been sentiment rather than engineering. The custom build had already
done what I wanted it to: taught me what actually happens inside these
systems rather than how to call one.

The current version works the same way in principle, but through an agent
calling OpenAI's API, grounded against the same freely available source
material — the Red Book and the rest.

## Why it's currently offline

It's disabled, deliberately.

The model doesn't read tables reliably, and in this domain a lot of the
guidance that matters most lives in tables — including safety distances.
Some answers were wrong. For most subjects a wrong answer is an annoyance;
for the distance between a works and live traffic it isn't, so the right
call was to take it down until the table handling is fixed rather than leave
something plausible-sounding running.

It isn't a failure specific to my build. The same weakness shows up in the
equivalent tools at DfT and GeoPlace/HAUC, and I've raised it with both.
Models are fluent enough that a wrong number arrives with exactly the same
confidence as a right one, which is precisely why safety-critical guidance
is the worst place to find out.

<!-- Worth a line on what the fix looks like — structured extraction of the
     tables rather than asking the model to read them, presumably. Saying
     how you'd solve it is more interesting than saying it's broken. -->

## The code

The 2023 chatbot, with a smaller sample corpus in place of the full
highways one, is on
[GitHub](https://github.com/KFergusonUK/ChatBot).
