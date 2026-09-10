---
layout: page
title: StreetWorks SDK
permalink: /projects/streetworks-sdk/
---

*Python · open source · [PyPI](https://pypi.org/project/streetworks/) ·
[GitHub](https://github.com/KFergusonUK)*

## The problem

Roadworks and street works data is published by hundreds of
separate authorities around the world, in inconsistent formats, on endpoints that change.
Anyone wanting a wider picture has to write and maintain bespoke
handling for each one.

## What it does

StreetWorks SDK normalises data from over 130 official sources, from around the world, into one
consistent model, so the source of a record stops mattering to the code
consuming it.

```bash
pip install streetworks
```

```python
# REPLACE THIS with a real short example from your own README —
# ideally one that fetches from two different authorities and shows
# that the output objects are identical in shape. That single snippet
# communicates the whole value of the library.
```

## Design notes

<!-- Worth writing up, because these are the interesting bits:
     - the Common Models layer and .to_common() converters
     - reliability grading across sources of differing quality
     - local government reorganisation awareness
     - handling county-level endpoints sunsetting from April 2027 -->

## Coverage

<!-- A table of the sources covered makes the "130+" claim inspectable
     rather than something a reader has to take on trust. Even a link
     to a coverage file in the repo works. -->

## Status

Current version 0.10.0. 
