---
title: Collection was modified; enumeration operation may not execute
date: 2009-06-17T12:38:34+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - .net
  - dev
  - dicas
  - dotnet
  - microsoft
---
Para remover um item de uma colecção, nada mais do que

List<string> s = new List<string>() { &#8220;1&#8221;, &#8220;2&#8221; };
  
s.Remove(&#8220;1&#8221;);

Mas quando usamos a colecção dentro de um ciclo e queremos remover o item que está carregado actualmente, podemos ser surpreendidos com uma excepção do tipo &#8220;Collection was modified; enumeration operation may not execute&#8221;.

List<string> s = new List<string>() { &#8220;1&#8221;, &#8220;2&#8221; };
  
foreach (string ss in s)
  
{
  
s.Remove(ss);
  
}

Para contornar este erro, e eliminar todos os items que pretendemos durante o ciclo, basta a seguinte alteração ao código

List<string> s = new List<string>() { &#8220;1&#8221;, &#8220;2&#8221; };
  
foreach (string ss in **new List<string>(s)**)
  
{
  
s.Remove(ss);
  
}