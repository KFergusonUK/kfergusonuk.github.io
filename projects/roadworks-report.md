---
layout: page
title: Automated roadworks reporting
permalink: /projects/roadworks-report/
description: "An API-driven text report of high-impact roadworks for Durham County Council, replacing a manual weekly process. Since replicated by 20+ authorities."
---

*Durham County Council · with Causeway one.network · since replicated by
20+ highway authorities*

An API-driven text report of high-impact roadworks on the council website,
replacing a manual weekly compilation.

## The problem

Durham had always published a text-based report of major works. The
coordination team compiled it by hand — adding high-impact works to a
spreadsheet each week, with checks by several teams before it reached the
webpage. Accurate, but slow, and consuming staff time every week for
something that could be derived from data the council already held.

Map-based works data was already embedded on the site through one.network.
But a text list does something a map doesn't: it's accessible to screen
readers, it's scannable, and it works for anyone who wants to read rather
than navigate. Keeping it mattered.

## The constraint that shaped it

The obvious approach — pull everything from an existing API — fails on two
counts.

First, the report exists to highlight *disruptive* works: road closures,
signals. A feed of all works buries the ones people need to know about.

Second, GDPR. The Street Manager Works Description field is free text, which
means it can contain anything a user typed, including personal data. You
can't publish that to a council website unchecked.

## How it works

Working with one.network, we built an API that returns works in text format,
drawing only on the Traffic Management module — which scopes it to the
high-impact works the report is for.

For the descriptions, we compiled a controlled list of works descriptions
for staff to use, added by the coordination and TTRO teams as a matter of
course. That sidesteps the free text field entirely: the public gets useful
context about each works, and nothing unreviewed reaches the website.

[View the live report](https://www.durham.gov.uk/roadworks)

## Outcome

Up-to-date text-based roadworks information, published continuously rather
than weekly, with the manual compilation removed entirely. The approach has
since been replicated by more than 20 highway authorities.

<!-- Confirm the current adoption figure — you thought it may now be
     30+ authorities rather than 20+. Update this page, the projects list
     and the home page bullet together when you know.

     Also worth adding if you have it: roughly how much staff time this
     freed up. -->
