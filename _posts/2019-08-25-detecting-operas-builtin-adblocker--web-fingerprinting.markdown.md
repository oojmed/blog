---
layout: post
title: "Opera: Detecting Opera's built-in adblocker"
date: "2019-08-25 16:50:21 +0100"
category: "Fingerprinting"
---

We're starting a new series of posting, detailing our 'adventure' into web fingerprinting. This series will detail how we have discovered how to figure out what browser or adblocker you are using.

<!--more-->

This first post will be detailing how to detect Opera's built-in adblocker. It's quite easy, as it injects a style tag into the head of webpages. Quite a proprietary, as most of the time it's blocked request-side nowadays. At least with browser extensions.

What you need to do, is detect how many style elements there are. If there are more than normal (most likely more than 0), loop through each and see if it's innerText contains common text in Opera's adblocker.

Proof of concept / example code:
{% highlight js %}
var styles = document.getElementsByTagName('style');

var injection = styles.length > 0;

if (injection) {
  styles = [].slice.call(styles);

  for (var i = 0; i < styles.length; i++) {
    var styleIsOperaAB = styles[i].innerText.includes(':root topadblock');

    if (styleIsOperaAB) { alert('Opera (Adblock) Detected'); break; }
  }
}
{% endhighlight %}

[Proof of Concept](https://oojmed.com/Web-Multiprint/#opera-adblock)
