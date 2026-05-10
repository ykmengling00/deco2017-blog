---
title: Website Accessibility, EU Cookie Compliance and Debugging Reflections
date: 2026-05-11
author: Milo Yang
summary: How I used axe DevTools to identify and fix 31 color contrast violations, implemented EU cookie consent, and learned to approach debugging methodically rather than getting stuck in dead-end loops.
tags:
  - EU Cookie
  - WCAG 2 AA
  - accessibility
---
When I first ran axe DevTools on the EazyCars prototype, I was not expecting much. The site looked fine to my eyes. But the tool found 31 accessibility violations across five pages — all of them color contrast issues. That was a wake-up call.

I ran scans on five pages: the homepage, the listings browse page, the sell page, the car detail page, and the garage page. axe reported issues exclusively in the color-contrast category, all tagged wcag2aa. The failures were concentrated in a handful of elements that appeared repeatedly across pages: buttons, location text, and the logout link in the navigation.

What surprised me was that many of these elements had what I considered "close enough" contrast. The buttons used a brand pink on white, which looked fine on screen. But the tool measured the ratios precisely — 3.82:1 where 4.5:1 is required for normal text under WCAG 2 AA. axe does not care about visual impressions. It cares about numbers.

Rather than fix each violation individually, I looked for the root causes. Most problems traced back to four CSS values:

The brand pink #e94560 was being used as both a background color (for buttons) and a text color (for links on dark backgrounds). On white backgrounds it produced good contrast for text, but on dark navy backgrounds it fell short. I made targeted adjustments — darkening button backgrounds to #c8324a to keep white text readable, changing location and hint text from #888888 to #555555 for better contrast on white, and switching the logout navigation link from pink to near-white #e8e8e8 against the dark nav background.

After the first round of fixes, axe still flagged one issue on every page: the logout link. The normal state had been fixed, but the hover state still used the old pink. The fix was straightforward once I identified it — normalize both the default and hover states to use near-white on dark.

The EU Cookie consent banner required a different kind of thinking. It is not just a UI element — it is a legal requirement with specific behavioral rules. The banner must appear on first visit, persist until the user makes a choice, and record that choice in a cookie for one year.

I implemented this using HTMX to handle the user interaction without a full page reload. The banner sits fixed at the bottom of every page via the layout template. A dispatch:before hook in the MojoJS application context checks for the consent cookie on every request — if the cookie exists, the banner is suppressed. When the user clicks "Accept" or "Reject," HTMX sends a POST to a /consent endpoint that writes the cookie and returns an empty response for HTMX to swap in, effectively removing the banner from the DOM.

The tricky part was getting the empty response right. MojoJS ctx.render() was wrapping the response in a layout, producing a full page instead of an empty fragment. I solved this by using ctx.render({ text: '<!-- cookie-consent-done -->' }) which gives HTMX something to swap while rendering nothing visible.

There is a debugging anti-pattern I caught myself falling into during this project: when something does not work, trying the same kind of solution repeatedly with minor variations, hoping something will eventually stick. I did this with the cookie banner problem — I must have tried eight different approaches before understanding why none of them were working.

The real fix was not more attempts. It was understanding the rendering pipeline: how MojoJS compiles templates, how the layout wraps content, and what HTMX actually receives when it makes a request. Once I understood that the template engine was wrapping the response in a layout even when I expected it not to, the solution became obvious.

Two things stayed with me from this process. First, automated tools are necessary but not sufficient. axe caught color contrast violations reliably, but it took human judgment to decide whether an element was decorative or meaningful, and to prioritize fixes that mattered across the whole site rather than one page at a time.

Second, accessibility is not a feature you add at the end. It is a constraint that shapes decisions from the beginning. If I had checked contrast ratios during initial design rather than after the interface was built, I would have chosen different default colors and saved considerable retrofitting work.

The EazyCars prototype now passes axe validation on all five scanned pages with zero violations. The EU cookie banner appears for new visitors and disappears after consent. Both of these feel like they belong to a more responsible kind of web development — one that considers who the users actually are and what they need.