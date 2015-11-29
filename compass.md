---
layout: post
title: Compass
permalink: /compass/
---

<img src="/assets/work/compass.png">


# Everything you need to know about your commute on a screen

I built out compass as a side project while at Brave UX. It's a realtime system that gathers data from a bunch of different API's like Uber and Weatherundground.

Compass is a three-part system. At the heart, it's a decoupled front and back end built on top of node, redis, socket.io, and angular.js. A raspberry pi with a stripped down version of rasbian then runs chromium to open up the information.

Compass is deployed in all of the [Uber Offices](http://uberoffices.com) and a couple other locations throughout DC.
