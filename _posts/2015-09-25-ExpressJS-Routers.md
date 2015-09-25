---
layout: post
title: "Express.js Routers"
published: true
---

###Express Routing Basics
When you create a basic server with Express, it comes with a built in router. This is great for getting started on your full-stack app, but as your backend service needs grow, your middleware can start to look pretty crowded. To keep code modular, the Express Router method can be used.

###Getting Started
Let's go ahead and create our simple server. This assumes you already have Express installed.

{% highlight javascript %}
// server.js
var express = require('express');
var awesomeApp = express();

awesomeApp.listen(3030, function(){
  console.log('awesomeApp is listening on localhost:3030');
});
{% endhighlight %}

Pretty simple. At this point you can start serving, say, "Hello human friend".

{% highlight javascript %}
// server.js
var express = require('express');
var awesomeApp = express();

awesomeApp.get('/', function(req, res){
  res.json('Hello human friend');
});

awesomeApp.listen(3030, function(){
  console.log('awesomeApp is listening on localhost:3030');
});
{% endhighlight %}

So this is great, but as mentioned before, what if our front end started to have a wide assortment of requests- not only for robot greetings but also robot insults and predictions as well? We wouldn't want our server file to become cluttered and unreadable. To handle the increased server load lets set up our routers.

{% highlight javascript %}
// server.js
var express = require('express');
var awesomeApp = express();

var greetingRouter = Express.Router(),
    insultRouter = Express.Router(),
    predicitionRouter = Express.Router();

require('/greetings/greetingsRoutes.js')(greetingRouter);
require('/insults/insultsRoutes.js')(insultRouter);
require('/predictions/predictionsRoutes.js')(predictionRouter);

awesomeApp.use('/greetings', greetingRouter);
awesomeApp.use('/insults', insultRouter);
awesomeApp.use('/predictions', predictionRouter);

awesomeApp.listen(3030, function(){
  console.log('awesomeApp is listening on localhost:3030');
});
{% endhighlight %}

Now when our server receives a request with a url "/insults/smarmy" the server directs the request to our insultRouter which then continues to handle the request.

{% highlight javascript %}
// inults/insultsRoutes.js
module.exports = function(insultRouter){
  insultRouter.get('/smarmy', function(req, res){
    res.json('Came back for another one did you meat man? I\'d say you have a screw loose, but you seem to have lost all your screws.');
  });
  insultRouter.get('/uncalledFor', function(req, res){
    res.json('100010101010101101111010101010101101010010101010110101');
  });
};
{% endhighlight %}

You can see that our insultRouter is set up to serve a variety of insults for any occasion. This is a pretty basic example, but hopefully it gives you some useful tools to get started on a well organized back-end set up.
