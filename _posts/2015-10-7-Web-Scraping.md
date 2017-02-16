---
layout: post
title: "Scraping: aka build-your-own-JSON"
published: true
---

### What we're doing
Web scraping is a powerful technique that allows you to get the data you need by traversing the DOM of another site, picking the data you want and storing it for use. Be aware that if you are scraping continuously, and the site's layout changes, your "formula" may need to change. This post looks into the basics of scraping a site from your Node server and creating an object that you can serve or store.

### Tools for your quest
Since scraping is easiest to implement on simple sites (at least in layout) that see little change over time, we'll use a [Wikipedia site](https://en.wikipedia.org/wiki/List_of_paintings_by_Rembrandt) for this exercise that contains a fairly uniform table. We'll be grabbing the image url and title.

You'll need to npm install request and cheerio. Of particular interest, cheerio allows you to use jQuery like commands to traverse imported HTML... on your server. We'll assume your server is set up.

{% highlight javascript %}
// server.js highlight
var request = require('request');
var cheerio = require('cheerio');

app.get('/scrape', function (req, res, next) {
  var url = 'https://en.wikipedia.org/wiki/List_of_paintings_by_Rembrandt';
  request(url, function(error, response, html){
    // upcoming
  });
}
{% endhighlight %}

As you can see, we've set up a url variable to contain our target, a more robust scraping tool could perhaps update that variable to use across multiple pages of a site, but for now we'll keep it simple. Let's flesh this out more

{% highlight javascript %}
// server.js highlight
var request = require('request');
var cheerio = require('cheerio');

app.get('/scrape', function (req, res, next) {
  var url = 'https://en.wikipedia.org/wiki/List_of_paintings_by_Rembrandt';
  request(url, function(error, response, html){
    if(!error){
      var paintings = [];
      var $ = cheerio.load(html);
    }
  });
}
{% endhighlight %}

We've set up our variable paintings to store our objects as we parse them. The variable '$' may look a bit mysterious at the moment, but we'll be using it just like we would jQuery. Actually, let's do that now. Our goal is to find a point in the DOM to traverse from, something unique is often a good starting point, but since we'll only ever be grabbing the image and the title (which is right next to the image in the DOM structure), let's grab that directly. Our image resides in an "a" element with a class of "image". Let's start there.

{% highlight javascript %}
// server.js highlight
var request = require('request');
var cheerio = require('cheerio');

app.get('/scrape', function (req, res, next) {
  var url = 'https://en.wikipedia.org/wiki/List_of_paintings_by_Rembrandt';
  request(url, function(error, response, html){
    if(!error){
      var paintings = [];
      var $ = cheerio.load(html);
      $('a.image').each(function(){
        aElement = $(this);
        console.log(aElement);
      });
    }
  });
}
{% endhighlight %}

I threw the console log in to give you an idea of what we're interacting with. One of these "aElement"s shows up on your terminal as:

{% highlight bash %}
{ '0':
   { type: 'tag',
     name: 'a',
     attribs:
      { href: '/wiki/File:Rembrandt_The_Circumcision_in_the_Stable.jpg',
        class: 'image' },
     children: [ [Object] ],
     next: null,
     prev: null,
     parent:
      { type: 'tag',
        name: 'td',
        attribs: {},
        children: [Object],
        next: [Object],
        prev: [Object],
        parent: [Object] } },
  options:
   { withDomLvl1: true,
     normalizeWhitespace: false,
     xmlMode: false,
     decodeEntities: true },
  length: 1,
  _root: [Circular] }
{% endhighlight %}

You might notice it has children, next, prev, and parent- all things we'd expect to be able to use with jQuery. In this next section of code, let's use our newfound powers to grab the imageURL and title by using every "a" tag with a class of image as our starting point.

{% highlight javascript %}
// server.js highlight
var request = require('request');
var cheerio = require('cheerio');

app.get('/scrape', function (req, res, next) {
  var url = 'https://en.wikipedia.org/wiki/List_of_paintings_by_Rembrandt';
  request(url, function(error, response, html){
    if(!error){
      var paintings = [];
      var $ = cheerio.load(html);
      $('a.image').each(function(i, element){
        var json = { imageURL: "", title: ""};
        json.imageURL = "https:" + $(this).children().attr('src');
        json.title = $(this).parent().next().text();
        paintings.push(json);
      });
      sendPaintings(paintings);
    }
  });
  var sendPaintings = function(data){
    res.send(data);
  };
}
{% endhighlight %}

Awesome! You can now use your custom built JSON object to create a pretty snazzy art gallery.
