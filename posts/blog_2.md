---
title: Cutting Down to What Matters — Scoping the Core Features
date: 2026-04-24
author: Milo Yang
summary: After a week of feature brainstorming and hard cuts, this post documents how the initial idea list got trimmed to a focused set of core requirements, and what trade-offs were made to keep the prototype achievable.
tags:
  - scope-management
  - core-features
  - feasibility
---
I've been trying to pin down the actual functionality, and it's trickier than I expected. I kept writing lists of features and then realising half of them were out of scope for a prototype. So I forced myself to cut hard.

The core I landed on: this app needs listings, a way to search and filter them, some kind of trust layer so buyers aren't going in blind, and a way for buyers to inquire without immediately handing over their phone number. That's it. Everything else is a nice-to-have that will dilute what the prototype actually demonstrates.

On listings — sellers need to add key details: make, model, year, mileage, condition, price, and at least one photo. Without that foundation, nothing else matters.

Search and filter is non-negotiable. Nobody wants to scroll through fifty listings that don't match what they're looking for. Price range, body type, fuel type, location — these feel like the essentials.

The trust layer is where I'd like to make this feel different from a standard classifieds site. Rather than relying solely on a seller rating (which is easy to inflate and doesn't tell you much), I want to explore whether the community can generate its own credibility signals. How many sales has this person completed? Are there any discussions or warnings attached to their account? It's about making information visible before the buyer commits to an inquiry.

For inquiries, a structured form that goes directly to the seller through the platform feels right. It protects buyer privacy and keeps everything within BlaBla's ecosystem. HTMX should handle the inline interaction without much trouble.

What I'm still unsure about: whether to include pricing guides or market trend data. That either needs external data or a fair amount of logic I'm not sure I can pull off cleanly in the time available. I'm also not sure yet how image uploads will work with SQLite's storage constraints — I might end up using URL-based images to keep it simple.

My biggest risk is scope creep. I need to get the basic listing and inquiry flow working first, then decide what to add next.
