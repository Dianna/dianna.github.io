---
layout: post
title: "React: How it's different and what that means for you"
published: true
---
My past couple of blog posts have highlighted the advantages of JavaScript and isomorphic apps. For this post I'd like to talk about one framework in particular that's taking the internet by storm - ya you know what it is already - React.  

##Get ready for the weirdness  
###Components
React requires that you mix your JavaScript with your HTML (JSX) which it then compiles. There are numerous advantages to this approach. In particular, all of your logic and structure for a particular component is in one place which makes it much easier to see relationships of different code segments and therefore makes it more maintainable. This encourages to write modular reusable code which in turn makes your code more easily testable.  
###Virtual DOM
React takes advantage of this modularity with a unique update cycle. Essentially React keeps a snapshot of the DOM and when it sees a part of it has changed updates *only that part of the DOM*. This is a major advancement for front-end development as it greatly minimizes the amount of updating and therefore increases your app's speed and smoothness of the UX. You still have access to these DOM elements as 'refs'.  
###Component state and props
Each component has it's own state (data that persists on that component) which it can pass to dependent child components as props (inherited data). In addition these components have a type of data/DOM lifecycle. You can react to incoming changes in data of that component or its parent component before, while and/or after those changes happen.  

##How it comes together
As a whole this creates a pleasent UX. Bare bones HTML can be rapidly rendered server-side and then added to by JavaScript as data is retrieved. Though it initially feels a little weird writing JSX, once you get over the shock factor it's actually a very intuitive way to organize your code. In addition, since React is agnostic to the MC part of your MVC app, you can easily incorporate Backbone.js or Flux which can be a nice way to break into the React way of thinking.
