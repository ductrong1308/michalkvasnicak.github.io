---
layout: post
title: Lazy loading images using AngularJS
tags:
    - angularjs
    - javascript
---

## The Problem


In one of my projects *(using AngularJS)* I needed some sort of image lazy load because a page was heavy on photos.
However, I didn't want to bother users with loading of images they won't see *(because the page was optimized for mobile
devices primary using 3G connections)*.


## The naive solution

First I tried to write a really naive solution for this kind of problem. It was easy,
just create a directive that will listen to document scroll and resize events and if an image is in the viewport load it.


{% gist michalkvasnicak/71101854cd9d6fbb7cf2 angular-naive-image-lazy-load.js %}

[Demo page]({{ site.url }}/examples/2014-08-04-lazy-loading-images-using-angularjs/naive.html)

The problem is that we are polluting the `document` and the `window` with event listeners *(it can cause
performance problems on low-performance devices)*. Another problem is that there is no scroll/resize stop event,
so the scroll/resize listener is fired continuosly until you stop resizing/scrolling.


## The better but not the best solution

There are multiple ways how to solve this efficiently. Here is one of simplest ones.

In this solution I am using service which acts as a registry for listeners and invokes all listeners when the scroll/resize
 event is fired. To invoke registered listeners only once we are simulating the "scroll/resize stop" event using
 the `timeout` so listeners are called when user stops scrolling/resizing the window.


{% gist michalkvasnicak/23a6867403f55ab46452 angular-better-image-lazy-load.js %}

[Demo page]({{ site.url }}/examples/2014-08-04-lazy-loading-images-using-angularjs/better.html)

Of course there are much better and efficient ways to solve this *(e.g. you can pre-calculate an elementOffset, use
the scrollTop/scrollLeft with the clientHeight/clientWidth to determine a visibility and register only positions to the listener
service)*. Maybe I will write one in a future post. :)