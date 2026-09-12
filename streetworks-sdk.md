---
layout: page
title: StreetWorks SDK
permalink: /projects/streetworks-sdk/
---

*Python · open source, MIT licensed · [PyPI](https://pypi.org/project/streetworks/) ·
[GitHub](https://github.com/KFergusonUK/StreetWorks-SDK)*

<!-- IMAGE: the world map. Save it as assets/streetworks/world-map.png
     and uncomment. This is the single most persuasive thing on the page —
     it should be the first thing a visitor sees. -->
<!-- ![World map of live roadworks pulled through the SDK, blue dots marking works sites](/assets/posts/streetworks-sdk/world-map.png) -->
*A partial live pull through the SDK, August 2026. Each blue dot is a works site.*

## The problem

Roadworks and street works data is published by hundreds of separate
authorities around the world, in inconsistent formats, on endpoints that
change. Anyone wanting a wider picture has to write and maintain bespoke
handling for each one — working out how a given country's infrastructure
works before they can start on the actual problem.

## What it does

StreetWorks SDK normalises data from over 130 official sources worldwide
into one consistent model, so the source of a record stops mattering to the
code consuming it.

```bash
pip install streetworks
```

Two systems that share no schema, no standard and no language — the United
States (WZDx) and Spain (DATEX II). Run each through its converter and the
same caller code handles both, unchanged:

```python
from streetworks.common import from_datex2, from_wzdx
from streetworks.datex2.dgt import DGTClient   # Spain (DATEX II)
from streetworks.wzdx import WZDxClient        # USA   (WZDx)

with WZDxClient() as wzdx:
    feed = wzdx.fetch("https://wzdx.wsdot.wa.gov/api/v4/WorkZoneFeed")
us_works = from_wzdx(feed.road_events, territory="USA",
                     administrative_area="Washington")

with DGTClient() as dgt:
    es_works = [from_datex2(s, territory="Spain") for s in dgt.iter_roadworks()]

# Two unrelated standards — now the same shape.
for works in [*us_works, *es_works]:
    for site in works.sites:
        print(works.territory, site.works_type, site.date_confidence)
```

Discovery works the same way. Ask what covers a territory and each result
prints its scope, whether it needs credentials, and the exact import line to
use next — so there's no hunting through docs to reach a given feed.

## Design notes

**The common model doesn't flatten the source.** Every record keeps its
untouched original on `.raw`. You reach for the same field names whichever
provider it came from, but the native structure, language and any extra
richness stay accessible underneath. Norway carries Z coordinates in its
national road-link geometry; a common model that discarded that would be
throwing away something real.

**Gazetteers are first-class, and split correctly.** An address register
answers "where is this address?"; a street register answers "what and where
is this street?", with geometry. Conflating them produced a genuine
analytical error early on — "European gazetteers have no street geometry"
looks true if every example to hand is an address register, and it's false.
The geometry lives in the street register, usually published by a different
body entirely.

**Never silently reproject, always label the coordinate system** — and the
same discipline applied to the vertical axis. Elevation is carried as a
named provider's stated value with its vertical datum labelled as carefully
as its CRS, rather than assuming two sources agree because they happen to.

<!-- IMAGE: the common model diagram.
     assets/streetworks/common-model.png -->
<!-- ![Diagram showing native provider schemas resolving to shared Works, Site and Street shapes](/assets/posts/streetworks-sdk/common-model.png) -->

Full detail in the [common model
docs](https://github.com/KFergusonUK/StreetWorks-SDK/blob/main/docs/concepts/common-model.md).

## Coverage

138 providers currently registered, 103 of which need no credentials at all.
Roadworks and streetworks feeds are the focus, with street gazetteers as the
necessary companion — plus Digital Traffic Regulation Orders, the modelled
NUAR API, and the UK Police Crime API.

<!-- IMAGE: the provider matrix.
     assets/streetworks/provider-matrix.png -->
<!-- ![Summary provider matrix showing coverage by territory](/assets/posts/streetworks-sdk/provider-matrix.png) -->

The full provider-by-provider matrix, including state-by-state and
province-by-province breakdowns for the USA, Canada, Germany and France,
is in the [repository
docs](https://github.com/KFergusonUK/StreetWorks-SDK).

## What it turned up

Building it surfaced something I didn't expect. Plenty of countries publish
a national roadworks **feed** — the road authority describing what it's
doing on its own network. Very few run a national roadworks **register**:
the statutory system where anyone wanting to dig has to apply, and an
authority grants or refuses it. Of the national systems reachable so far,
only England's Street Manager, Scotland's SRWR and Jersey's RoadWorkx
qualify. Most good permit registers are single cities working in isolation.

England has a complete, joined-up national stack — register, gazetteer and
legal traffic orders, all reachable through one interface. That turns out to
be genuinely rare.

<!-- IMAGE: worker safety / abuse risk map.
     assets/streetworks/worker-safety-map.png -->
<!-- ![Map overlaying roadworks sites with historic crime data](/assets/posts/streetworks-sdk/worker-safety-map.png) -->

## Full write-up

[I set out to connect England's streetworks systems. I ended up mapping a
third of the world's.](/2026/08/27/connecting-englands-streetworks-systems.html)
— the standards, the surprises, the Section 50 aside, road worker safety,
and what building it taught me about how fortunate England is.

## Status

Current version 0.10.0, and a genuine work in progress. NUAR is modelled but
not live, several feeds are waiting on credentials, and there are whole
countries not yet reached.

A personal project, built in my own time and separate from my day job. The
original idea came from a conversation with
[Christopher Carlon](https://github.com/CarlonChristopher); pull requests
from Christopher and Jamie Atkinson have gone in since.

If you maintain a feed that should be in here, or you've built something on
top of it, I'd like to hear about it.
