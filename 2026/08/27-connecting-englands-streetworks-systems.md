---
layout: post
title: "I set out to connect England's streetworks systems. I ended up mapping a third of the world's."
date: 2026-08-27
excerpt: "The StreetWorks SDK now pulls from over 130 official sources worldwide. What I found along the way about how differently countries handle roadworks data — and how fortunate England is."
---

<!-- IMAGES: eight images from the original article need adding.
     Save them into assets/posts/streetworks-sdk/ and uncomment the
     matching line below each caption. Suggested filenames are given.
     Every image also needs alt text for screen readers — replace the
     bracketed text in each. -->

<!-- ![World map of live roadworks pulled through the SDK, blue dots marking works sites](/assets/posts/streetworks-sdk/world-map.png) -->
*StreetWorks SDK — roadworks world map, partial live pull, 26 August 2026. Blue dots are works sites.*

> "Make a Streetworks (and roadworks) Software Development Kit (SDK) that makes connecting to England's systems — Street Manager, NSG, D-TRO, National Highways, etc. — easier, and maybe even pulls in data from across the UK... Europe... or even the world, puts it through a common model, and makes it consistent to use. Just link to each country's version of Street Manager. How hard can it be…"

After several months of work, multiple thousand burnt tokens, and many headaches and dead ends later, the StreetWorks SDK is now looking in pretty good shape.

At the start of this project, little did I realise just how lucky England is with our national streetworks infrastructure platforms. The story is not repeated in many countries. Nor did I realise how interesting the data comparisons would be.

My mental model was basically: link all the English systems I already use into one easy install, then find the equivalent of Street Manager in each country, connect to it, normalise the data, job done. It turns out there often isn't an equivalent of Street Manager.

Sometimes there isn't a national system at all. Sometimes there is, but only for motorways. Sometimes every state, province, region or municipality does something different. Sometimes the data is there, but you need a gazetteer to make sense of the location. Sometimes there's an API, but you need credentials, or you have to register first. Sometimes there's an open feed, but it follows a standard that's implemented completely differently from every other feed using that same standard.

And sometimes there just isn't a feed.

The SDK is now pulling in data from over 130 official sources worldwide, each with its own shape, authentication method, pagination and schema. All of that now comes together in StreetWorks-SDK, where the data can be parsed through a common model.

## So what is StreetWorks SDK, and what does it do?

Put simply, the SDK takes all the hard bits about connecting to various data sources — England and elsewhere — and makes them much, much simpler to use. Where is it, how does it want to talk to my computer, what do I need to do, how does it handle passwords.

Now, instead of spending the first valuable hour of a project working out how to handle tokens from DfT, handshakes in Swedish, or just what 시작일 means in Korean, you specify where you want to connect to, add your username and password to the `.env` file if needed, use the common model, and you're in. Data ready to go, with whatever project you had in mind.

That can be as a solo developer, a development team, or an AI agent.

And that last one is becoming increasingly important. If an AI agent is asked to build something that uses roadworks data, the difficult part shouldn't be discovering how a particular country's infrastructure works before it can even start solving the actual problem — which is exactly the sort of thing the discovery layer, common model, and the agent-oriented files now in the repo are meant to take off its plate.

## Quick start

The whole point is that this is short. Install it, ask what covers the place you care about, then connect and read the data back through the common model.

First, discovery — which providers exist, and which need no credentials:

```bash
pip install streetworks
```

```python
import streetworks

# Find a feed — browse by territory, or list the credential-free ones.
len(streetworks.providers())                  # every provider
len(streetworks.providers(credentials=False)) # just the keyless ones
streetworks.providers(territory="Spain")      # what covers Spain?
```

Run against the live registry today, that returns 138 providers currently registered, 103 of which need no credentials at all. Asking for Spain lists the five real Spanish feeds: DGT (Dirección General de Tráfico) nationally, Consell de Mallorca (IDEmallorca) for the island, Servei Català de Trànsit for Catalonia, Dirección de Tráfico del Gobierno Vasco for the Basque Country, and Ayuntamiento de Madrid (INFORMO) for the capital.

Each entry prints its own summary — what it is, its network scope, whether it needs credentials, and, on the last line, the exact import to use. That final line is the bridge to the next step:

