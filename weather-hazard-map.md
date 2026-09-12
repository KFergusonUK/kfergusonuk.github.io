---
layout: page
title: Weather Hazard Map
permalink: /projects/weather-hazard-map/
---

*FastAPI · PostGIS · React · Leaflet · built on open data*

A live multi-layer map of weather and environmental hazards across the UK,
assembled entirely from open data feeds.

## Demo

<div class="video">
  <iframe src="https://www.youtube-nocookie.com/embed/c4XvABAFe5c"
          title="Weather Hazard Map — multi-layer demo"
          frameborder="0" allowfullscreen></iframe>
</div>

The demo runs through the layers as they currently stand: rainfall, river
levels and flooding, local temperature and weather station feeds, wind
direction, and user-added custom hazards.

## Why

<!-- Two or three sentences. What made you start this? The tornado angle
     is the distinctive part — the UK has one of the highest tornado rates
     per unit area in the world and no dedicated risk mapping tool, which
     is a genuinely odd gap. Worth saying that plainly here, since it's
     the thing that makes this more than another weather map. -->

## How it's built

Everything comes from open sources — ESWD event records, ERA5 reanalysis via
the Copernicus Climate Data Store, and Blitzortung lightning data, alongside
the live environmental feeds shown in the demo. The layers are independent,
so each feed can fail without taking the map down with it.

<!-- Worth expanding on whichever of these is true:
     - how the layers are composed and refreshed
     - what you do when a feed goes down or returns nonsense
     - how user-added hazards are stored and validated
     The failure-handling question is the interesting one on any system
     built from feeds you don't control. -->

<!-- IMAGE: a still or two from the map for readers who won't play video.
     assets/weather-hazard-map/layers.png -->
<!-- ![Weather Hazard Map showing rainfall and river level layers over the UK](/assets/weather-hazard-map/layers.png) -->

## Where it's going

The tornado risk layer is the intended direction and isn't built yet. The
UK has one of the highest tornado rates per unit area in the world, and no
dedicated risk mapping tool — which is the gap that started this off. The
current layers are the data foundation that would sit underneath it.

This one gets attention in bursts rather than continuously. It's here
because the gap is real and worth someone filling, not because it's
finished.
