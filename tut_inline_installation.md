---
layout: default-withsidebar
title: Inline Installation of Extensions
author: shwetankdixit
copyright: opera-ccby
---

## Introduction

Usually, the way users install extensions is by going to the [Opera Addons Store](http://addons.opera.com) and selecting an extension there. However, at times, you might want a way for a peson to click on a link on your own web page, and install it without having to go the addons store. This process is known as 'inline installation' of extensions.

![image](static/images/inline_install.png)

This is especially useful for extension authors who have a dedicated webpage or site for the extension, where they would like the peple visiting that page to install it without having to leave their site. With 'inline installation' of extensions, you can have the browser prompt the user to install the extension from the current web page itself. Keep in mind that the extension is still served and installed from the [Opera Addons Store](http://addons.opera.com), but the user will not be required to go there, and can install the extension from the prompt right from your web page.

## What to add in your webpage

You will need to use the [`opr.addons.installExtension()`](addons.html#method-installextension) function and add it to your page. Its first parameter is the `id` of the extension, followed by an optional success callback and an error callback.

Let's see some sample code of how it would be like to add inline installation in a webpage.

Assuming a link with the `id` as 'myextension'.

<pre class="prettyprint">&lt;button id=&quot;installext&quot;&gt;Click here to add my extension&lt;/button&gt;.</pre>

We would add the following peice of JavaScript.

<pre class="prettyprint">var id = &quot;samplextensionid&quot;;

var myExtension = document.querySelector(&quot;#installext&quot;);

myExtension.addEventListener(&#39;click&#39;, function(event) {
    opr.addons.installExtension(id, function () { console.log(&#39;success&#39;);}, function (errorMessage) {console.log(&#39;Error: &#39;+errorMessage)});
}, true);</pre>

When the person clicks on the button, it calls `opr.addons.installExtension()`. The first parameter will take the 'id' of the extension and call the installation dialog box, from which the user can see details of the extension (Its name, icon and a short summary of the permissions it wants access to). The user can then either cancel the installation request or go on to install it.

## Some things to keep in mind

There are a few things to keep in mind when using `opr.addons.installExtensio()`. The first is that it can only be used in the top frame (that is, not through an iFrame or such). Secondly, at most only one installation may run at a given time, and it can only be used in response to a user gesture (for example, a mouse click). So it is recommended that you implement this functionality in conjuction with a user interface element which makes it clear what the gesture is supposed to be (for example for using a button if using the mouse click event).

One more thing to keep in mind is that you can call for the inline installation of any opera extension from your webpage.

So go on and add inline installation of Opera extensions to your web pages. It's a great way to get more people to install extensions and it is very simple to implement!



