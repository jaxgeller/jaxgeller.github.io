---
layout: post
title: Counter.js
permalink: /projects/counter.js/
---

Counter.js is a slim (less than 1kb gzipped), performant, es6 module, that counts numbers.
It uses RAF and easing functions to provide a buttery-smooth, 60-fps counter.

<h3 style="margin-top: 50px;">Demo</h3>

<style>
  .counter-demo { overflow: hidden; margin-top: -50px;margin-bottom: 100px; } .counter-text { margin-top: 50px; }
  .left {
    float: left;
    margin-right: 40px;
  }
  .right {
    float: left;
  }
</style>
<div class="counter-demo">
  <div class="left">
    <h4 id="text-up" class="counter-text">0</h4>
    <button id="up" onclick="up();">Up</button>
  </div>
  <div class="right">
    <h4 id="text-down" class="counter-text">1000</h4>
    <button id="down" onclick="down();">Down</button>
  </div>
</div>

<script src="/assets/projects/counter.js"></script>>

<script>
function up() {
  var opts = {
    start: 0,
    end: 1000,
    duration: 3000,
    selector: document.getElementById('text-up'),
    done: function(){alert('supports callbacks!')}
  }
  var c = new Counter(opts);
  c.run();
}
function down() {
  var opts = {
    start: 1000,
    end: 0,
    duration: 3000,
    selector: document.getElementById('text-down'),
    done: function(){}
  }
  var c = new Counter(opts);
  c.run();
}
</script>



### Documentation and Source

[See it on github](https://github.com/jaxgeller/counter.js)
