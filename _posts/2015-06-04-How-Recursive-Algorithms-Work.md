---
layout: post
title: "The Fibonacci Sequence- Understanding recursive algorithms"
published: true
---
## When would recursion be useful?
At its core, what makes a recursive function a recursive function is that it will, 
at least in some cases, call itself with new information to be processed. This structure 
shines when working with tree-data structures, permutations or repetitive mathematical 
processes because the structure of the data being searched is indefinite (as in the first 
two cases) or more generally the amount of times the computation must occur is indefinite 
(as in all three cases). That is, we don’t know ahead of time how many children a node in 
a tree may have / the conditions under which we’ll be finding permutations / how many times 
we must perform a mundane calculation. The goal of this function can be threefold- updating 
local information that is eventually returned, causing a side effect (e.g. console-logging), 
or both. The goal of this post is to examine the inner workings of a recursive function.

## Our candidate: the Fibonacci sequence
Let’s take a look at the Fibonacci sequence in which a number in the sequence is equal to 
the sam of the two values before it. For this exercise we’ll try to find the value of a 
fibonacci number at a random place in the sequence. If we input 0 we would expect to receive 
the value at that index (0) and if we input 5 we would expect to receive 8. This 
fantastically mundane task of indefinitely long addition is a perfect candidate for 
recursion.

### Here’s the start of the sequence for your reference: 0,1,1,2,3,5,8,13

Here’s our skeleton code:
{% highlight javascript %}
// Our function takes an index of the number
var findFibonacci = function(index){
  return value; // it returns the value of the number at that index in the sequence
};
{% endhighlight %}
Ok, we’ve got the general idea for our input and output, but what goes in between? This is 
a good time to consider edge cases. Since the fibonacci sequence is created by adding the 
last two numbers calculated to get the next number, we must have two numbers to start with. 
These will be 0 and 1 respectively. To keep this code simple, we’ll assume our user isn’t 
evil, and won’t input a negative number:
{% highlight javascript %}
var findFibonacci = function(index){  
  if ( index < 2 ){               // Edge cases: This condition catches indexes 0 and 1  
    return index;                // We return index because the value happens to be the same  
  }  
  return value;  
}  
{% endhighlight %}
Solid! But our code isn’t really doing anything yet. Let’s imagine how the Fibonacci sequence 
works and construct our recursive function from there:

Imagine our user inputs 4. The output should be 3 (the sum of the two previous values). Lets 
abstract those values to their indexes, where n = the index of 3 in the sequence and the 
numbers bellow correspond to the values and those indeces:
{% highlight javascript %}
// Input is 4
// idx2        idx3     idx4
( n - 2 ) + ( n - 1 ) =  n
    1     +     2     =  3
{% endhighlight %}  
We can do this process for both 1 and 2 as well:
{% highlight javascript %}
// Input is 1:                          Input is 2:
// idx0        idx1     idx2               idx1       idx2      idx3
( n - 2 ) + ( n - 1 ) =  n              ( n - 2 ) + ( n - 1 ) =  n
    0     +     1     =  1                  1     +     1     =  2
{% endhighlight %}
You may notice that if we add everything on the left side of those two equations, we still 
get 3:
{% highlight javascript %}
0 + 1 + 1 + 1 = 3
{% endhighlight %}
Lets use this knowledge to our advantage. To find the value at the input index, we must 
calculate all the values that added together to create the two before it and the two that 
added together to make those two values and so forth. In the end, we can imagine this as 
finding the path of 0s and 1s that lead to this value. Don’t worry if that seems obscure, 
it’s a higher level view of what our code will be accomplishing. For now I’ll add this 
concept to our code and we’ll break it down afterwards into a series of computations:
{% highlight javascript %}
var findFibonacci = function(index){
  if ( index < 2 ){
    return index;
  }
  return findFibonacci(index - 2) + findFibonacci(index - 1);
}
{% endhighlight %}
Let’s imagine our previous scenario of an input 4, expecting 3. We would call the 
findFibonacci on the indexes 3 and 2:
{% highlight javascript %}
We call findFibonacci(4) —>
Stack 1-  findFibonacci(4) returns findFibonacci(3) + findFibonacci(2)
{% endhighlight %}
Each of these function calls would return functions as well:
(Note: The number of the stack symbolizes how far into our recursive function we’ve delved 
in finding our answer. The colors match their return values deeper in the stack.)
{% highlight javascript %}
Stack 2-  findFibonacci(3) returns findFibonacci(2) + findFibonacci(1)
Stack 2-  findFibonacci(2) returns findFibonacci(1) + findFibonacci(0)
{% endhighlight %}
Our function calls that have 1 or 0 passed will return that value:
{% highlight javascript %}
Stack 3-  findFibonacci(2) returns findFibonacci(1) + findFibonacci(0)
Stack 3-  findFibonacci(1) returns 1
Stack 3-  findFibonacci(1) returns 1
Stack 3-  findFibonacci(0) returns 0
{% endhighlight %}
Those final calls will return their respective values:
{% highlight javascript %}
Stack 4-  findFibonacci(1) returns 1
Stack 4-  findFibonacci(0) returns 0
{% endhighlight %}
The value that is found at any of these stacks is added to its pair and then returned 
(passed back up the stack to the function that originally called it):
{% highlight javascript %}
From Stack 4 we return 1 and 0 to the findFibonacci(2) that called it which adds them to 1

From Stack 3 we return 1  and 0 to the findFibonacci(2) that called it which adds them to 1
             we return 1 and the 1 from stack 4 to the findFibonacci(3) that called it & adds to 2
{% endhighlight %}
From Stack 2 we return the added values from stack 3 ( 1 and 2 ) 
which findFibonacci(4) at Stack 1 evaluates to our final output, 3 and returns. 
Success!