```text
DGT (Dirección General de Tráfico)
Spain's national roadworks feed.
Network scope: multi authority interurban
Scope: National except Catalonia and the Basque Country...
Credentials: No credentials required
from streetworks.datex2.dgt import DGTClient   <-- the import you need next
```

So the browse step hands you the import line directly — no hunting through docs for how to reach a given feed.

Then pick a feed and pull it. Most need no credentials at all; where one does, it goes in a `.env` file (a template is provided, just rename it) and the rest stays the same:

```python
from streetworks import get_provider
from streetworks.common import from_datex2

# Connect — get_provider() hands back the client class for a key.
DGTClient = get_provider("dgt")               # Spain's national feed
with DGTClient() as dgt:                      # no credentials needed
    situations = list(dgt.iter_roadworks())

# Common model — run the native records through the from_* converter.
for situation in situations:
    works = from_datex2(situation, territory="Spain")
    for site in works.sites:
        print(works.territory, site.works_type, site.date_confidence)
```

Each `Works` carries its sites, every field in your own terms — `works_type`, `date_confidence`, `territory` — with the untouched source record still there on `.raw`. And where a territory has more than one provider (Spain has five, Norway has three), asking for the place rather than a specific feed lists the candidates instead of guessing which one you meant.

## The common model

<!-- ![Diagram of the common model, showing native provider schemas resolving to shared Works, Site and Street shapes](/assets/posts/streetworks-sdk/common-model.png) -->

