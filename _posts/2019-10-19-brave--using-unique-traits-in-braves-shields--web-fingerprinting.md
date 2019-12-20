---
layout: post
title: "Brave: Using unique traits in Brave's 'Shields'"
date: "2019-10-19 10:30:00 +0100"
category: "Fingerprinting"
---

We knew Brave would be hard to fingerprint, as they are [actively against browser fingerprinting](https://github.com/brave/brave-browser/wiki/Fingerprinting-Protection-Mode). However, when we discovered that's Brave's 'Shields', their built-in adblocker has a [unique list of things to block, not block, etc.](https://github.com/brave/adblock-lists), specifically we looked at their [unbreaking list](https://github.com/brave/adblock-lists/blob/master/brave-unbreak.txt). I thought about how we could exploit this.

<!--more-->

First, we gathered some URLs which could be requested via a third party. We

{% highlight js %}
var bUrls = [
  'https://track.atom-data.io/test.js',
  'https://vidazoo.com/aggregate',
  'https://vidazoo.com/proxy',
  'https://mediabong.net/test.js',
  'https://imprvdosrv.com/test.js'
];
{% endhighlight %}

And then I create an array of functions (probably a bad implementation):

{% highlight js %}
var bFuncs = [
  bTestLoad1,
  bTestError1,
  bTestLoad2,
  bTestError2,
  bTestLoad3,
  bTestError3,
  bTestLoad4,
  bTestError4,
  bTestLoad5,
  bTestError5
];

function bTestLoad1() {
  bTestLoad(0);
}
function bTestError1() {
  bTestError(0);
}

function bTestLoad2() {
  bTestLoad(1);
}
function bTestError2() {
  bTestError(1);
}

function bTestLoad3() {
  bTestLoad(2);
}
function bTestError3() {
  bTestError(2);
}

function bTestLoad4() {
  bTestLoad(3);
}
function bTestError4() {
  bTestError(3);
}

function bTestLoad5() {
  bTestLoad(4);
}
function bTestError5() {
  bTestError(4);
}

function bTestLoad(i) {
  var url = bUrls[i];

  window.bABCount++;

  console.log('<span style="color: lightgreen;">Script ' + (i + 1) + ' <span class="var">(' + url + ')</span> loaded successfully</span>', window.bABCount, bUrls.length);

  if (window.bABCount === 5) { bTestFinal(); }
}

function bTestError(i) {
  window.bABAll = false;

  var url = bUrls[i];

  window.bABCount++;

  console.log('<span style="color: red;">Script ' + (i + 1) + ' <span class="var">(' + url + ')</span> failed to load</span>', window.bABCount, bUrls.length);

  if (window.bABCount === 5) { bTestFinal(); }
}

function bTestFinal() {
  var output = window.bABAll ? '<span style="color: lightgreen;">Brave (Shields) detected</span>' : '<span style="color: red;">Brave (Shields) not detected</span>';
  console.log(output);
}
{% endhighlight %}

And then creating a loop to run these functions (also setting some global variables to track progress):
{% highlight js %}
window.bABCount = 0;
window.bABAll = true;

for (var i = 0; i < bUrls.length; i++) {
  console.log('Injecting script tag <span class="var">(' + bUrls[i] + ')</span>', i + 1, bUrls.length);

  var script = document.createElement('script');
  script.type = 'text/javascript';
  script.src = bUrls[i];
  script.onload = bFuncs[i * 2];
  script.onerror = bFuncs[(i * 2) + 1];
  document.body.appendChild(script);
}
{% endhighlight %}

That's it, run this and check the console for output.

[Proof Of Concept / In Action](https://oojmed.com/Web-Multiprint/#brave-shields)
