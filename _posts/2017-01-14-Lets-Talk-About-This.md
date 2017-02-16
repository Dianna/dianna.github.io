---
layout: post
title: "Let's Talk About this"
published: true
---

The concept of *this* in Javascript is much blogged about. In my time perusing such posts, some have proved more helpful than others. Writing this post if for me a therapeutic rehash of concepts long ago turned to second nature, maybe, for someone else, this is the post that finally makes the concept click. Ya, let’s go with that.

By default, when *this* is not set in any particular context it will simply be the window. If you open your console and type *this*, the window object is what you get.

The most important thing to understand about *this* is that it is NOT static. It changes depending on its context and the same *this* in your line of code could hypothetically be many different things depending, for example, on how a function is called. Let’s examine these mysterious function scenarios.

### Mysterious function scenarios
#### Global scope
If I create the following function in my console and call it. What will the output be?

{% highlight javascript %}
function thisRetriever () {
  return this;
}
{% endhighlight %}

If you thought the window object then you get a gold star. Why did we get the window? This will become clearer as we continue, but for the time just understand that *this* will never refer to a function. Since *this* has no explicit context, it defaults to window.

#### Object scope
So how can we create a context for our *thisRetriever* function? For starters, we can make it the method of an object. When this happens, the context becomes the object. We can do this two ways.

{% highlight javascript %}
// First, assuming thisRetriever is defined as above:
var tree = {
  leaves: 100
};
tree.thisRetriever = thisRetriever;

// Second, if thisRetriever is not yet defined:
var tree = {
  leaves: 100,
  thisRetriever: function() {
    return this;
  }
};
{% endhighlight %}

What would happen if, in either of these scenarios I called `tree.thisRetriever()`? I would get the tree object that serves as its context. It is possible to override this context. Let’s explore that next.

### Overriding Context
#### Call and Apply
It is possible to set the context for your function in a variety of ways. An explicit way which will override any context (except in the case of *bind*, which we’ll tackle later) is by using the *call* and *apply* methods. The first argument of both of these methods is the thisArg. If you leave it as *null* it will default to the window; however, you can explicitly state your desired context here. Look at the scenario below. What do you think the returned value of `tree.thisRetriever` will be in each example?

{% highlight javascript %}
function thisRetriever() {
  return this;
}

var gamestore = {
  scythe: “epic”,
  smallworld: “overplayed”,
  thisRetriever: thisRetriever
}

// Example 1
thisRetriever.();
// Example 2
thisRetriever.call(gamestore);
// Example 3
thisRetriever.apply(gamestore);
// Example 4
gameStore.thisRetriever();
// Example 5
gameStore.thisRetriever.call(window);
{% endhighlight %}

If you said it would return the object `window` for examples 1 and 5 but `gamestore` for 2, 3, and 4 you’re correct! We explicitly set the context to gamestore in examples 2 and 3, while in example 5 we overrode the context to set it as the `window`. Note that example 5 could be called as `gameStore.thisRetriever.call()` and the result would still be `window` because the method will ALWAYS override the context with something (which is `window` by default).

#### Bind
Using *bind* is another notoriously difficult concept to master. So I’ll only touch on it here and add another post in the future to illuminate *bind*’s murky depths. The important thing to note is that *bind* will permanently set the context and scope of a function on definition. Here’s an example:

{% highlight javascript %}
function gameRetriever () {
  return this.game;
}

var chris = {game: “pandemic legacy”};
var dianna = {game: “scythe”};

var gameNight = gameRetriever.bind(chris);
var gameNightTwo = gameNight.bind(dianna);

gameNight() //returns “pandemic legacy”
gameNightTwo() // ALSO returns “pandemic legacy”
{% endhighlight %}

Since `gameNightTwo` utilizes the same function instance as the function `gameNight` and the context was permanently set by *bind* we will only ever be able to play Pandemic Legacy. *Sigh*. Takeaway: since the context is permanently set, *this* will never change, even if I use *call* or *apply*.

### Contructors
The final major scenario to consider is *this* in the context of constructors. A constructor, in Javascript is simply a function that creates a new object. I will post a more in depth review of constructors in a future post. Here’s the basics:

{% highlight javascript %}
var Tree = function(years, height) {
  this.centimeters = height,
  this.age = years,
  this.grow = function () {
    this.age++;
    this.centimeters += 5;
  }
}

var maple = new Tree(2, 130);
var magnolia = new Tree(10, 400);
console.log(maple) // {centimeters: 130, age: 2}
console.log(magnolia) // {centimeters: 400, age: 10}
{% endhighlight %}

As you can see, *this* in each new `Tree` object is unique to the new instance of tree, not to the constructor function. As expected if I call `grow` on each of these `Tree` instances, the *this* in the `grow` method will apply the changes only to its immediate tree instance.

{% highlight javascript %}
maple.grow();
console.log(maple) // changed to {centimeters: 135, age: 3}
console.log(magnolia) // unchanged {centimeters: 400, age: 10}
{% endhighlight %}

### Special Cases
#### strict-mode
If you are using strict mode, your *this* will NOT recursively move up the scope context to find it’s context. If nothing is explicitly set, it will default to *undefined*. For example, calling the following will return undefined.

{% highlight javascript %}
function thisRetriever () {
  ‘use strict’
  return this;
}
{% endhighlight %}

#### ES6
ES6 introduces many new features which I have explored in other posts; however, it’s worth discussing here arrow functions as relates to *this*. If you’re not familiar with arrow functions, check out [my ES6 blogpost](https://dianna.github.io/ES6-How-and-When-to-Use-Arrow-Functions/) before tackling these! Here’s a simple arrow function:

{% highlight javascript %}
const returnThis = () => this;
{% endhighlight %}

Upon definition, the context is set. In this case, that’s the global scope. No matter what context I now subject this function to, it will always keep the `window` context. This is because *this* context is applied lexically in arrow functions.

{% highlight javascript %}
var shinyNewObj = {returnThis: returnThis};
shinyNewObj.returnThis() // returns window

returnThis.call(shinyNewObj); // returns window

returnThis.bind(shinyNewObj); // returns window
{% endhighlight %}
