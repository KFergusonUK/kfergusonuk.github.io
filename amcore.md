---
layout: page
title: AMCORE
permalink: /projects/amcore/
---

*Asset Management & Condition Reporting Engine — an AI-native highway asset
management system. FastAPI, React, built with Claude Code and the Anthropic
API.*

## The idea

What if you never had to remember how your asset management system worked?
No training courses, no clicking through menus, no forgetting which screen
does what. You ask, in plain English, and it happens.

In AMCORE the language model isn't a chatbot bolted onto the side of the
system. It is the control layer. "Show me all critical issues on the map."
"Create a defect on this asset." The instruction and the action are the same
thing.

I designed this after working with several highway asset management systems
and seeing the same patterns each time — where the friction is, where things
get missed, and where audit trails break down.

## Demo

<!-- VIDEO: upload the 3-minute demo to YouTube, then paste the ID into the
     embed below and uncomment it. Unlisted works fine — unlisted videos
     still embed normally, they just don't appear in search or on your
     channel. -->

<!--
<div class="video">
  <iframe src="https://www.youtube-nocookie.com/embed/VIDEO_ID_HERE"
          title="AMCORE demo — natural language control of a highway asset management system"
          frameborder="0" allowfullscreen></iframe>
</div>
-->

<!-- IMAGE: two or three stills for readers who won't play a video.
     The map view with defects, and the natural-language command in action,
     are the two that matter.
     assets/amcore/map-view.png, assets/amcore/command.png -->
<!-- ![AMCORE map view showing highway assets and flagged defects](/assets/amcore/map-view.png) -->
<!-- ![A plain-English instruction being executed against the asset register](/assets/amcore/command.png) -->

## How it's built

- **Versioned asset templates** — asset definitions change over time, and
  historic records have to stay valid against the definition they were
  created under
- **NSG/USRN anchoring with UKPMS linkage** — assets tied to the street
  register rather than floating on coordinates alone
- **Event-driven workflows**, with manual workflow editing and creation
- **A three-tier AI safety layer** — the part that matters most, and the
  reason this isn't just a chat box wired to a database
- **A full audit trail on everything**, with soft deletes throughout

<!-- The three-tier safety layer deserves several sentences of its own.
     "We put AI in it" is the least interesting possible description of a
     system where the interesting engineering is in constraining what the
     model is allowed to do. What are the three tiers, and what does each
     one stop? -->

## Recent work

AI-assisted Street Gazetteer maintenance and street works functionality,
integration with Road-Stat data from
[Christopher Carlon](https://www.linkedin.com/in/christopher-martin-carlon/),
weather integration for asset management, and continued workflow
improvements.

The Auto-Roundabout and AI Polygonisation tools for the Street Gazetteer are
the ones I'm most pleased with.

<!-- IMAGE: Auto-Roundabout or AI Polygonisation in action. A before/after
     of a roundabout being generated is a genuinely striking visual and
     needs no explanation.
     assets/amcore/polygonisation.png -->
<!-- ![Automatic roundabout geometry generation in the Street Gazetteer tools](/assets/amcore/polygonisation.png) -->

## On how it was built

AI didn't design this. I did. But it made the distance between an idea and a
working system dramatically shorter — some of that design happened at my
desk, some on my phone while walking the dog. That's what AI-assisted
development actually looks like now.

## Status

Still evolving, and progressing well.

A personal project, built in my own time and separate from my day job. Views
here are my own and nothing in it speaks for my employers.

If you work in highways or local government asset management and this
resonates, I'd be glad to hear from you.
