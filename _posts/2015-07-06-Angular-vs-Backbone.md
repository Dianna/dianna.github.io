---
layout: post
title: "Angular vs. Backbone- a quick overview"
published: true
---

###Why These Frameworks Exist
Both Angular and Backbone are popular frameworks that set out to tackle the problem of rapid update, single page apps. Both use the MV* framework to accomplish that goal but with different values that led to equally different frameworks. You may be familiar with either the concept of MVC frameworks or have done some work with Angular or Backbone and are therefore familiar with the concept that division of purpose is a smart move. Localized DOM interaction and data logic allows for greater ease in debugging and more readable code. These two frameworks differ in how 'oppinionated' they are.

###Expressive Angular
Angular was designed to be used for rapid development with teams that may not all be javascript engineers. It makes HTML more expressive through the use of customized directives which allows design oriented team-members a quick overview of which html elements constitute which parts of the page quckly and with minimal overhead. HTML was never intended to be used outside of the academic scope and has largely maintained it's newsprint tag names (e.g. <p>, <section>, <article>). When first reading through an HTML document, the reader is unlikely to know what all the pieces are for. Angular's directives take away the division between purpose and nomenclature (e.g. <picture-gallery>, <reviews>). Another exciting feature that can't be left out of this rapid overview is Angular's 2-way data binding that makes updating the View and its corresponding Model information automatic, saving the programmer many extra lines of code and possibly complex set up for every new model-view relationship. One possible con to Angular is that unless it is very well thought out and organized (pro-tip: do this), it can be hard to scale up.

###Open-Minded Backbone
Backbone gains its power from its small footprint and unopinionated framework. That being said, Backbone will not offer advice or guidance as Angular will with it's suggestive animations and division of tasks into minute bits (controllers, services and factories). Rather, it will give the bare structure necessary (collections, models & views) to organize purpose and leave the programmer all the room he needs to dream up whatever he wants to create.