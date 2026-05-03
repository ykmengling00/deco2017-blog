---
title: How I Am Using DDD to Plan My EazyCars Features
date: 2026-05-3
author: Milo Yang
summary: After finishing my wireframes in Blog 04, I realised that just showing what goes on the page is not enough, I also need to explain what data each element uses or produces. That is why I started learning DDD, a simple table format that describes data attributes, their meanings, and example values. This post is about how I am applying this method to EazyCars, especially to the car search and listing pages.
tags:
  - Data design
  - DDD
  - Planning
---
When I finished my wireframes for EazyCars, I felt good about the layout. I had a clear search bar at the top, simple navigation labels like Buy and Sell, and a clean car listing page with key details like price, mileage, and contact info. But then I asked myself a question I should have asked earlier: how does the back-end actually know what data to send to each part of the page?

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

I am doing this for every section of the wireframe — the homepage search, the listing grid, the car detail page, and the contact form. The goal is not to replace technical documentation. It is to create a shared vocabulary between me as the front-end designer and whoever builds the back-end. When we both look at the same DDD, we can discuss the data requirements without me having to write code or them having to read wireframes in detail.

One thing I found really useful is that DDD forces you to think about data naming early. When I wrote "listing id" instead of just "id," I realised that I had not clearly distinguished between a listing identifier and a user ID or a message ID. That kind of ambiguity would have caused bugs later. Catching it in the DDD stage — when nothing is built yet — means I can rename things without rewriting anything.

I also started thinking about data that comes from the BlaBla Corp platform itself. Since users are already logged in through the platform, I do not need a registration form. But I still need a way to identify the current user and link their activity to their account. That means my DDD also has to include platform-level data fields that I will receive from the session cookie, not from my own database.

The next step for me is to turn these DDD tables into actual database schema using SQLite, and then write MojoJS routes that serve the right data to each HTMX component. But first, I want to go through every page in the wireframe and finish the DDD entries. I estimate I have about six more sections to document before I can move to implementation with confidence.