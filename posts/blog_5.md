---
title: How I Am Using DDD to Plan My EazyCars Features
date: 2026-05-03
author: Milo Yang
summary: "After finishing my wireframes, I realised I had not answered a basic question: how does the back-end know what data to send to each part of the page? This post documents how I applied DDD to document data requirements, why I chose MojoJS and SQLite for this prototype, and how I planned for accessibility testing and performance validation from the start so they are built into the architecture rather than retrofitted."
tags:
  - Data design
  - DDD
  - evaluation-planning
---
When I finished my wireframes for EazyCars, I felt good about the layout. But then I asked myself a question I should have asked earlier: how does the back-end actually know what data to send to each part of the page?

That question led me to DDD — Data Definition Document. The concept from Week 9 tutorial: instead of just drawing boxes, you write down what information each box needs or creates, in a table with three columns: attribute, description, and example value. No typescript types, no database fields — just clear descriptions that anyone on the team can understand.

Let me show what I found when I applied this to my own wireframe.

The homepage search bar is not just a UI element — it generates a data record:

| attribute | description | example value |
| car search query | filters for browsing available cars in the community | Toyota Camry, $15000, Sydney NSW |

The search result list that appears below also needs its own DDD entry:

| attribute | description | example value |
| listing id | unique identifier for each car listing | 48392 |
| car make | brand of the vehicle | Toyota |
| car model | model name of the vehicle | Camry |
| car year | year of manufacture | 2019 |
| price | advertised price in AUD | $15,000 |
| mileage | odometer reading in kilometres | 45000 |
| listing image url | link to the cover photo of the car | /assets/img/camry-2019-hero.webp |

One thing I found really useful: DDD forces you to think about data naming early. When I wrote "listing id" instead of just "id," I realised I had not clearly distinguished between a listing identifier and a user ID or a message ID. That kind of ambiguity would have caused bugs later. Catching it in the DDD stage means I can rename things without rewriting anything.

I also discovered that data can live on relationships, not just on entities. My inquiry form generates an inquiry record that links a buyer to a listing. The inquiry record is not owned by either the buyer or the listing — it belongs to the relationship between them. That is something I had not thought about before DDD forced me to write it out.

From the DDD, I was able to sketch a simple ERD showing how my three tables relate:

- user creates listing: one user can have many listings, each listing has one seller (1 : many)
- listing receives inquiry: one listing can have many inquiries, each inquiry is about one listing (1 : many)
- user sends inquiry: one user can send many inquiries, each inquiry is from one buyer (1 : many)

The DDD also made visible something I had glossed over: my wireframe assumed a logged-in user, but I had not defined what user data the front-end would receive from BlaBla's session cookie. That is platform-level data I need to handle, even though it is not in my own database.

Beyond DDD, I also had to make concrete technical decisions. The brief specifies MojoJS, SQLite, and HTMX. At first I accepted this without questioning why, but understanding the "why" matters for making good decisions.

MojoJS has built-in template rendering and session management, which reduces the number of libraries I need to integrate. That matters for a prototype where I want to keep things simple and avoid the overhead of configuring multiple dependencies. SQLite runs without a separate server process — the database is just one file on disk. For a six-week prototype, not needing to set up and maintain a database server is a real time saver. HTMX is already included in the template, and it lets me add interactive behaviours like form submissions and partial page updates without writing client-side JavaScript. This also means fewer external scripts to load, which directly helps with the loading time target.

The trade-off is clear: this stack is not designed for high-traffic production use, and SQLite has limits on write concurrency. But for a prototype with a defined scope and a six-week window, these constraints are acceptable. I am not building for scale — I am building for a demonstration of function.

On evaluation: I have planned to test each route with Lighthouse for performance and axe DevTools for accessibility. I will also run manual tests with keyboard-only navigation and with NVDA screen reader to catch issues that automated tools miss. For EU cookie compliance, since I am deliberately avoiding third-party tracking cookies and only using session cookies necessary for the app to function, I expect the compliance burden to be minimal — but I will still add a small note in the footer clarifying the cookie policy to be thorough.