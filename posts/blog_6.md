---
title: Finding and Fixing a Template Naming Problem
date: 2026-05-03
author: Milo Yang
summary: My first attempt at rendering a listings page gave me a "Nothing could be rendered" error. This post documents how I traced the problem to a naming mismatch between my controller actions and template filenames — MojoJS expects templates to match the action name, not follow the "index as default" convention I assumed from other frameworks.
tags:
  - debugging
  - template
  - mojojs
---
I was excited to see my first listings page working. I had written the Model, created the Controller, added the routes, and set up the HTML templates. I opened the browser, went to /listings, and instead of seeing my car listings, I got a Server Error page that said "Nothing could be rendered."

I did not panic immediately. I read the error message and looked at what the server was telling me. The trace showed that the route was correct — it was routing to "listings#listPage" — but something was wrong after that. The server tried to render a view called "listings/listPage" and could not find anything to show.

I opened my views/listings folder and looked at what I had created. I named the main listing page "index.html.tmpl" because that is how a lot of web servers work by default — index means the main page. But MojoJS does not work that way. It looks for a template that matches the action name in the controller.

My controller action was called "listPage" so MojoJS was looking for a file called "listPage.html.tmpl", not "index.html.tmpl". I had been thinking about it from a file system perspective rather than from the framework's perspective.

I renamed three files to match the action names: index.html.tmpl became listPage.html.tmpl, new.html.tmpl became newListingPage.html.tmpl, and detail.html.tmpl became detailPage.html.tmpl.

After restarting the server, the page loaded correctly. I tested the form submission and the detail page as well — everything worked.

The lesson I took from this is simple: when using a framework, the file naming conventions are not just suggestions. They are the rules that the framework uses to connect pieces together. I should have checked the MojoJS documentation for how templates are found before I started naming them. Next time, I will read the framework's conventions first, not assume, and test each piece before moving on to the next one.

What this also reminded me: the DDD I wrote earlier in the process paid off here. When I was sketching out what data the listing page needed, I had already defined the listing id, make, model, year, price, mileage, and image url as separate fields. Because of that early thinking, I knew exactly what to query from the database when the page finally rendered. The data model was clear; the only problem was the file name.