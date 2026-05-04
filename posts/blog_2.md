---
title: Cutting Down to What Matters — Scoping the Core Features
date: 2026-04-24
author: Milo Yang
summary: After a week of feature brainstorming and hard cuts, this post documents how the initial idea list got trimmed to a focused set of core requirements, what trade-offs were made, and why each priority decision was made based on prototype constraints and user needs.
tags:
  - scope-management
  - core-features
  - feasibility
---
I've been trying to pin down the actual functionality, and it's trickier than I expected. I kept writing lists of features and then realising half of them were out of scope for a prototype. So I forced myself to cut hard.

The core I landed on: this app needs listings, a way to search and filter them, some kind of trust layer so buyers aren't going in blind, and a way for buyers to inquire without immediately handing over their phone number. That's it. Everything else is a nice-to-have that will dilute what the prototype actually demonstrates.

On listings — sellers need to add key details: make, model, year, mileage, condition, price, and at least one photo. Without that foundation, nothing else matters. I put this first because without a listing, nothing else exists in the system.

Search and filter is non-negotiable. Nobody wants to scroll through fifty listings that don't match what they're looking for. Price range, body type, fuel type, location — these feel like the essentials. I ranked this second because it's the primary way buyers interact with the system. If search is frustrating, nothing else matters no matter how good the trust layer is.

The trust layer is where I'd like to make this feel different from a standard classifieds site. Rather than relying solely on a seller rating, I want to explore whether the community can generate its own credibility signals. How many sales has this person completed? Are there any discussions or warnings attached to their account? It's about making information visible before the buyer commits to an inquiry. But I ranked this below search because it depends on having listings first, and in a prototype with no real transactions, there's nothing to build a reputation system on. It has to come after the foundation.

For inquiries, a structured form that goes directly to the seller through the platform feels right. It protects buyer privacy and keeps everything within BlaBla's ecosystem. HTMX should handle the inline interaction without much trouble. I ranked this fourth because it is a secondary flow — most buyers will browse before they inquire, so browsing has to work well first.

What I am still unsure about: whether to include pricing guides or market trend data. That either needs external data or a fair amount of logic I'm not sure I can pull off cleanly in the time available. I'm also not sure yet how image uploads will work with SQLite's storage constraints — I might end up using URL-based images to keep it simple.

The biggest trade-off I made was cutting the messaging thread feature I originally planned. A real message thread between buyer and seller felt like the right UX, but it means building a full inbox system — which is essentially a second major feature on top of listings. The prototype window is tight, and I decided to replace it with a one-message inquiry form instead. It loses some functionality, but it keeps the scope tight enough that I can actually finish something.

My biggest risk is scope creep. I need to get the basic listing and inquiry flow working first, then decide what to add next. Every feature I considered had to answer one question before I moved it up the list: does this help a buyer find or contact a car? If not, it waited.