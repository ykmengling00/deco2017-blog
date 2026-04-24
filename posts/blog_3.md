---
title: Getting Concrete — How the Data and User Flow Are Shaping Up
date: 2026-02-14
author: Milo Yang
summary: Moving from ideas to actual structure, this post documents the decisions made about what data the app needs to track, how listings relate to users, and the concrete flow a buyer goes through from searching to making an inquiry.
tags:
  - database-design
  - user-flow
  - concrete-decisions
---
After a few weeks of staying high-level, I finally sat down and tried to draw out what the app actually looks like when you use it. That exercise was more useful than I expected — it surfaced assumptions I hadn't questioned and forced me to make calls I'd been avoiding.

The flow I landed on has four main steps. First, a buyer lands on the browse page and can search or filter listings. Second, they click into a listing and see the full details plus the seller's trust profile. Third, they fill out an inquiry form rather than seeing a phone number immediately. Fourth, the seller receives the inquiry through the platform and responds — keeping both parties' contact details private until they're ready to share.

That inquiry step is the part I feel most uncertain about. It's clearly the right call for privacy, but I'm not sure yet whether it should feel more like a message thread or a structured form submission. The difference matters for how I build the back-end. I'm leaning toward a message-thread style because it feels more natural in a community context, but that means I'm essentially building a messaging feature, which is more complex than I initially planned. I might scale it back to a simple one-message inquiry to keep the prototype stable.

On the data side, I spent a while working through what needs to be stored. The obvious table is listings — things like make, model, year, mileage, condition, price, description, and listing date. Then there's the user side, but the brief already says BlaBla handles authentication, so I'm not storing login credentials. What I do need is a way to associate listings with a user ID and track inquiry records. I'm thinking three main tables: one for listings, one for inquiries, and one for seller profiles — where the profile is a thin layer on top of the BlaBla user ID that adds community-specific data like how many successful sales they've completed and their account age.

One concrete decision I made: I'm not storing images in the database. SQLite doesn't handle large blobs well, and the prototype environment doesn't have robust file storage. Instead, I'll store image URLs and the listings will pull images from publicly accessible URLs. This means listings won't always have real photos, but it's the only realistic approach for this scope. I might add a placeholder image fallback for listings that don't have one yet.

I also had to revisit my trust layer idea. I initially imagined a detailed reputation system, but in a prototype with no real transactions, there's nothing to base that on. I've scaled it back to something simpler: showing account age and a count of active listings. It's not a real trust signal yet, but it's something I can build the UI around and expand later.

The accessibility question I raised in an earlier post is still on my mind. I've started sketching the page structure with semantic HTML in mind — proper headings, labels on form fields, alt text on images. I'm testing early with keyboard navigation to catch issues before they become entrenched in the layout.

What's left to figure out: the inquiry response flow (message vs. form), pagination for the browse page, and whether I need a "saved listings" feature to demonstrate a user's personal space on the platform.