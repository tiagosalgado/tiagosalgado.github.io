---
title: Classes Pair e Triplet
date: 2008-09-03T23:18:40+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - asp.net
  - dotnet
  - web
---
Muito rápidamente, as classes <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.pair.aspx" target="_blank">Pair</a> e <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.triplet.aspx" target="_blank">Triplet</a> servem para guardar objectos que possam estar relaccionados. A classe <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.pair.aspx" target="_blank">Pair</a> é usada normalmente para guardar as colecções do <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.control.viewstate.aspx" target="_blank">ViewState</a> e do <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.pagestatepersister.controlstate.aspx" target="_blank">ControlState</a>, usando a propriedade <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.pair.first.aspx" target="_blank">First</a> e a proriedade <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.pair.second.aspx" target="_blank">Second</a> respectivamente.

Podemos usar a classe <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.triplet.aspx" target="_blank">Triplet</a> para guardar as cores primárias por exemplo.

<pre>Triplet cores = <span style="color:#0000ff;">new</span> Triplet("<span style="color:#8b0000;">Vermelho</span>","<span style="color:#8b0000;">Azul</span>","<span style="color:#8b0000;">Amarelo</span>");</pre>

Para fazer o output de ambas as cores deveremos ter algo assim:

<pre>lblCoresPrimarias.Text = String.Format("<span style="color:#8b0000;">As cores primárias são </span></pre>

<pre><span style="color:#8b0000;">o {0}, o {1} e o {2}.</span>", cores.First, cores.Second, cores.Third);</pre>