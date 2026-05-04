---
title: Cutting Down to What Matters — Scoping the Core Features
date: 2026-04-24
author: Milo Yang
summary: After identifying what the platform needs to do at a high level, I had to make hard decisions about what actually goes into the prototype. This post documents how I prioritised the core features, what trade-offs I made, and why I chose to build a skeleton of trust rather than a working reputation system.
tags:
  - scope-management
  - core-features
  - feasibility
---
After identifying what the platform needs to do at a high level, I had to make hard decisions about what actually goes into the prototype. That was harder than I expected.

The core I landed on: this app needs listings, a way to search and filter them, some kind of trust layer so buyers are not going in blind, and a way for buyers to inquire without immediately handing over their phone number. That is it. Everything else is a nice-to-have that will dilute what the prototype actually demonstrates.

The priority order was not obvious. I initially thought the trust layer should be the centrepiece — that is what makes this different from Gumtree. But the more I thought about it, the more I realised trust cannot exist without listings first. And without search, no one will find the listings. So I reordered.

On listings — sellers need to add key details: make, model, year, mileage, condition, price, and at least one photo. Without that foundation, nothing else matters. I put this first because without a listing, nothing else exists in the system.

Search and filter is non-negotiable. Nobody wants to scroll through fifty listings that do not match what they are looking for. Price range, body type, fuel type, location — these feel like the essentials. I ranked this second because it is the primary way buyers interact with the system. If search is frustrating, nothing else matters no matter how good the trust layer is.

The trust layer — I really wanted this to be the differentiator. I imagined detailed reputation scores, community warnings, completed sale counts. But I had to be honest with myself: in a prototype with no real transactions, there is nothing to build a reputation system on. I scaled it back to showing account age and active listing count. It is not a real trust signal yet, but it is something I can demonstrate in the UI and expand later. Scope constraint: I chose to build a skeleton of trust rather than a working system, because the latter requires real data I do not have.

For inquiries, a structured form that goes directly to the seller through the platform protects buyer privacy and keeps everything within BlaBla's ecosystem. HTMX should handle the inline interaction without much trouble. I ranked this fourth because it is a secondary flow — most buyers will browse before they inquire, so browsing has to work well first.

What I am still unsure about: whether to include pricing guides or market trend data. That either needs external data or a fair amount of logic I am not sure I can pull off cleanly in the time available. I am also not sure yet how image uploads will work with SQLite's storage constraints — I might end up using URL-based images to keep it simple.

The biggest trade-off I made was cutting the messaging thread feature I originally planned. A real message thread between buyer and seller felt like the right UX, but it means building a full inbox system — which is essentially a second major feature on top of listings. The prototype window is tight, and I decided to replace it with a one-message inquiry form instead. It loses some functionality, but it keeps the scope tight enough that I can actually finish something coherent.

My biggest risk is scope creep. Every feature I considered had to answer one question before I moved it up the list: does this help a buyer find or contact a car? If not, it waited.