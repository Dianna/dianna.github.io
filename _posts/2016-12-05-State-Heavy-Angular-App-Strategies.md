---
layout: post
title: "State-Heavy Angular App Strategies"
published: true
---

Angular is an incredibly powerful tool for creating rich SPAs quickly. As an MVC framework, it gives developers the tools they need to keep their engineering concerns separate, but inconsistent use or even misuse of these tools can lead to a bloated app that’s hard to debug and scale. During my time growing and refactoring a large state-heavy Angular app, I spent many hours researching best practices which, over the course of year, I refined to best suit my needs. I’ve gathered the bigger takeaways here.

###State
Keep this centralized. *Seriously*. For each of your different models, create a unique service. That service should retrieve and cache (if necessary) all such models and do nothing else.

###Logic
I recommend having separate services for state and logic if you expect your app to remain scalable. If your data requires parsing or re-formatting before you’re prepared to save it as part of your app’s state, that should still happen in a separate service than that model’s state retrieval and caching service.

If you find a service is getting really bloated, you likely need to create another service to offload some of the logic. As an example, for a grade dashboard I created, I had separate services for formatting dates, parsing class grades for viewing, sorting grade results, creating header templates for the class report, and parsing grade results into an exportable format.

###Views
It’s usually pretty obvious how a page can be broken down into separately viewable pieces—the sidebar, navbar, content, comments, individual comment, etc… When adding a new route and its view, identify these pieces and relegate them to individual partial views or directives (depending on behavior). These views and directives can then be used in the central view for that route’s module. In this way, the central view for the module is concise and all of its components are easily identifiable. Each directive maintains one purpose which makes growth, reuse and debugging of these directives and their parent view considerably simpler.

###Keeping views up to date
Having designated all logic and state to services, it’s now easy to keep controllers thin. Use controllers to act as traffic control between the view and the information within the services. The controller for the grade dashboard I mentioned in the logic section is <300 lines of code, and that was a HUGE feature. Use two-way binding when possible. The fewer middlemen between state services and the view, the easier to scale and debug.

###Some notes on File Structure
Your code should be as modular as possible to ensure easy navigation. Strive to keep any specialized services, directives, etc… within the same module as the app route they are used by. State and logic services, which will be used across the app, should not be within one route’s module. Similarly, when a directive is used across many different routes, it’s earned it’s place somewhere more global. I’m being a bit wishy-washy on the particulars because this depends enormously on what your app is actually comprised of, but whatever structure you choose, try to keep code that deals with the same view within one folder as much as possible. That way, when you need to update or remove that view there’s no code hunting and minimal code extrication from other modules.
