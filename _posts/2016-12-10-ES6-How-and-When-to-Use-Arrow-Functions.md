---
layout: post
title: "ES6: How and When to Use Arrow Functions"
published: true
---
  
Arrow functions have been cited as one of developers' [favorite new features](http://www.2ality.com/2015/07/favorite-es6-features.html) in ES6. There are two major pros we gain from this new syntax. First, in many common function use cases \(essentially whenever you could use an anonymous function\), our code can be written much more concisely. Second, this picks up its context lexically, which is a boon to developers who still get confused about when and how to use *bind* \(buuuut, you’re still going to need *bind* folks, just not as often\). Because of that second use case however, this new syntax cannot be used as a constructor function and usually should not be used as a method. I’ll go through some examples to show you where arrow functions can save you time and where you should stick to the old function syntax.

### How to use an arrow function
There are two ways to use this syntax. They’re called concise and block syntax. Since block syntax is a bit more familiar, let’s start there. 

{% highlight javascript %}
//ES5
var exclame = function(phrase, excitementLevel) {
  while (excitementLevel) {
    phrase += '!';
    excitementLevel--;
  }
  return phrase;
}

//ES6
const exclame = (phrase, excitementLevel) => {
  while (excitementLevel) {
    phrase += '!';
    excitementLevel--;
  }
  return phrase;
}
{% endhighlight %}

All we needed to do was remove the word *function* and add the fat arrow *“=\>”* between the arguments and function body. Concise syntax is possible when there is up to 1 argument. Using this syntax below for the native method map, I’m able to write a single, clear line of code.

{% highlight javascript %}
const people = [
  {first: 'Margaret', last: 'Hamilton'},
  {first: 'Grace', last: 'Hopper'}
];

// Create array of full names
//ES5
const fullNames = people.map(function(person) {
  return person.first + ' ' + person.last;
});

//ES6
// variation 1
const fullNames1 = people.map(person => `${person.first} ${person.last}`);

// variation 2
const fullNames2 = people.map((person) => `${person.first} ${person.last}`);
{% endhighlight %}

We were able to remove the parentheses from around the argument *person* as well as the brackets around the function body. You can choose to keep both the parentheses and the brackets if you wish. Note that if there are no arguments, a pair of empty parentheses must be included. Some developers prefer to leave the argument parentheses, like in variation 2. When the brackets are removed from the function body, the return is implicit so you do not need to write *return* at all. If you’re unfamiliar with the backtick string syntax I used to set fullName, it’s pretty easy to pick up and you can [check that out here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

### Particulars on common use cases
If you want to implicitly return an object literal, there’s an extra step to prevent the bracket from being interpreted as the start of the function block&emdash;adding parentheses around the object.

{% highlight javascript %}
const lottoPrizes = ['Trip to Bali', 'Trip to NYC', 'Trip to Oklahoma City'];
const shuffledNumbers = [54, 23, 95, 55, 9, 21];

const winResults = shuffledNumbers.map((number, i) => ({drawOrder: i+1, luckyNumber: number,  winner: !!lottoPrizes[i], prize: lottoPrizes[i] || 'nothing'}));
{% endhighlight %}

Implicit returns work with conditionals, such as this filter example:

{% highlight javascript %}
const physicsMidterms = [
  {grade: 95, first: 'Bill', last: 'Nye'},
  {grade: 98, first: 'Neil', last: 'deGrasse Tyson'},
  {grade: 32, first: 'Myron', last: 'Ebell'}
];

const excellingStudents = physicsMidterms.filter(student => student.grade > 90);
{% endhighlight %}

There are some other cases, but in lieu of repeating work that’s been done, I’ll send you along to [MDN's documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) if you’re interested in an exhaustive understanding of arrow functions.

### When not to use an arrow function
- You need stack trace: arrow functions are anonymous
- Constructor functions: invocation using new will error
- Most methods: this is applied lexically, so parent object cannot be referenced or modified
- ES6 Generators: cannot use yield in arrow functions

### Helpful further reading:
About use: <http://exploringjs.com/es6/ch_arrow-functions.html>
