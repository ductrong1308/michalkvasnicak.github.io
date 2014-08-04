---
layout: post
title: Lazy loading images using AngularJS
---

## The Problem


In one of my projects *(using AngularJS)* I needed some sort of image lazy load because page was heavy on photos.
However, I didn't want to bother users with loading of images they won't see *(because page was optimized for mobile
devices and primary using 3G connections)*.


## Naive solution

First I tried to write really naive solution for this kind of problem. It was easy,
just create directive that will listen on document scroll and resize events and if image is in viewport load it.


{% gist michalkvasnicak/71101854cd9d6fbb7cf2 angular-naive-image-lazy-load.js %}

[Demo page]({{ site.url }}/examples/2014-08-04-lazy-loading-images-using-angularjs/naive.html)

Problem is that we are polluting `document` and `window` with event listeners *(it can cause
performance problems on low-performance devices)*. Another problem is that there is no scroll/resize stop event,
so scroll/resize listener is fired continuosly until you stop resizing/scrolling.


## Better but not the best solution

There are multiple ways how to solve this efficiently. Here is one of simplest ones.

In this solution I am using service which acts as a registry for listeners and invokes all listeners when scroll/resize
 event is fired. To invoke registered listeners only once we are simulating "scroll/resize stop" event using
 `timeout` so listeners are called when user stops
scrolling/resizing window.


{% gist michalkvasnicak/23a6867403f55ab46452 angular-better-image-lazy-load.js %}

[Demo page]({{ site.url }}/examples/2014-08-04-lazy-loading-images-using-angularjs/better.html)

Of course there are much better and efficient ways to solve this *(for example you can pre-calculate elementOffset, use
scrollTop/scrollLeft with clientHeight/clientWidth to determine visibility and register only positions to listener
service)*. Maybe I will write one in future post. :)