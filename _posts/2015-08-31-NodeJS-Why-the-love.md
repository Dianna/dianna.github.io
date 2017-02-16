---
layout: post
title: "Node.js: Why the love?"
published: true
---
### First: What is Node.js?   
Simply put, Node.js is a runtime environment often used as a server (point: not a framework!) that runs JavaScript server-side. That being said, Node's v8 JavaScript Runtime Engine and wrappers which provide added functionality are written in C which makes it incredibly fast. Besides running JavaScript, what sets Node.js apart from other servers (e.g. .NET, PHP or Java) is that it has a single "thread" which is reffered to as the "event loop". This loop can constantly accept requests and doesn't "fill up" in the way that traditional servers enlist all of their threads.  

### What's happening under the hood  
The way Node handles an indefinite number of requests with a single event loop is by passing off these requests into callbacks so it can constantly receive more requests. The event loop is entered automatically at the first request and exited when no callbacks are left in the loop, similar to how JavaScript is handled by the browser- this is what the Node's claim to being "event driven" means. When a callback has a result this is returned to the event loop which will either send it off for further processing or return it to the client as a response. As a result, quick requests can be promptly handled while a more complex request is churning in the background- this is what is meant by Node's claim that it's a "non-blocking I/O"- that is, the event loop is not held up by such requests.  
  
### The Node.js philosophy  
To make the above process possible, each function that handles a request must have a callback to execute when processing is complete (or to string together a series of processes). Part of what makes this possible is JavaScript's "closure" support (access to outer scopes' variables). This is important because Node events are executed independently and these functions need to maintain state to be logical in this non-linear (asynchronous) environment. This makes Node a fantastic option for app's that are I/O dependent and intensive (e.g. chat, data-streaming, file uploading); however, Node may not be the best option for incredibly CPU intensive apps as it is difficult to scale beyond a single CPU core.  

An added benefit of using Node and therefore JavaScript server-side is the ability to create isomorphic apps. This allows the developer to use one environment which, combined with treatment of JSON as a first class data structure, can drastically cut latency and facilitate easier debugging.  
  
### Resources
If you're looking for more information, I found the following posts extremely helpful in getting up to speed on Node. I highly recomend Shubhra Kar's [podcast on Node](http://www.se-radio.net/2015/06/episode-230-shubhra-khar-on-nodejs/ "node podcast")  

Further Resources:  
- On how [single-threaded non-blocking IO model](http://stackoverflow.com/questions/14795145/how-the-single-threaded-non-blocking-io-model-works-in-node-js) works  
- Further understanding the [non-blocking IO model](http://stackoverflow.com/questions/18040366/understanding-nodejs-non-blocking-io)  
- Fantastic [blog](http://voidcanvas.com/describing-node-js/) on how Node works, with a list of pros and cons  
