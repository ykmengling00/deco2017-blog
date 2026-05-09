---
title: My Wireframe Ideas for EazyCars
date: 2026-04-28
author: Milo Yang
summary: "The wireframe stage is where the most important UX decisions get made — before any code exists. This post documents how I designed the homepage around a single prominent search bar, planned for AA-level accessibility from the start as an architectural decision rather than an afterthought, and made deliberate choices about loading performance that reflect the brief's speed requirements."
tags:
  - wireframe
  - user-experience
  - accessibility
---
When I first sketched my wireframe for EazyCars, I did not think much past where things would go on the page. I just drew boxes and lines and assumed I would figure the rest out later. That was a mistake. The wireframe stage is where the most important decisions about user experience get made — before any code exists, before any design polish gets applied.

The biggest question I kept asking myself was: how long would it take a brand new user to understand what this page does? If someone has never seen EazyCars before, can they land on the homepage and immediately know what to do? If the answer is no, something is wrong with the layout.

I put one big search bar right at the top — budget, make, model, location. Nothing else. No grid of categories, no carousel of featured cars, no wall of promotional banners sitting above the fold. All of that can come later, below the fold, for people who want to browse instead of search. But for someone who knows what they want, the fastest route in is a search bar. That is how I kept the learning cost low.

For navigation, I kept the labels plain. Buy. Sell. My Garage. Messages. A first-time user should not need a glossary to understand where to click. I also added breadcrumbs on pages that go deeper into the site — like a specific car listing — so people always know where they are and how to go back.

The part I spent the most time thinking about was accessibility. I know the assignment asks for AA-level accessibility, so I tried to plan for it early rather than bolt it on at the end. Every section in the wireframe has a clear heading level — one H1 per page, then logical H2s and H3s below. I also marked landmark regions: header, navigation, main content, sidebar, footer. This way, a screen reader user can jump straight to the section they need.

I also planned for keyboard navigation from the start. Every interactive element should be reachable without a mouse. Form fields have visible labels, not just placeholder text — because placeholder text disappears when you start typing and screen readers do not always announce it correctly. I also made sure all form inputs have associated labels using the for attribute, and that error states are communicated both visually and through ARIA live regions so screen reader users know when something went wrong.

I also thought about loading speed at this stage. The assignment says pages should ideally load under one second and never exceed three seconds. I made a note to keep images small and avoid heavy third-party scripts on the homepage. If a user is on a slow mobile connection, they should still be able to see the search bar and start browsing within a couple of seconds. I plan to test this with browser DevTools Network tab and Lighthouse, measuring on a simulated 3G connection to reflect real-world mobile conditions. I will also test with a screen reader running to ensure accessibility performance does not degrade with latency.

Overall, this wireframe exercise taught me that design decisions made early are hard to undo later. Building in accessibility, clear navigation, and low learning cost is much easier when you are just moving boxes on a page than when you are rewriting code.