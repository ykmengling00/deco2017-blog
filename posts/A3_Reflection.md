---
title: Reflection on EazyCars — A2 Web App Prototype
date: 2026-05-11
author: Milo Yang
summary: A reflective analysis of the EazyCars prototype built for DECO2017, covering performance evaluation, UX and accessibility testing, functional requirements decisions, and key lessons learned.
tags:
  - EazyCars
  - Reflection
  - DECO2017A3
---

<style>
img { max-width: 700px; height: auto; }
</style>

Most pages held up well. Buy, Sell, and Garage all hit 100 on Performance. Homepage came in at 76. That was the thing I had to think about.

![Figure 1: Lighthouse results for /](assets/images/a3/lighthouse-homepage.png)

The Homepage score was dragged down by a Largest Contentful Paint of 5.6 seconds. The page was loading a full-bleed image without any optimisation strategy. First Contentful Paint was quick at 0.3 seconds, which meant something was painting immediately, but the largest element on the page took much longer to resolve. That gap between FCP and LCP pointed straight at the image loading approach. A good FCP score is 1.8 seconds or less (Google, 2024), and the site was well within that. The LCP issue on Homepage was a separate problem that needed its own approach.

![Figure 2: Lighthouse results for /listings](assets/images/a3/lighthouse-buy.png)
![Figure 3: Lighthouse results for /listings/new](assets/images/a3/lighthouse-sell.png)
![Figure 4: Lighthouse results for /listings/:id](assets/images/a3/lighthouse-car-details.png)
![Figure 5: Lighthouse results for /garage](assets/images/a3/lighthouse-garage.png)

I went through the Lighthouse diagnostics to understand what was taking so long. Unused JavaScript was sitting at around 490 KiB across pages. The image payload on Homepage alone had estimated savings of over 6,000 KiB. Rather than scramble to optimise images the night before submission, I documented the issue clearly and noted what the fix would involve: lazy loading, explicit width and height attributes on image elements, and some form of image compression or responsive images. Cumulative Layout Shift on the detail page at 0.058 was also traced to images without explicit dimensions, which would be a straightforward fix in any future iteration.

One thing that worked out better than I expected was the HTMX partial rendering. When a user filters the listings page, only the list section refreshes. The navigation and surrounding layout stay intact. This kept the Buy page at 100 Performance without any special optimisation. I did not plan for this to be a performance feature. It was a UX decision first. But it turned out to matter for speed too, and it is the kind of decision that scales well as the app gets larger.

The lesson here was about not assuming. I thought the site would perform consistently across pages. Homepage at 76 taught me to test every route individually. A performance problem identified early is much cheaper to fix than one found after the project is complete.

The first axe DevTools run returned 31 color contrast violations. Every single one was on the dark navy navigation bar. I had picked that color because it looked right to me. What looks fine to a designer is not always what passes the contrast check (Costa, 2021). Rather than change the whole palette at once, I went through each failed element one at a time. Some text got lighter. Some elements got bolder. A few decorative things I thought were subtle were creating failures for screen readers too. After going through every template file and fixing each combination, every page came back with zero violations.

![Figure 6: axe DevTools results for Homepage](assets/images/a3/axe-devtools-homepage.png)
![Figure 7: axe DevTools results for /listings](assets/images/a3/axe-devtools-buy.png)
![Figure 8: axe DevTools results for /listings/new](assets/images/a3/axe-devtools-sell.png)
![Figure 9: axe DevTools results for /listings/:id](assets/images/a3/axe-devtools-car-details.png)
![Figure 10: axe DevTools results for /garage](assets/images/a3/axe-devtools-garage.png)

EU cookie compliance was new territory for me. The consent banner needed to appear before any cookies were set, give users a real choice between Accept and Reject, and remember that choice on their next visit. I read through the Directive 2002/58/EC to understand what valid consent actually means under the law (European Parliament and Council, 2002). The implementation I settled on checks for a consent cookie at the dispatch hook level, serves the banner as a clean template fragment via HTMX, and sets a persistent cookie once a choice is made. Using HTMX meant the banner disappears smoothly once the user clicks either button. No full page reload. No jarring transition. Both buttons are equally prominent, which is what the directive expects and what good usability practice supports (Rigou and Georgiadou, 2025).

