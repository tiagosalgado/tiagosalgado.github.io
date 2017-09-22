---
title: 'Utilizar a API do bit.ly para gerar um url curto em C#'
date: 2010-04-24T00:33:13+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - 'C#'
  - dev
  - dicas
---
Hoje andei a dar uma olhadela à API do serviço bit.ly. Para utilizarmos basta mesmo criar uma conta, e com a key que gera para utilizarmos a API rapidamente fazemos qualquer brincadeira.

Segue um exemplo rápido de como gerar um link curto a partir da URL inserida.

<p style="background-color: white;">
  <code style="font-size: 12px;">&lt;span style="color: blue;">string &lt;/span>&lt;span style="color: black;">username &lt;/span>&lt;span style="color: blue;">= &lt;/span>&lt;span style="color: darkred;">"username"&lt;/span>&lt;span style="color: gray;">;&lt;br />
&lt;/span>&lt;span style="color: blue;">string &lt;/span>&lt;span style="color: black;">api &lt;/span>&lt;span style="color: blue;">= &lt;/span>&lt;span style="color: darkred;">"your_api_key"&lt;/span>&lt;span style="color: gray;">;&lt;br />
&lt;/span>&lt;span style="color: blue;">using &lt;/span>&lt;span style="color: gray;">(&lt;/span>&lt;span style="color: black;">WebClient w &lt;/span>&lt;span style="color: blue;">= new &lt;/span>&lt;span style="color: black;">WebClient&lt;/span>&lt;span style="color: gray;">())&lt;br />
&lt;/span>&lt;span style="color: black;">{&lt;br />
&lt;/span>&lt;span style="color: blue;">string &lt;/span>&lt;span style="color: black;">LongUrl &lt;/span>&lt;span style="color: blue;">= &lt;/span>&lt;span style="color: darkred;">"http://blog.tiagosalgado.com"&lt;/span>&lt;span style="color: gray;">;&lt;br />
&lt;/span>&lt;span style="color: blue;">string &lt;/span>&lt;span style="color: black;">bitLyUrl &lt;/span>&lt;span style="color: blue;">=string&lt;/span>&lt;span style="color: black;">.Format&lt;/span>&lt;span style="color: gray;">(&lt;/span>&lt;span style="color: darkred;">"http://api.bit.ly/v3/shorten?login={0}&apiKey={1}&uri={2}&format=txt"&lt;/span>&lt;span style="color: gray;">,&lt;/span>&lt;span style="color: black;">username&lt;/span>&lt;span style="color: gray;">,&lt;/span>&lt;span style="color: black;">api&lt;/span>&lt;span style="color: gray;">,&lt;/span>&lt;span style="color: black;">LongUrl&lt;/span>&lt;span style="color: gray;">);&lt;br />
&lt;/span>&lt;span style="color: blue;">string &lt;/span>&lt;span style="color: black;">ShortUrl &lt;/span>&lt;span style="color: blue;">= &lt;/span>&lt;span style="color: black;">w.DownloadString&lt;/span>&lt;span style="color: gray;">(&lt;/span>&lt;span style="color: black;">bitLyUrl&lt;/span>&lt;span style="color: gray;">);&lt;br />
&lt;/span>&lt;span style="color: black;">Console.Write&lt;/span>&lt;span style="color: gray;">(&lt;/span>&lt;span style="color: black;">ShortUrl&lt;/span>&lt;span style="color: gray;">);&lt;br />
&lt;/span>&lt;span style="color: black;">Console.Read&lt;/span>&lt;span style="color: gray;">();&lt;br />
&lt;/span>&lt;span style="color: black;">}&lt;/span></code>
</p>

E temos algo como isto:

<img src="http://img691.imageshack.us/img691/154/outputbitlyapp.png" alt="" width="677" height="342" />

<a title="Download do Projecto" href="http://www.box.net/shared/pr65xpnjhe" target="_blank"><img src="http://e3.boxcdn.net/resources/a5y417x8pf/thumbs/27x30/application/zip.gif" border="0" alt="File icon" width="24" height="24" align="absMiddle" />bitLy_get_shorturl_csharp.zip</a>