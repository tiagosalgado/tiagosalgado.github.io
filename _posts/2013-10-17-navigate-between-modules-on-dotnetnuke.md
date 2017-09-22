---
title: Navigate between modules on DotNetNuke
date: 2013-10-17T12:21:32+00:00
author: tiagosalgado
layout: post
categories:
  - .net
tags:
  - Dotnetnuke
---
On DotNetNuke, each folder inside DesktopModules are considered Modules (and they can’t communicate directly between each others).

Imagine you have:

**Module1**

  * View.ascx
  * Stuff.ascx (this is a module definition only accessible by ControlKey)

If you need to open Stuff.ascx from View.ascx, you can achieve this easily using Globals.Navigate() and specifying the ControlKey, because you have the TabID and ModuleID of the current Module.

But, when we want to do the same but in different Modules (folders) is more complicate.

So if you have something like:

**Module1**

  * View.ascx (assume this has a aspx page associated)
  * Stuff.ascx (this is a module definition only accessible by ControlKey)

**Module2**

  * OtherStuff.ascx (assume this has a aspx page associated)
  * MoreStuff.ascx (this is a module definition only accessible by ControlKey)

And you want on View.ascx or Stuff.ascx (Module1) to go to MoreStuff.ascx (Module2), we need to get the TabID, ModuleID (of Module2), and the controlKey.

We have the ControlKey, because we can specify that in DNN (Extensions Manager or DNN manifest file for compiled modules), but the TabID and ModuleID are dynamic, so is different per DNN website.

So, to be able to navigate across different modules, we need to find the TabID and ModuleID related to the module we want to access.

This code would help to do that:
{% gist tiagosalgado/d643c31ccb3d3de3347671fc87eb2b95 %}