One thing this project made clear is that accessibility and compliance work are most effective when treated as design inputs from the beginning.

Not everything I planned made it into the final build. This section looks at what I planned, what I built, and why some things changed.

The BlaBla Corp brief specified three core tables: listings, inquiries, and listing_images. I respected those and built around them. On top of that, I added a car_makes reference table so the Sell page could offer a dropdown of known makes rather than free text input. This came from noticing that free-text make fields on used car platforms tend to be full of inconsistent entries. A controlled vocabulary fixed that at the point of entry rather than having to clean it up later.

The feature I spent the most time thinking about not building was the real-time chat interface. My original idea was a persistent conversation thread between buyer and seller. After working through the MojoJS context model and talking through the scope, I realised a WebSocket-based chat was beyond what the prototype needed and would have introduced architectural complexity that pulled focus from the core listing and inquiry flow. I switched to a form-based inquiry instead: buyer fills out a message, it gets stored in the inquiries table, seller sees it in their garage page. No real-time component, but the function is complete and honest about what it is. Buyer email is never exposed to the seller, which is better for user privacy than the chat approach would have been.

The inquiry system as built does everything the BlaBla Corp data model requires. It is not the most sophisticated feature I could have imagined, but it is the right feature for this project. A half-finished chat feature would have been the wrong thing to build given the constraints of the assignment. Responsive design was implemented with CSS media queries. Navigation collapses to a hamburger menu below 768 pixels. Card grids shift from multi-column to single column on mobile. The cookie banner stacks its buttons vertically on very small screens. I tested this manually across different viewport sizes.

Three things from this project are coming with me to the next one.

First, accessibility is not a checkbox. I used axe DevTools because the brief required it, but I ended up understanding it in a way I had not before. Color is not decoration. It carries information (Costa, 2021). When that information is invisible to part of the audience, the design has failed whether most people notice or not. Next time I will run automated accessibility checks as part of the build process, not at the end when the options are harder and the deadline is closer.

Second, performance targets need to be verified. I thought the site would perform consistently and it did not. Buy, Sell, and Garage at 100 felt good. Homepage at 76 was a surprise. The lesson is simple: test every route, not just the ones that seem like they might be slow. A performance problem found in week three is much cheaper to fix than one found in week twelve.

Third, trade-offs are not failures. Dropping the chat feature in favour of a form-based inquiry was the right call for this project. It felt uncomfortable because I had to let go of something I had imagined, but the inquiry system is better for the constraints of the assignment than a half-finished chat feature would have been. A working prototype that honestly meets its brief is worth more than an impressive one that overscopes and underdelivers. EazyCars is not a finished platform and it was never meant to be. It is a prototype that proves the concept and demonstrates the process. The next version will be better because of what this one taught me.

References

BlaBla Corp. (2026). Design Brief 2026. University of Sydney, DECO2017 (ND).
Costa, M. (2021). Guidelines for accessibility in the historic city of Valletta. In Accessibility on the Web: Planning and Conformance. i-access.eu.
European Parliament and Council. (2002). Directive 2002/58/EC concerning the processing of personal data and the protection of privacy in the electronic communications sector. Official Journal of the European Communities, L 201/37.
Google. (2024). First Contentful Paint (FCP). web.dev. https://web.dev/articles/fcp
Hellbusch, J. (2024). Creating a better user experience through accessible web design. In Handbook of Accessible Communication. Springer.
Nielsen, J. (1994). 10 usability heuristics for user interface design. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/
Rigou, M. and Georgiadou, N. (2025). Design principles for cookie banners: Balancing legal compliance and usability. ResearchGate.
Weinbaum, N. and Kamp, R. (2024). The cookie conundrum: Balancing privacy, compliance and user experience. Journal of Data Protection & Privacy, 7(2).