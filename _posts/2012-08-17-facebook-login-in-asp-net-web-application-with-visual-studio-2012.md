---
title: Facebook login in ASP.NET web application with Visual Studio 2012
date: 2012-08-17T15:03:11+00:00
author: tiagosalgado
layout: post
categories:
  - asp.net
tags:
  - asp.net mvc
---
One of the new features on Visual Studio 2012, is the ability to integrate a social login (Facebook, Twitter, etc) in our web applications.

In this example, we can see how easy is to add Facebook login to our project.

First, we need to create an App in Facebook Developers page. To do that, go to <a href="http://developers.facebook.com/apps" target="_blank">http://developers.facebook.com/apps</a> and create a new one.

After creating the app, we have something like this:

[<img class="alignnone" title="facebook-create-app" src="http://img502.imageshack.us/img502/9738/facebookappcreate.png" alt="" width="714" height="530" />](http://img502.imageshack.us/img502/9738/facebookappcreate.png)

Now, go to VS2012, create a new project (i choose ASP.NET MVC 4 Web Application). Then, in Solution Explorer, you&#8217;ll find a class in App_Start folder called AuthConfig.cs.

Open it, and uncomment the following code:

<pre class="brush: csharp; title: ; notranslate" title="">OAuthWebSecurity.RegisterFacebookClient(
 appId: "",
 appSecret: "");

</pre>

Here, you need to add your appId and appSecret returned when you create the App on Facebook.

And thats it. Try yourself and see how easy it is.

This method is the same for ASP.NET Webforms, MVC. To WebPages, you need to uncommend the code in _AppStart.cshtml.