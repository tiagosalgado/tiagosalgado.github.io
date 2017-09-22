---
title: Error on GetSkin() loading a DotNetNuke website
date: 2013-08-02T23:36:56+00:00
author: tiagosalgado
layout: post
categories:
  - .net
tags:
  - Dotnetnuke
---
Trying to load a DotNetNuke website, I was getting an error on Default.aspx page in this specific line:

> **Server Error in &#8216;/&#8217; Application.**
  
> **Object reference not set to an instance of an object**
  
> **Description: An unhandled exception occurred during the execution of the current web request. Please review the  stack trace for more information about the error and where it originated in the code.**
  
> **<span>Exception Details: System.NullReferenceException: Object reference not set to an instance of an object.</span>**

<pre class="brush: csharp; title: ; notranslate" title="">ctlSkin = IsPopUp ? UI.Skins.Skin.GetPopUpSkin(this) : UI.Skins.Skin.GetSkin(this); </pre>

(Line 666, coincidence? Eheh)

The error was related to DNNMenu web user control, saying that couldn&#8217;t found the .ascx file.

After struggling a bit, I&#8217;ve found my DesktopModules folder was being considered as a Virtual Directory on IIS and pointing to a wrong path. So after fixing, pointing out to the right path, everything was working again 🙂