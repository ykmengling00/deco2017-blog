---
title: My Wireframe Ideas for EazyCars
date: 2026-04-28
author: Milo Yang
summary: "I started building wireframes for EazyCars and realised that thinking about layout is also thinking about how easy it will be for someone to use. I focused on three things: keeping the learning cost low, making navigation obvious, and planning for accessibility from the beginning. This post is about what I was thinking and why I made the choices I did."
tags:
  - wireframe
  - user-experience
  - accessibility
---
When I first sketched out my wireframe for EazyCars, I did not think too far past where things would go on the page. I just drew boxes and lines and assumed I would figure the rest out later. That was a mistake. The wireframe stage is actually where the most important decisions about user experience get made — before any code exists, before any design polish gets applied.

The biggest question I kept asking myself was: how long would it take a brand new user to understand what this page does? If someone has never seen EazyCars before, can they land on the homepage and immediately know what to do? If the answer is no, something is wrong with the layout.

I put one big search bar right at the top — budget, make, model, location. Nothing else. I did not want a grid of categories, or a carousel of featured cars, or a wall of promotional banners sitting above the fold. All of that can come later, below the fold, for people who want to browse instead of search. But for someone who knows what they want, the fastest route in is a search bar. That is how I kept the learning cost low.

For navigation, I kept the labels plain. Buy. Sell. My Garage. Messages. I did not use clever words or design jargon. A first-time user should not need a glossary to understand where to click. I also added breadcrumbs on pages that go deeper into the site — like a specific car listing — so people always know where they are and how to go back.

The part I spent the most time thinking about was accessibility. I know the assignment asks for AA-level accessibility, so I tried to plan for it early. I made sure every section in the wireframe has a clear heading level — one H1 per page, then logical H2s and H3s below. I also marked landmark regions: header, navigation, main content, sidebar, footer. This way, a screen reader user can jump straight to the section they need.

I also thought about keyboard navigation. Every interactive element in the wireframe should be reachable without a mouse. That means planning tab order and making sure form fields have visible labels, not just placeholder text. I know placeholder text is easy to misuse — it disappears when you start typing and screen readers do not always announce it correctly. So every input field in the wireframe has a proper label sitting above or beside it.

I also considered loading speed at this stage. The assignment says pages should ideally load under one second. I made a note in my wireframe to keep images small and to avoid heavy third-party scripts on the homepage. If a user is on a slow mobile connection, they should still be able to see the search bar and start browsing within a couple of seconds.

Overall, this wireframe exercise taught me that design decisions made early are hard to undo later. Building in accessibility, clear navigation, and low learning cost is much easier when you are just moving boxes on a page than when you are rewriting code. I will be referring back to these wireframes as I start building the actual pages in MojoJS and HTMX.