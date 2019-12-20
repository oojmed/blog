---
layout: post
title: "How we made: A Dark Mode + Mode Changer to our chat"
date: "2019-05-21 19:10:47 +0100"
---

Recently we’ve been working on my chat, and we’ve recently added a dark mode, plus a changer. This is how we created it.

<!--more-->

First, you have a small section of HTML which creates two icons, and a container for them.

{% highlight html %}
<div id="modeChangers" style="float: right; padding:5px;">
    <div id="nightMode" onclick="javascript:nightMode();"><i class="fas fa-moon fa-2x" aria-hidden="true"></i></div>
    <div id="lightMode" style="display: none;" onclick="javascript:lightMode();"><i class="fas fa-sun fa-2x" aria-hidden="true"></i></div>
</div>
{% endhighlight %}

Then, some simple JS (JavaScript) which makes hides and shows the appropiate icons and sets an attribute to the body.

{% highlight js %}
function nightMode() {
    document.body.setAttribute("data-theme", "dark");

    document.getElementById("nightMode").style.display = "none";
    document.getElementById("lightMode").style.display = "block";
}

function lightMode() {
    document.body.setAttribute("data-theme", "light");

    document.getElementById("nightMode").style.display = "block";
    document.getElementById("lightMode").style.display = "none";
}
{% endhighlight %}

Finally, the CSS to work with it. When styling every element for each theme, just put a selector for the body before it. An example can be seen below:

{% highlight css %}
body[data-theme="light"] #users {
    background-color: #f1f1f1;
    color: #1f1f1f;
}

body[data-theme="dark"] #users {
    background-color: #2f2f2f;
    color: #ffffff;
}
{% endhighlight %}
