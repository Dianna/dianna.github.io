---
layout: post
title: "Simple Intro to ES6 Classes"
published: true
---

ES6 classes are not too much of a leap from [what you’re already familiar with](http://javascriptissexy.com/oop-in-javascript-what-you-need-to-know/). They just make life easier through syntactic sugar. When I switched my development back from Angular to React, this time with ES6, I could immediately see how much clearer and succinct my code was—not to mention the helpful exceptions thrown whenever I tried to use it incorrectly.

### ES6 Class Advantages
- Throws exception automatically if not used correctly (i.e. no *this* context because *new* wasn’t used or *super* wasn’t called. More on that coming up)
- Cleaner code, lower boiler plate through streamlined class creation syntax and ability to extend native objects
- Separation of concerns. Can create *static* functions that are only accessible from the class, not an instance

### Declaration vs. Expression
To get started I’d like to quickly introduce how an ES6 class is created. Like ES5, classes can be defined as an expression or declaration.

{% highlight javascript %}
// Class Declaration
class Boat {}

// Unnamed Class Expression
const Boat = class {}

// Named Class Expression
const Boat = class BoatClass {}
{% endhighlight %}

So what do we need to know about these different ways to define a class? First it’s worth noting that unlike normal function declarations class declarations are *not* hoisted, so make sure they are declared before you try to access it. Other than that, they behave like other function declarations. As for the expressions, naming or not naming the function has the [same advantages as a function expression](https://kangax.github.io/nfe/). Essentially, it allows you to directly reference the function *only within its scope*. If you’re not sure which to use, start with a declaration.

### Creating a base class
The *constructor* function is used to define properties during instantiation. It isn’t required when creating a base class, but you can’t really accomplish much without it. Here’s an example:

{% highlight javascript %}
class Boat {
  constructor(seaworthy, energySource, name) {
    this.seaworthy = seaworthy;
    this.energySource = energySource;
    this.name = name;
  }
  static describe() {
    console.log('Boat is a base data type for all water-faring vessels');
  }
}

Boat.describe(); // Boat is a base data type for all water-faring vessels
const kayak = new Boat(false, 'human', 'Swell Time'); // creates a new Boat instance
kayak.describe(); // Error: kayak.describe is not a function
{% endhighlight %}

ES6 also introduces static methods, which exist on the constructor method itself, rather than an instance of the class.

### Creating a subclass
One of the more exciting developments in ES6 classes is *extends*. This allows you to create a subclass of an existing class. What’s so awesome about that is you can easily create your own subclass using a native standard object such as Array, Object or Math as the base class. Your subclass will then inherit all the built in prototype methods from the base class. That means you can easily create custom collections from Array with specialized getters, setters and sorts for the type of data you’re expecting, or perhaps add specialized methods to Math. We’ll get into that in my next post. Let’s start simple by extending our *Boat* class.

#### Using *extends* and *super*
Just like an ES5 subclass, an ES6 class will inherit any properties or methods of the parent. I’ll show you the code first and then unpack what’s happening here.

{% highlight javascript %}
class Dinghy extends Boat {
    constructor (energySource, name, inflatable) {
    super(false, energySource, name);
    this.size = 'tiny';
    this.inflatable = inflatable;
  }
}

const lakeBoat = new Dinghy('wind', 'Sand Witch', true);
{% endhighlight %}

The *extends* keyword simply derives the class *Dinghy* from the base class *Boat*. Seems sensible enough, but what is that super function doing? Remember how the *constructor* method defines properties during instantiation of a base class? When creating a subclass, an instance of the base class must first be instantiated before adding further properties to it, otherwise you’ll be thrown a reference error because *this* is not defined. The function *super* instantiates that object. Note that I’m setting *seaworthy* to *false* automatically for all Dinghy instances upon calling *super*. When creating *lakeBoat* I simply define the *energySource* and *name* properties. The new property *size* = *‘tiny’* is common to all Dinghy instances.

Neither the *constructor* nor *super* function are required when deriving a class; however, the *constructor* function is necessary to add further properties and a call to *super* is required if you need to reference the base class. So if you’re just adding further prototype methods, you can skip that step. Wait, we haven’t gone over those.

### Creating and Extending Prototype Methods
Adding a prototype method to the Boat class after it’s defined is the same in ES6 as ES5; however, if you wanted to define a method as you’re declaring the class, that would look like this:

{% highlight javascript %}
class Boat {
  constructor(seaworthy, energySource, name) {
    this.seaworthy = seaworthy;
    this.energySource = energySource;
    this.name = name;
  }
  // Prototype method below
  scuttle() {
    this.sunk = true;
    console.log(`${this.name} be at the bottom of the deep blue ${this.seaworthy ? 'sea' : 'lake'}`);
  }
}

lakeBoat.scuttle(); // Sand Witch be at the bottom of the deep blue lake
lakeBoat.sunk === true; // true
{% endhighlight %}

Note that there is no comma after constructor, which would cause a syntax error. We can define prototype methods on derived classes in the same way. In some cases we may wish to call a base method from a subclass’s prototype method. To do this, we need to have called *super*. For example:

{% highlight javascript %}
class Dinghy extends Boat {
  constructor (energySource, name, inflatable) {
    super(false, energySource, name);
    this.size = 'tiny';
    this.inflatable = inflatable;
  }
  encounterSharkSwarm() {
    this.deflated = true;
    super.sink();
  }
}

const lakeBoat = new Dinghy('wind', 'Sand Witch', true);

lakeBoat.encounterSharkSwarm();  // Sand Witch be at the bottom of the deep blue lake
lakeBoat.deflated === true  // true
{% endhighlight %}

### Conclusion
That wraps up our gentle yet shark infested intro to ES6 classes. As we embrace this new, cleaner syntax, let’s take a final look at what we’re leaving behind.

{% highlight javascript %}
// ES5-ified
var Boat = function(seaworthy, energySource, name) {
  if (!(this instanceof Boat)) {
    throw new Error("Boat is a constructor function, you must use new");
  }
  this.seaworthy = seaworthy;
  this.energySource = energySource;
  this.name = name;
};

Boat.describe = function () {
  console.log('Boat is a base data type for all water-faring vessels');
};

Boat.prototype.sink = function() {
  this.sunk = true;
  console.log(this.name, 'be at the bottom of the deep blue ', this.seaworthy ? 'sea' : 'lake');
};

var Dinghy = function(energySource, name, inflatable) {
  if (!(this instanceof Dinghy)) {
      throw new Error("Dinghy is a constructor function, you must use new");
  }
  Boat.call(this, false, energySource, name);
  this.size = 'tiny';
  this.inflatable = inflatable;
}

Dinghy.prototype = Object.create(Boat.prototype);
Dinghy.prototype.constructor = Boat;
Dinghy.prototype.encounterSharkSwarm = function() {
  this.deflated = true;
  this.sink();
};
{% endhighlight %}
