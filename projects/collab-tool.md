---
layout: page
title: Streets Collaboration Tool
permalink: /projects/collab-tool/
---

*GeoPlace · Technical Project Lead, since 2026*

<!-- BEFORE PUBLISHING: this is GeoPlace work, currently a prototype.
     Get sign-off on what can be said publicly — particularly anything
     about status, timelines or funding. What's below is deliberately
     high-level and drawn from what you've already said in public. -->

A tool to help highway authorities and utilities cost permits, model the
savings available from collaborating, and coordinate planned works with each
other.

## The problem

HAUC(UK) turned 40 in 2026 — four decades of highway authorities and
utilities learning, sometimes painfully, how to work together. The
coordination challenge hasn't gone away. It's changed shape.

The data exists now. What still lags is the tooling that helps organisations
see what the benefits of collaborating would actually be, and who their
potential collaborators might be.

## Presented at the HAUC Convention 2026

<img src="/assets/hauc/hauc-convention-2026.png"
     alt="Presenting the Road and Street Works Collaboration Tool at the HAUC(UK) Convention 2026 in Manchester"
     class="talk-photo">

Christopher Carlon and I presented the tool in the Innovating Data session
at the [HAUC(UK) Convention
2026](https://www.geoplace.co.uk/news-events/events/hauc-convention/hauc-convention-delegate-information/past-conventions/hauc-convention-2026/hauc-convention-agenda-2026)
in Manchester, on 23 April 2026 — the convention marking HAUC(UK)'s 40th
year.

## How it's built

A containerised application: Python and FastAPI on the backend, integrated
with Street Manager and the street gazetteer, with a Next.js and React
frontend and Leaflet for mapping. It uses the
[StreetWorks SDK](/projects/streetworks-sdk/) — a personal open-source
project that turned out to be the right tool for the job — for its data
handling.

## Origin

The idea came out of a sprint win at the NWG Innovation Festival with
Jonathan Bates and Christopher Carlon, and has grown from there into a
working prototype at GeoPlace.

## Status

<!-- Say only what you've been cleared to say. -->
