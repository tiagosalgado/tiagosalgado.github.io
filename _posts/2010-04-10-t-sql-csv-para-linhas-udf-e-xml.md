---
title: 'T-SQL: CSV para Linhas (UDF e XML)'
date: 2010-04-10T18:19:57+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - dev
  - sql
  - sql server 2008
---
Esta semana precisei de fazer exactamente o que o titulo do post indica, ou seja, retornar todas as linhas de uma tabela mas ao mesmo tempo separar os valores numa das colunas em cada linha.

Para testar, criei a seguinte tabela:

<div class="csharpcode">
  <pre class="alt"><span class="kwrd">CREATE</span> <span class="kwrd">TABLE</span> [dbo].[Agentes](</pre>
  
  <pre>    [Codigo] [<span class="kwrd">varchar</span>](50) <span class="kwrd">NULL</span>,</pre>
  
  <pre class="alt">    [Nome] [<span class="kwrd">varchar</span>](50) <span class="kwrd">NULL</span>,</pre>
  
  <pre>    [Emails] [<span class="kwrd">varchar</span>](100) <span class="kwrd">NULL</span></pre>
  
  <pre class="alt">) </pre>
</div></p> 

Inseri alguns dados com o mesmo formato da tabela real que iria depois utilizar:

<div class="csharpcode">
  <pre class="alt">INSERT <span class="kwrd">INTO</span> Agentes </pre>
  
  <pre><span class="kwrd">VALUES</span>(<span class="str">'C12345'</span>,<span class="str">'Agente 1'</span>,<span class="str">'agente1@xpto.pt;agente1.loja@xpto.pt'</span>)</pre>
  
  <pre>INSERT <span class="kwrd">INTO</span> Agentes </pre>
  
  <pre><span class="kwrd">VALUES</span>(<span class="str">'C12346'</span>,<span class="str">'Agente 2'</span>,<span class="str">'agente2@xpto.pt;agente2.loja@xpto.pt'</span>)</pre>
</div>

Como já podem perceber, o campo “Emails” precisa de ser retornado com apenas um email, ou seja, preciso de passar uma listagem como esta

<img src="http://img256.imageshack.us/img256/1892/select1.png" width="362" height="66" />

para uma como esta

<img src="http://img256.imageshack.us/img256/3897/select2c.png" width="267" height="98" />

Para isso precisei de fazer uma função que me separasse cada um emails de forma a poder retornar um por cada linha.

<div class="csharpcode">
  <pre class="alt"><p>
  <span class="kwrd">CREATE</span> <span class="kwrd">FUNCTION</span> dbo.SplitEmails(@separador <span class="kwrd">char</span>(1), <br />@emails <span class="kwrd">varchar</span>(512))
</p></pre>
  
  <pre><span class="kwrd">RETURNS</span> <span class="kwrd">table</span></pre>
  
  <pre class="alt"><span class="kwrd">AS</span></pre>
  
  <pre><span class="kwrd">RETURN</span> (</pre>
  
  <pre class="alt">    <span class="kwrd">WITH</span> Emails(pn, <span class="kwrd">start</span>, stop) <span class="kwrd">AS</span> (</pre>
  
  <pre>      <span class="kwrd">SELECT</span> 1, 1, CHARINDEX(@separador, @emails)</pre>
  
  <pre class="alt">      <span class="kwrd">UNION</span> <span class="kwrd">ALL</span></pre>
  
  <pre>      <span class="kwrd">SELECT</span> pn + 1, stop + 1, CHARINDEX(@separador, @emails, </pre>
  
  <pre>stop + 1)</pre>
  
  <pre class="alt">      <span class="kwrd">FROM</span> Emails</pre>
  
  <pre>      <span class="kwrd">WHERE</span> stop &gt; 0</pre>
  
  <pre class="alt">    )</pre>
  
  <pre>    <span class="kwrd">SELECT</span> <span class="kwrd">SUBSTRING</span>(@emails, <span class="kwrd">start</span>, <span class="kwrd">CASE</span> <span class="kwrd">WHEN</span> stop &gt; 0 <span class="kwrd">THEN</span> </pre>
  
  <pre>stop-<span class="kwrd">start</span> <span class="kwrd">ELSE</span> 512 <span class="kwrd">END</span>) <span class="kwrd">AS</span> email</pre>
  
  <pre class="alt">    <span class="kwrd">FROM</span> Emails</pre>
  
  <pre>  )</pre>
</div>

  
Por fim, para ter a listagem com o formato pretendido, bastou fazer o CROSS APPLY com a minha tabela de agentes, e está o trabalho feito.

<div class="csharpcode">
  <pre class="alt"><span class="kwrd">SELECT</span> a.Codigo,a.Nome,c.email <span class="kwrd">FROM</span> Agentes a</pre>
  
  <pre><span class="kwrd">CROSS</span> APPLY SplitEmails(<span class="str">';'</span>,a.Emails) c</pre>
</div>

<img src="http://img406.imageshack.us/img406/6914/select3.png" width="271" height="110" />

Outra forma de retornar esta listagem, indicada pelo <a href="http://twitter.com/CaioProiete" target="_blank">@Caio</a>, era recorrendo ao XML e evitava assim criar uma função para fazer o split dos emails.

<div class="csharpcode">
  <pre class="alt"><span class="kwrd">WITH</span> Consulta <span class="kwrd">AS</span></pre>
  
  <pre>(</pre>
  
  <pre class="alt"><span class="kwrd">SELECT</span> Codigo, Nome,</pre>
  
  <pre><span class="kwrd">CAST</span>(<span class="str">'&lt;email&gt;'</span> + </pre>
  
  <pre>REPLACE(Emails, <span class="str">';'</span>, <span class="str">'&lt;/email&gt;&lt;email&gt;'</span>) + </pre>
  
  <pre><span class="str">'&lt;/email&gt;'</span> <span class="kwrd">AS</span> XML) </pre>
  
  <pre><span class="kwrd">AS</span> EmailXml</pre>
  
  <pre class="alt"><span class="kwrd">FROM</span> Agentes</pre>
  
  <pre>)</pre>
  
  <pre class="alt">&#160;</pre>
  
  <pre><span class="kwrd">SELECT</span> Codigo, Nome,</pre>
  
  <pre class="alt">    r.<span class="kwrd">value</span>(<span class="str">'.'</span>, <span class="str">'varchar(255)'</span>) <span class="kwrd">AS</span> Email</pre>
  
  <pre><span class="kwrd">FROM</span></pre>
  
  <pre class="alt">    Consulta</pre>
  
  <pre><span class="kwrd">CROSS</span> APPLY</pre>
  
  <pre class="alt">    consulta.EmailXml.nodes(<span class="str">'email'</span>) <span class="kwrd">AS</span> x(r)</pre>
</div>