More on the common model is in the [repository docs](https://github.com/KFergusonUK/StreetWorks-SDK/blob/main/docs/concepts/common-model.md).

What this means is that you don't need to know the Italian for end date, or the German for street. Built over the top of every feed is a simple, always-works-the-same reference point, so you don't need to know how DATEX II, WZDx or Street Manager work, or what the main data fields like "works start date" mean in each. However you interact, it's always the same — or you can go to the `.raw` feed and get the data as it comes.

The inclusion of `.raw` is, I feel, important. Working through the data, I enjoyed seeing the different reference names and data nuances, such as Norway having Z coordinates in its feeds. By including the raw feeds, the native, culturally important data and labelling are not lost. They remain underneath, always accessible, some providing really rich additional data that would be difficult to accommodate in a common model.

The interface is unified, but the meaning — and the Z coordinate — is not flattened.

## What's included

The main focus is, of course, streetworks and roadworks feeds, which can be surprisingly different. Next, and generally a much-needed companion, are the street gazetteers, or their equivalents. Also covered is the Digital Traffic Regulation Order platform, the initial layout for the coming National Underground Asset Register (NUAR) API, and the UK Police Crime API — more on that later.

## The other half: street gazetteers

Roadworks are only half the story. A lot of roadworks data can't even be plotted on its own — the feed tells you a works is on a given street, but not where that street physically is. For that you need a gazetteer: the authoritative register of what a street or address actually is, and where.

So the SDK treats gazetteers as a first-class part of the package, not an afterthought — and it keeps two things apart that are genuinely different. An address register (France's BAN, the Netherlands' BAG, Norway's Kartverket) answers "where is this address?". A street register (Great Britain's OS Open USRN, the Netherlands' NWB, Norway's NVDB) answers "what and where is this street?", with real geometry.

Lumping them together produced a real analytical error early on. "European gazetteers have no street geometry" looked true when the only examples to hand were address registers, and it's simply false. The geometry lives in the street register, which in most countries is published separately, by a different body, from the addresses.

And just like the roadworks side, the gazetteer side has its own common model — BAN, BAG, NVDB, OS Open USRN and the rest all resolve to the same Street, Segment and Address shapes, so you reach for the same fields whichever country's register you're reading. I won't repeat the whole demonstration here; the GitHub docs walk through the gazetteer converters in full.

## What real geometry lets you do

Once a street register gives you real geometry, you can do more than join it to roadworks. One of the worked examples in the repository takes Great Britain's OS Open USRN street centrelines, densifies each one, and drapes it over a real elevation model — here, the Environment Agency's LIDAR Composite DTM — to produce a 3D surface of the streets following the actual terrain. This is the Durham peninsula:

<!-- ![3D mesh of Durham peninsula street centrelines draped over a LIDAR terrain model](/assets/posts/streetworks-sdk/durham-peninsula-3d.png) -->
*OS Open USRN street centrelines draped over the Environment Agency's LIDAR Composite DTM (1m) of the Durham peninsula — a worked example from the repository, not a feature of the installed package. The mesh exports to STL, so you can 3D print it.*

A couple of things about this matter more than the picture. First, it's built with no heavy geospatial stack — no GDAL, no rasterio — just a hand-rolled raster decoder over the same httpx-and-nothing-else approach as the rest of the SDK.

Second, and more importantly, the draped height is treated honestly as what it is: a sampled estimate of the ground beneath a flat street centreline, never written back into the data as though it were a measured elevation of the road. Where the terrain model has a genuine gap, the point is dropped, not interpolated across — filling it would fabricate a plateau that isn't really there.

That's the same discipline the rest of the SDK applies horizontally — never silently reproject, always label the coordinate system — now applied to the vertical axis. Elevation is carried as a named provider's stated value, with its vertical datum labelled as carefully as its CRS. The Environment Agency's heights here are Ordnance Datum Newlyn; Norway's NVDB uses a different datum again. Both are real, and the SDK never quietly assumes they're the same — even where, for two British sources, they happen to agree.

## Provider matrix

<!-- ![Summary provider matrix showing coverage by territory across roadworks, gazetteer, TTRO, NUAR and other columns](/assets/posts/streetworks-sdk/provider-matrix.png) -->
*The TTRO, NUAR and Other columns are effectively UK-only today (D-TRO, NUAR model, Police / DataVIA), hence the gaps elsewhere.*

The full provider-by-provider matrix — including the state-by-state and province-by-province breakdown for the USA, Canada, Germany and France, and the specific data source behind every cell — lives in the [repository docs](https://github.com/KFergusonUK/StreetWorks-SDK).

## Data landscapes

Working through each zone, it struck me just how differently each area handles roadworks data. Some systems are very mature and clearly working well; others, perhaps less so.

<!-- ![Comparison of data landscape maturity across regions](/assets/posts/streetworks-sdk/data-landscapes.png) -->

### USA and Canada

I've grouped these as they share, in different proportions, two main systems: the more modern WZDx (Work Zone Data Exchange) and 511, based on an older system where North Americans would dial 511 for traffic and travel information, which has now largely gone online while keeping the name.

Both are very useful. The USA seems to be working towards WZDx, and Canada is largely on 511; some states are the other way round, some have both. Both are relatively easy to read once you're set up, meaning that if you've worked out one feed, you've worked out most of the pattern. That doesn't necessarily mean there is a feed, however — some states are missing from both entirely. Overall, good work for North America there.

### Europe

DATEX II dominates the national roads, including even the UK's National Highways feed. Beyond the main roads, though, the picture is not so rosy. Some countries have a larger national feed, which is great to read in, but many are by state or province, and that doesn't mean consistency between them. In some cases each area within one country can have its own unique schema and setup, some credential-protected, some not, some not available at all. The NAP system seems to be bringing some of that together. Overall, a tricky picture, but so much fun to look into, with a lot of data-rich feeds.

### UK

On to Blighty. England, as I mentioned earlier, really is fortunate with the national systems in place — Street Manager, NSG, D-TRO. The data is mostly clean and detail-rich. For all the complaining we like to do, these really are great systems that we're lucky to have.

England is really the one place the SDK reaches a complete, joined-up national stack. The roadworks register (Street Manager), the street gazetteer (the National Street Gazetteer, plus OS Open USRN for open geometry) and the legal traffic orders (D-TRO) are all there, and all reachable through one interface. Most of that is access-controlled — you bring your own account for Street Manager, the NSG and D-TRO — but two routes need no account at all: OS Open USRN for street geometry, and Street Manager Open Data, the open subscribe-and-receive push feed of the register's own works events. So you can get real English roadworks and street data flowing with zero credentials, then add accounts to unlock the fuller picture.

And here's the thing that really makes England fortunate, which only became clear once I'd looked at everywhere else. Plenty of countries publish a national roadworks **feed** — the road authority telling you what it's doing on its own network. Far fewer run a national roadworks **register**: the statutory system where anyone wanting to dig has to apply, and an authority grants or refuses it.

That distinction is the difference between being told about works and being the gatekeeper of them, and it is surprisingly rare. Of the national and territory-wide systems I've found so far, only a handful of national roadworks registers appear to exist in a usable, reachable format at all: England's Street Manager, Scotland's SRWR, and — punching well above its size — Jersey's RoadWorkx. Ireland is a near-miss: MapRoad is a genuine national statutory register on paper, but I couldn't find a published endpoint, schema or way in, so as far as actually consuming the data goes, it isn't reachable. That gap between "a register exists" and "you can actually get the data" is itself part of the story.

Plenty of strong candidates don't make that list, which is what surprised me. Iceland, New Zealand and Spain all have genuinely good national roadworks feeds — but they're the road authority's own operational feed, not a permit register. Most of the other really good permit registers I found are single cities doing it well in isolation: New York, Paris, Copenhagen, Helsinki. Not whole countries. A proper national register is the exception, not the rule.

The UK as a whole is more fractured. Scotland publishes a brilliant SRWR feed, but it must be linked to the NSG for plotting, as it doesn't carry coordinates. Wales and Northern Ireland are limited, in the openly accessible feeds I found, to traffic feeds on main roads — a real miss on the data here. I would love to see some Welsh and Northern Irish API feeds in future.

### Crown Dependencies

Jersey, Guernsey, Isle of Man. Jersey is the standout star here with a great roadworks feed. Guernsey and the Isle of Man have some catching up to do on the roadworks side, though Guernsey does have a good street gazetteer.

### New Zealand and Australia

New Zealand has a single country-wide feed, which is brilliant to see. Australia is split by region with no national register, and as with Europe there are multiple approaches to how the data is fed out — some credential-protected, some not. It's a mixed bag, but most of the data is pretty good.

### Nordics

I know we already covered Europe, but I did enjoy looking at the Nordic data. Norway stood out as one of the few sources I found carrying Z coordinates in its national road-link geometry, and I think that's great. It tells a real story of the region's layout and the importance of the topography.

## A Section 50 aside

<!-- ![Screenshot of an example Section 50 application page built against the Street Manager API](/assets/posts/streetworks-sdk/section-50.png) -->
*The top half of an example Section 50 application page, for use with the Street Manager API.*

I included this as a "why not" to provoke a little discussion, because the current Section 50 process is, in my opinion, somewhat outdated. Street Manager handles Section 50s, so why not use it as a Section 50 management platform? The free-text fields aren't exposed in the open data publication, so why not use them to store webpage-calculated bonds and contacts?

This isn't a recommendation, to be clear — just an observation that we have a pretty good national system that could be used to handle S50 management. The full example application page, along with the HTML and Python, is in the repository examples.

## Worker safety and road worker abuse

<!-- ![Map showing roadworks sites overlaid with historic crime data to indicate abuse risk](/assets/posts/streetworks-sdk/worker-safety-map.png) -->
*Road worker safety and abuse risk map example, from the [repository examples](https://github.com/KFergusonUK/StreetWorks-SDK/blob/main/docs/examples.md).*

Some might be slightly confused as to why I've included the UK Police Crime API in a streetworks SDK, but I see the two as often sadly interlinked. Road crews are often subject to verbal and sometimes physical assault. If we're able to use the data to help determine whether a worksite sits in an area where additional safety measures might be appropriate, based on historic crime levels, then while it won't guarantee nothing happens, perhaps measures could be put in place to reduce the likelihood of an incident — visible camera presence, a change of working hours.

Roadworks can be frustrating for us all, and I'm sure we've all seen someone "just standing around" "making us late" on a worksite. There are often good reasons for this, and after the summer we just had, I'm surprised more aren't sat eating ice creams.

## Busy is relative

I included a slightly silly comparison: Durham City and Paris. For those who don't know, Durham is a small cathedral city in northern England with a population of around 50,000, covering about 15 km². Paris is a European capital with a population of around two million, covering about 105 km².

At the time there were 18 active streetworks in Durham City — quite a busy day, I thought. Then I loaded up Paris, with over 1,800 active roadworks. I went back to check the Paris feed was working properly and that this wasn't every API feed Paris had. No: active works only. I suspect Paris hardly felt the difference either.

"Busy" really is relative.

<!-- ![Side-by-side comparison of active works in Durham City and Paris](/assets/posts/streetworks-sdk/durham-vs-paris.png) -->

## Code examples

Two systems that share no schema, no standard, and no language — the United States (WZDx) and Spain (DATEX II). Run each through its converter and the same caller code handles both, unchanged:

```python
from streetworks.common import from_datex2, from_wzdx
from streetworks.datex2.dgt import DGTClient   # Spain  (DATEX II)
from streetworks.wzdx import WZDxClient        # USA    (WZDx)

# United States: Washington State DOT (WZDx)
with WZDxClient() as wzdx:
    feed = wzdx.fetch("https://wzdx.wsdot.wa.gov/api/v4/WorkZoneFeed")
us_works = from_wzdx(feed.road_events, territory="USA",
                     administrative_area="Washington")

# Spain: DGT national roadworks (DATEX II)
with DGTClient() as dgt:
    es_works = [from_datex2(s, territory="Spain") for s in dgt.iter_roadworks()]

# Two unrelated standards — now the same shape. Identical code handles both.
for works in [*us_works, *es_works]:
    for site in works.sites:
        print(works.territory, site.works_type, site.date_confidence)
```

And the point I care about most — the common model sits over the top, but the native data is never flattened away:

```python
works = es_works[0]
site = works.sites[0]

print(site.works_type)       # common model — one name for every provider
print(site.date_confidence)  # ...and honest about how sure the source is

# Nothing is thrown away. .raw points back at the exact source record(s),
# in their own structure and language, always still there.
print(works.raw)             # the untouched payload, as Spain sent it
```

## What could you build with it?

The SDK isn't really the end product. It's the plumbing underneath whatever comes next. A few obvious possibilities:

- **Roadworks-aware routing** — route around active works, closures or restrictions
- **Network monitoring** — bring works from multiple authorities into one view
- **Cross-border applications** — build something once and use the same model across different countries
- **Street works analytics** — compare works, durations, impacts and patterns across authorities
- **Site safety tools** — combine works locations with other datasets, such as historic crime data, to inform site-specific safety measures
- **Digital twins and 3D applications** — use street geometry and elevation data to model the physical environment
- **Asset management** — combine works and gazetteer information with other infrastructure datasets
- **AI agents** — give an agent a consistent way to discover and consume roadworks data without having to understand every provider's API first
- **Automated decision-making** — feed standardised works data into systems that need to react to changes in the road network
- Anything else someone hasn't thought of yet

<!-- ![Autonomous vehicle rerouting example generated from SDK data](/assets/posts/streetworks-sdk/av-reroute.png) -->
*An AV rerouting example from the [repository](https://github.com/KFergusonUK/StreetWorks-SDK/blob/main/docs/examples.md) — not perfect, but a nice visual of what's possible.*

And that's really the point. I don't want to decide what StreetWorks-SDK is for. I want to make the data easy enough to use that other people can decide.

## Where this goes next

The SDK is open source, MIT licensed, and on PyPI — `pip install streetworks` — with the code at [github.com/KFergusonUK/StreetWorks-SDK](https://github.com/KFergusonUK/StreetWorks-SDK).

It's a genuine work in progress. NUAR is modelled but not yet live, as it needs formal access I don't have in a personal capacity. Several feeds are sat waiting on credentials, and there are whole countries I haven't reached yet. The provider matrix will keep moving.

I should be clear that this is a personal project, built in my own time and separate from my day job. Any views here are my own, and nothing in it speaks for my employers.

The original idea came from a conversation with Christopher Carlon, likely over WhatsApp, where we considered how nice it would be to have a ready-made SDK for streetworks in the UK, and I sort of got carried away. What started as "wouldn't it be nice if someone made this?" became "hang on, what does Sweden do?" Then: what about Spain? Then: does Australia have anything? Then: do all US states do this the same?

And somewhere along the way, the project became global. I'm still investigating.

If you work with roadworks, street, or address gazetteer data — or you maintain a feed that should be in here — I'd genuinely like to hear from you. Tell me what's missing, tell me what I've got wrong, clone the repo, add something and submit a pull request, or just tell me your country does something interesting with Z coordinates. And if you build something on top of it, even better.

That was always the point: handle the boring plumbing, so there's time left over for the interesting ideas.

Thank you to the people who have submitted pull requests to the project so far — Christopher Carlon and Jamie Atkinson. Your assistance is very much appreciated.
