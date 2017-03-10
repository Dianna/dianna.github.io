---
layout: post
title: "Targetting Dynamic Elements"
published: true
---

The methods *call*, *apply*, and *bind* are very powerful, yet underused, native Javascript methods. This post will walk you through an exercise that utilizes call and bind methods to dynamically create and modify elements. I've provided links to the exercises and solutions, but have included the same code in the post for those simply browsing. Before we start, let’s review what these methods share and how they differ.

#### Prerequisites
Javascript closure (recommended resource at end of post)
Basic jQuery (Google is your friend)

### Review
#### Call and Apply
These two methods are nearly identical. Both are used to immediately invoke a function and both set the *this* context using what is given as the first argument. The difference is what follows the *this* argument. *call* accepts an argument list while *apply* accepts a single argument array.

#### Bind
The power of *bind* is that it allows delayed invocation, but with the explicitly stated *this* context like we saw with *call* and *apply*. Like *call*, it takes an argument list. There are many ways to use the delayed invocation and argument list with *bind* to your advantage. You may also have seen some of the excellent (if long) blog posts on the four major ways to use it (links to those at the end of this post). While these are phenomenal resources, I think the easiest way to learn is in small bites, tackling a minimum of concepts to achieve complete understanding. In this exercise, we’ll simply be using *bind* to set the this context without invoking the function.

### Exercise
#### Part 1:
To get started we’re going to create a button that adds more buttons to the page. Each button will show a number indicating when it was created (i.e. 1st button shows “1”, 2nd shows “2”, etc…). Make a solid attempt to [solve this](https://jsfiddle.net/bananabanananana/c1zxn82e/1/) then return for a solution and explanation. JQuery is not required, but helpful and my solution will use it.

```html
<!-- HTML -->
<div class="button-container">
  <button class="button-maker">Make button</button>
</div>
```

{% highlight javascript %}
//JS
var buttonFactory = {
  numberOfButtons: 0,
  makeButton: function () {
    this.numberOfButtons++;
    $('.button-container').append('<button class="baby-button">I\'m button ' + this.numberOfButtons + '!</button>');
  }
};

// When "Make Button" is clicked, execute buttonFactory's makeButton method. Ensure that the numberOfButtons property updates.
// Your code here

{% endhighlight %}

```css
button {
  margin: 10px;
  width: 90px;
  padding: 5px;
  border: 1px solid gray;
}

.baby-button {
  background-color: #000;
  color: #fff;
}
```

[Solution](https://jsfiddle.net/bananabanananana/poxac4pe/2/) with explanation.

#### Part 2:
Now that we’ve created a button making machine, we’ll make those new buttons do something. Your goal is to make each button—only the clicked button—change it’s color. Try that out [here](https://jsfiddle.net/bananabanananana/nem3smtj/1/).

```html
<!-- HTML -->
<div class="button-container">
  <button class="button-maker">Make button</button>
</div>
```

{% highlight javascript %}
//JS
var buttonFactory = {
  numberOfButtons: 0,
  makeButton: function () {
    this.numberOfButtons++;
    $('.button-container').append('<button class="baby-button">I\'m button ' + this.numberOfButtons + '!</button>');
  },
  changeColor: function () {
    var color = randomColor();
    $(this).css('background-color', color);
  }
};

$('.button-maker').on('click', buttonFactory.makeButton.bind(buttonFactory));

// When a button created by "Make Button" is clicked, execute the buttonFactory's changeColor method. ONLY that button's color should change
// Your code here



// Helper function
function randomColor () {
  var colors = ['red', 'orange', 'yellow', 'green', 'blue', 'purple'];
  var randomIndex = Math.floor(Math.random() * 6);
  return colors[randomIndex];
}

{% endhighlight %}

```css
button {
  margin: 10px;
  width: 90px;
  padding: 5px;
  border: 1px solid gray;
}

.baby-button {
  background-color: #000;
  color: #fff;
}
```

[Finished Code](https://jsfiddle.net/bananabanananana/6bbhvao1/4/) with explanations.

Hopefully this gives you some insight into how to create and manipulate elements using JS. If you want to really remember these concepts I’d recommend adding a couple more methods to the code. Some ideas: create a reset button, make all the colors change every n-seconds, make all orange buttons go to the end of the button list.

### Resources
- [Javascript Closure](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
- [In depth look at *apply*, *call* and *bind*](http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals/)
- MDN documentation for [*call*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), [*apply*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) and [*bind*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

