---
layout: page
title: Street Works Sentinel Suite
permalink: /projects/sentinel-suite/
---

*Free desktop tooling built on Street Manager data · current version 1.0.2*

Four applications that help highway authorities and utilities get more out
of their Street Manager data, bundled behind a single launcher. Free to use,
and built over evenings and weekends.

## Demo

<div class="video">
  <iframe src="https://www.youtube-nocookie.com/embed/jrM3Wze6sow"
          title="Street Works Sentinel Suite — short demo"
          frameborder="0" allowfullscreen></iframe>
</div>

## The four tools

**FPN Sentinel** — identifies potential Fixed Penalty Notices using built-in
logic against your own Street Manager data, with works openable directly in
Street Manager from the results.

**TPI Sentinel** — tracks, analyses and visualises performance indicators.

**Management Sentinel** — surfaces wider trends, patterns and performance
across road and street works.

**Notice Sentinel** — handles Section 50 and Road Opening Notices, including
bond return details.

All four sit behind a central launcher that also handles updates, so there's
no manual re-downloading when a new version lands.

<!-- Worth a paragraph: what was the manual process before these existed?
     Working through a Download All My Data export by hand, presumably —
     roughly how long did that take, and how easy was it to miss things?
     The value of an automation tool is entirely in what it replaced. -->

## Designed to be tried, not deployed

The friction with tooling like this is usually the setup: nobody wants to
put real operational data into an unfamiliar application to find out whether
it's useful. So the download includes fictional street works test data —
with a few deliberately silly names hidden in it — that works with both FPN
and Management Sentinel. You can see exactly what the tools do before
deciding whether to point them at anything real.

Setup is: download the ZIP, extract it, add your Street Manager data (the
DAMD file doesn't need unzipping). The optional S70 distance check needs
GeoPlace API credentials; everything else doesn't.

## Keeping pace with national changes

The tools track the DfT's Street Manager Download All My Data layout, which
changes periodically — version 1.0.2 brought them back in line with the
latest revision. That's the unglamorous part of maintaining something other
organisations rely on: when the national format shifts, every user's
workflow breaks unless someone keeps up with it.

## Get it

- [Download from GitHub](https://github.com/KFergusonUK/Sentinel_Suite)
- [Guide and FAQs](https://github.com/KFergusonUK/Sentinel_Suite/blob/main/Guide.md)

Most bar charts are clickable, and name lists can be double-clicked.

## Who's using it

<!-- The most important section here, if you have anything for it. How
     many organisations? Any that would give you a line? Even a rough
     count beats a feature list — nobody adopts a tool out of politeness,
     so adoption is the evidence. -->

---

A personal project, built in my own time and separate from my day job.
Feedback welcome.
