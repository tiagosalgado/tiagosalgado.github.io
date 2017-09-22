---
id: 1378
title: Alert users when they have lost internet connectivity with Offline.js
date: 2013-11-01T18:00:31+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - javascript
---
To alert users when they&#8217;ve lost internet connectivity, we can use a really tiny library (only 3kb) called <a href="http://github.hubspot.com/offline/" target="_blank">Offline.js</a>.

So, what is Offline.js?

> Offline.js is a library to automatically alert your users when they&#8217;ve lost internet connectivity, like Gmail.
  
> It captures AJAX requests which were made while the connection was down, and remakes them when it&#8217;s back up, so your app reacts perfectly.

It is really straightforward to add implement this on our web applications.

1. Download <a href="https://raw.github.com/HubSpot/offline/master/js/offline.js" target="_blank">Offline.js</a>
  
2. Download a theme. For that, choose yours <a href="http://github.hubspot.com/offline/docs/welcome/" target="_blank">here</a>.
  
3. Add the JS and CSS files to your pages.
  
4. Add a div to be used to display the notification (see full code for an example)

## <a href="http://jsfiddle.net/TiagoSalgado/qYsAj/embedded/result/" target="_blank">Live Demo</a>

Ahh, and if you want to allow users to play Snake while they don&#8217;t have connection, just change the option &#8220;game&#8221; to true 🙂

Full code:
{% gist tiagosalgado/7266986 %}