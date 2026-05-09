---
title: Getting Concrete — How the Data and User Flow Are Shaping Up
date: 2026-04-24
author: Milo Yang
summary: "After staying high-level for a few weeks, I drew out the actual user flow and worked through the data structure. This post documents the four-step flow I designed, the three database tables I planned, and the key trade-off I made on image storage. It also explains how privacy considerations shaped the inquiry design and how I thought about data minimisation from the start."
tags:
  - database-design
  - user-flow
  - data-privacy
---
After staying high-level for a few weeks, I sat down and tried to draw out what the app actually looks like when you use it. That exercise surfaced assumptions I had not questioned and forced me to make calls I had been avoiding.

The user flow has four steps. A buyer lands on the browse page and can search or filter listings. They click into a listing and see the full details plus the seller's trust profile. They fill out an inquiry form rather than seeing a phone number immediately. The seller receives the inquiry through the platform and responds. This keeps both parties' contact details private until they are ready to share.

The inquiry step is the part where privacy and data design intersect directly. I thought about this carefully: the inquiry form should only collect what is actually needed — the buyer's ID (from BlaBla's session, not entered manually), the listing ID, the message, and a timestamp. I should not ask for the buyer's email or phone number at this stage, because that data is not necessary for the inquiry to work and collecting it would go against data minimisation principles. This is the kind of decision that feels small but has real implications for user trust and compliance.

On the data side, I worked through what needs to be stored. The obvious table is listings — things like make, model, year, mileage, condition, price, description, and listing date. Then there is the user side, but the brief already says BlaBla handles authentication, so I am not storing login credentials. What I do need is a way to associate listings with a user ID and track inquiry records. I am planning three main tables:

Table: listings

| attribute | description | example value |
| listing id | unique identifier for this listing | 48392 |
| seller id | BlaBla user ID of the seller | 10291 |
| make | brand of the vehicle | Toyota |
| model | model name of the vehicle | Camry |
| year | year of manufacture | 2019 |
| mileage | odometer reading in kilometres | 45000 |
| condition | new, excellent, good, fair, poor | good |
| price | advertised price in AUD | $15,000 |
| description | seller-written description | One owner, serviced regularly... |
| listing date | when the listing was posted | 2026-04-20 |
| image url | link to the cover photo | /assets/img/camry-2019-hero.webp |

Table: inquiries

| attribute | description | example value |
| inquiry id | unique identifier for this inquiry | 78231 |
| listing id | which listing this inquiry is about | 48392 |
| buyer id | BlaBla user ID of the buyer | 20943 |
| message | the buyer's initial inquiry text | Is this still available... |
| sent at | timestamp when inquiry was sent | 2026-04-21 14:30 |

Table: seller_profiles

| attribute | description | example value |
| seller id | BlaBla user ID, matches listings.seller_id | 10291 |
| account age | months since first listing was posted | 6 |
| active listings | how many listings this seller currently has | 2 |

One concrete decision I made: I am not storing images in the database. SQLite does not handle large blobs well, and the prototype environment does not have robust file storage. Instead, I will store image URLs and the listings will pull images from publicly accessible URLs. This means listings will not always have real photos, but it is the only realistic approach for this scope. Trade-off: I chose URL-based images because building a file upload system in SQLite would add complexity that does not serve the prototype's core purpose.

I also had to revisit my trust layer idea. I initially imagined a detailed reputation system, but without real transactions, there is nothing to base that on. I scaled it back to account age and active listing count. These are not strong trust signals, but they are better than nothing and they come from data I already have.

What is left to figure out: whether the inquiry response should be a message thread or a simple one-message reply. I am leaning toward the latter to keep the prototype stable, but the community context of this platform makes me want to explore the thread approach in a future iteration.