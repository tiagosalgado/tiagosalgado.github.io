---
title: Extrair texto entre tags HTML (LINQ+Regex)
date: 2010-04-15T22:26:21+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - .net
  - 'C#'
  - LINQ
  - Regular Expressions
---
Hoje o meu colega de trabalho pediu-me para o ajudar a extrair uma parte do texto de uma página p/ ser posteriormente enviado.

Não se trata nada de complexo, apenas apeteceu-me deixar aqui p/ consultar mais tarde caso precise 🙂

O HTML da página que deve ser pesquisado é algo como:

<div class="csharpcode">
  <pre class="alt"><span class="kwrd">&lt;</span><span class="html">TD</span> <span class="attr">valign</span>=<span class="attr">top</span> <span class="attr">colspan</span>=<span class="attr">6</span><span class="kwrd">&gt;</span>TESTE 1XPTO ONLINE<span class="kwrd">&lt;/</span><span class="html">TD</span><span class="kwrd">&gt;</span></pre>
</div></p> 

Para o fazer, fiz o seguinte código:

<div class="csharpcode">
  <pre class="alt">Regex r = <span class="kwrd">new</span> Regex(<span class="str">"&lt;TD(.*?)&gt;(.*?)&lt;/TD&gt;"</span>);</pre>
  
  <pre><span class="kwrd">string</span> s = <span class="str">@"<TD valign=top colspan=6>TESTE 1XPTO ONLINE</TD></pre>


<pre class="alt">            &lt;TD valign=top colspan=6&gt;TESTE 2XPTO ONLINE&lt;/TD&gt;</pre>


<pre>            &lt;TD valign=top colspan=6&gt;TESTE 3XPTO ONLINE&lt;/TD&gt;</pre>


<pre class="alt">            &lt;TD valign=top colspan=6&gt;TESTE 4XPTO ONLINE&lt;/TD&gt;</pre>


<pre>            &lt;TD valign=top colspan=6&gt;TESTE 5XPTO ONLINE&lt;/TD&gt;"&lt;/span>;</pre>


<pre class="alt">&#160;</pre>


<pre>MatchCollection mc = r.Matches(s);</pre>


<pre class="alt"><span class="kwrd">foreach</span> (Match m <span class="kwrd">in</span> mc)</pre>


<pre>{</pre>


<pre class="alt">    Console.WriteLine(m.Groups[2].Value.Trim());</pre>


<pre>}</pre>
</div>



<p>
  Outra forma de fazer o mesmo, e recorrendo ao LINQ, é esta:
</p>


<div class="csharpcode">
  <pre class="alt">var q = from Match m <span class="kwrd">in</span> <span class="kwrd">new</span> Regex(<span class="str">@"&lt;TD(.*?)&gt;(.*?)&lt;/TD&gt;"</span>).Matches(s)</pre>
  
  
  <pre>        select m.Groups[2].Value.Trim();</pre>
  
  
  <pre class="alt">&#160;</pre>
  
  
  <pre>q.ToArray&lt;<span class="kwrd">string</span>&gt;().ToList().ForEach(<span class="kwrd">new</span> Action&lt;<span class="kwrd">string</span>&gt;(EnviarSinais));</pre>
  
</div>




<p>
  Por fim, basta criar a função <strong>EnviarSinais</strong>:
</p>


<div class="csharpcode">
  <pre class="alt"><span class="kwrd">static</span> <span class="kwrd">void</span> EnviarSinais(<span class="kwrd">string</span> str)</pre>
  
  
  <pre>{</pre>
  
  
  <pre class="alt">        Console.WriteLine(str);</pre>
  
  
  <pre>}</pre>
  
</div>



<p>
  Quanto ao código em LINQ, se houver melhor forma de o fazer, indiquem pf 🙂
</p>