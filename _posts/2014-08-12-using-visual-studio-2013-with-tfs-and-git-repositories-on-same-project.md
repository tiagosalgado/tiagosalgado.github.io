---
title: Using Visual Studio 2013 with TFS and Git repositories on same project
date: 2014-08-12T15:17:31+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - git
  - tfs
  - visual studio
---
With Visual Studio 2013 is a nightmare to keep a git repository but having TFS as a source control plugin activated when we load the solution. Every time we open the solution, by default Visual Studio will select the Git provider and ignore completely TFS (oh, irony…)

To solve that, I had to move out the .git folder to an external folder and point the git dir to the actual source code folder.

So an example is:

<pre class="brush: plain; gutter: false; title: ; notranslate" title="">C:\Git\ (where I have my different .git repositories per project stored)
-- C:\Git\ProjectOne\.git
-- C:\Git\ProjectTwo\.git
</pre>

<pre class="brush: plain; gutter: false; title: ; notranslate" title="">C:\Code (where I have all projects source code)
-- C:\Code\ProjectOne\.git (this is a file, not a folder anymore)
-- C:\Code\ProjectTwo\.git (this is a file, not a folder anymore)
</pre>

So, this .git file has to be generated as:

<pre class="brush: bash; title: ; notranslate" title="">$ echo "gitdir: /git/ProjectOne/.git" &gt; .git
</pre>

After doing this, you can open Visual Studio and TFS will be selected by default. You still be able to run a “git status” or any git command as you always did before.