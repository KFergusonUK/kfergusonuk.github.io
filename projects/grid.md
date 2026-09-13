---
layout: page
title: GRID
permalink: /projects/grid/
---

*Personal experiment. FastAPI, PostGIS and Redis; React Native (Expo) for
the mobile client.*

A grimdark, location-based gang-territory game — an inspiration pot of
Pokémon GO, Necromunda/Warhammer 40k, and Cyberpunk 2077. Built mainly to
see how something like Pokémon GO actually works under the surface:
geofenced locations, proximity checks, spatial queries at scale.

## What's there

Gangs claim territory around real-world locations, with cards layered on
top. The backend is FastAPI with PostGIS for the geospatial side and Redis
alongside it; the mobile client is React Native and Expo, using
`react-native-maps` for the map itself.

![GRID map screen showing a gang's territory radius, nearby locations, and
a detail sheet for an enemy-held site](/assets/grid/grid.jpg)

## Status

Working end to end — map, territory, gangs. Whether it goes anywhere past
being a good way to learn how these games are built under the hood is
still an open question, and that's fine.
