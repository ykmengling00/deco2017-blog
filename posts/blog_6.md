---
title: Finding and Fixing a Template Naming Problem
date: 2026-05-03
author: Milo Yang
summary: "I spent an afternoon building the first version of my listings pages. Everything seemed correct: routes defined, controller methods written, template files created. But when I tried to visit the page, I got a 'Nothing could be rendered' error. This post is about how I found the problem, understood what was going wrong, and fixed it by learning how MojoJS expects template files to be named."
tags:
  - debugging
  - template
  - mojojs
---
I was excited to see my first listings page working. I had written the Model, created the Controller, added the routes, and set up the HTML templates. I opened the browser, went to /listings, and instead of seeing my car listings, I got a Server Error page that said "Nothing could be rendered."

I did not panic immediately. I read the error message and looked at what the server was telling me. The trace showed that the route was correct — it was routing to "listings#listPage" — but something was wrong after that. The server tried to render a view called "listings/listPage" and could not find anything to show.

I opened my views/listings folder and looked at what I had created. I named the main listing page "index.html.tmpl" because that is how a lot of web servers work by default — index means the main page. But MojoJS does not work that way. It looks for a template that matches the action name in the controller.

My controller action was called "listPage" so MojoJS was looking for a file called "listPage.html.tmpl", not "index.html.tmpl". I had named my file the wrong way round. I had been thinking about it from a file system perspective rather than from the framework's perspective.

I renamed three files to match the action names:
index.html.tmpl became listPage.html.tmpl
new.html.tmpl became newListingPage.html.tmpl
detail.html.tmpl became detailPage.html.tmpl

After restarting the server, the page loaded correctly. I tested the form submission and the detail page as well — everything worked.

The lesson I took from this is simple: when using a framework, the file naming conventions are not just suggestions. They are the rules that the framework uses to connect pieces together. I should have checked the MojoJS documentation for how templates are found before I started naming them. Next time, I will read the framework's conventions first, not assume, and test each piece before moving on to the next one.
