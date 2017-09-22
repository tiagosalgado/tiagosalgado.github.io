---
id: 1238
title: Add Twitter Bootstrap template to ASP.NET MVC Project
date: 2012-08-13T14:14:55+00:00
author: tiagosalgado
layout: post
categories:
  - asp.net
tags:
  - asp.net mvc
---
So, what is Twitter Bootstrap?

> Twitter Bootstrap is a free collection of tools for creating websites and web applications. It contains HTML and CSS-based design templates for typography, forms, buttons, charts, navigation and other interface components, as well as optional JavaScript extensions.

<a href="http://en.wikipedia.org/wiki/Twitter_Bootstrap" target="_blank">From Wikipedia</a>

To apply this template on a ASP.NET MVC Project, on an easy way, we need to follow this steps:

  1. Download MVC 4 TwitterBootstrap Starter Layout Page extension for Visual Studio and install it
  2. Open Visual Studio
  3. Create new ASP.NET MVC Project
  4. Add Nuget package <a href="http://nuget.org/packages/Twitter.Bootstrap" target="_blank">Twitter.Bootstrap</a>
  5. Go to Views > Shared and Add new item … and choose “MVC 4 TwitterBootstrap Starter Layout Page (Razor)” item
  6. Finally, go to “\_ViewStart.cshtml” file and change the Layout page to “\_BootstrapLayout.cshtml”

<pre class="brush: csharp; title: ; notranslate" title="">@{    
    Layout = "~/Views/Shared/_BootstrapLayout.cshtml";     
}
</pre>

You can find some themes to apply in <a href="http://bootswatch.com/" target="_blank">Bootswatch</a> (free) and <a href="https://wrapbootstrap.com/" target="_blank">Wrapbootstrap</a>.