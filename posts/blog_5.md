---
title: How I Am Using DDD to Plan My EazyCars Features
date: 2026-05-03
author: Milo Yang
summary: After finishing wireframes, I realised showing what goes on the page is not enough — I also need to explain what data each element uses or produces. This post documents how I am applying DDD to EazyCars and why I chose specific technical approaches based on the project's actual requirements.
tags:
  - Data design
  - DDD
  - technical-decisions
---
When I finished my wireframes for EazyCars, I felt good about the layout. I had a clear search bar at the top, simple navigation labels like Buy and Sell, and a clean car listing page with key details. But then I asked myself a question I should have asked earlier: how does the back-end actually know what data to send to each part of the page?

That question led me to DDD — Data Definition Document. The concept from Week 9 tutorial is straightforward. Instead of just drawing boxes, you also write down what information each box needs or creates. You do it in a table with three columns: attribute, description, and example value. No typescript types, no database fields — just clear descriptions that anyone on the team can understand.

Let me give a real example from EazyCars. In my wireframe, there is a search bar at the top of the homepage where users type budget, make, model, and location. That search bar is not just a UI element — it generates a data record. Here is what my DDD entry looks like:

attribute | description | example value
car search query | filters for browsing available cars in the community | Toyota Camry, $15000, Sydney NSW

The search result list that appears below the bar also needs its own DDD entry. For each car card in the listing, I need to define what data the front-end expects from the back-end:

attribute | description | example value
listing id | unique identifier for each car listing | 48392
car make | brand of the vehicle | Toyota
car model | model name of the vehicle | Camry
car year | year of manufacture | 2019
price | advertised price in AUD | $15,000
mileage | odometer reading in kilometres | 45000
listing image url | link to the cover photo of the car | /assets/img/camry-2019-hero.webp

One thing I found really useful is that DDD forces you to think about data naming early. When I wrote "listing id" instead of just "id," I realised that I had not clearly distinguished between a listing identifier and a user ID or a message ID. That kind of ambiguity would have caused bugs later. Catching it in the DDD stage — when nothing is built yet — means I can rename things without rewriting anything.

I also started thinking about data that comes from the BlaBla Corp platform itself. Since users are already logged in through the platform, I do not need a registration form. But I still need a way to identify the current user and link their activity to their account. That means my DDD also has to include platform-level data fields that I will receive from the session cookie, not from my own database.

But DDD alone is not enough. I also had to make concrete technical decisions. The brief specifies MojoJS, SQLite, and HTMX as the required stack. At first I accepted this without questioning why, but later I realised understanding the "why" matters for making good decisions.

MojoJS was chosen because it has built-in template rendering and session management, which reduces the number of libraries I need to integrate. That matters for a prototype where I want to keep things simple. SQLite makes sense because it runs without a separate server process — the database is just one file on disk. For a six-week prototype, not needing to set up and maintain a database server is a real time saver. HTMX is already included in the template, and it lets me add interactive behaviours like form submissions and partial page updates without writing client-side JavaScript.

The trade-off is clear: this stack is not designed for high-traffic production use. SQLite has limited write concurrency, and HTMX is not a full JavaScript framework. I know this, and I made the call based on the project's actual needs — a working prototype, not a scalable product. If the scope changes and I need PostgreSQL or a React front-end later, I can adapt. But for where I am right now, these tools fit the problem.

The next step is turning these DDD tables into actual database schema using SQLite, and then writing MojoJS routes that serve the right data to each HTMX component. But first, I want to go through every page in the wireframe and finish the DDD entries. I estimate I have about six more sections to document before I can move to implementation with confidence.