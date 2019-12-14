---
layout: post
title: "Opera | Exploiting the browser's own pages | Web Fingerprinting"
date: "2019-10-19 10:20:00 +0100"
---

After a very long time, our adventure / series of writeups in web fingerprinting is finally continuing. Today, we will be visiting Opera again, using an 'exploit' we (we believe we're the first ones to find it, but we may be wrong) discovered. The exploit allows fingerprinting / detection of the browser, Opera. This is because Opera has a problem / exploit / vulnerability / flaw, whatever you want to call it. Normally browser's will block pages from making requests to the browser's own pages (eg. `chrome://version/`, `opera://about`). But Opera doesn't.

We can exploit this in a simple process:
1. Create an iframe
2. Make the source `opera://about`
3. Set the iframe's onload event to run function X
4. Add the iframe to the body (or anywhere else in the document / DOM)
5. If function X runs / the iframe is loaded, the browser is Opera

Proof of concept / example code:
{% highlight js %}
var iframe = document.createElement('iframe');
iframe.src = 'opera://about';
iframe.onload = isOpera;
document.body.appendChild(iframe);

function isOpera() {
  alert('Opera detected');
}
{% endhighlight %}

Note:
We have contacted Opera on the issue several months ago and we have not received a reply.

Tested on the latest version of Opera as the time of writing this:


[Proof Of Concept](https://oojmed.com/Web-Multiprint/#opera-local-exploit)
