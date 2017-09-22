---
title: ASP.NET Membership + Location Paths
date: 2008-09-03T12:58:01+00:00
author: tiagosalgado
layout: post
categories:
  - aspnet
tags:
  - .net
  - asp.net
  - membership
  - security
  - web
---
Quando recorremos ao <a title="Membership" href="http://msdn.microsoft.com/en-us/library/yh26yfzy.aspx" target="_blank">Membership</a> para implementarmos a segurança da nossa aplicação, um dos problemas que podemos ter é quando definimos que determinada pasta será acedida apenas por utilizadores previamente autenticados e nela contemos ficheiros que usamos noutras páginas que não precisam desta mesma autenticação para serem acedidas, como ficheiros de estilos (<a title="CSS" href="http://en.wikipedia.org/wiki/Cascading_Style_Sheets" target="_blank">CSS</a>), imagens, etc.

Se tivermos por exemplo a seguinte estrutura:

/ (root)
  
/Imagens
  
/Imagens/usericon.png
  
/Estilos
  
/Estilos/estilos.css
  
/Login.aspx
  
/MinhaPaginaProtegida.aspx
  
/OutraPaginaProtegida.aspx

E no <a title="web.config" href="http://en.wikipedia.org/wiki/Web.config" target="_blank">web.config</a> definirmos que, para aceder a qualquer página  na aplicação temos que estar autenticados,<span style="font-size:x-small;color:#0000ff;"> </span>

<<span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">authorization</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">><br /> </span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">deny</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"> </span></span><span style="font-size:x-small;color:#ff0000;"><span style="font-size:x-small;color:#ff0000;">users</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">=</span></span><span style="font-size:x-small;">&#8220;</span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">*</span></span><span style="font-size:x-small;">&#8220;</span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"> /><br /> </</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">authorization</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">></span></span>

se usarmos o ficheiro de estilos ( /Estilos/estilos.css ) na nossa página de login, ao ser carregada no browser irá aparecer sem qualquer personalização feita. Isto porque na página Login.aspx teremos algo do tipo

<<span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">link</span></span> <span style="font-size:x-small;"></span><span style="font-size:x-small;color:#ff0000;"><span style="font-size:x-small;color:#ff0000;">type</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">=&#8221;text/css&#8221;</span></span> <span style="font-size:x-small;"></span><span style="font-size:x-small;color:#ff0000;"><span style="font-size:x-small;color:#ff0000;">rel</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">=&#8221;Stylesheet&#8221;</span></span> <span style="font-size:x-small;"></span><span style="font-size:x-small;color:#ff0000;"><span style="font-size:x-small;color:#ff0000;">href</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">=&#8221;Estilos/estilos.css&#8221;</span></span> <span style="font-size:x-small;"></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">/></span></span>

<div>
  e como este ficheiro está numa pasta dentro da zona protegida, o browser não vai conseguir carregar os estilos definidos na página.
</div>

Para podermos contornar isso temos que indicar no <a title="web.config" href="http://en.wikipedia.org/wiki/Web.config" target="_blank">web.config</a> que o ficheiro poderá ser acedido mesmo não sendo feita a autenticação na aplicação. Para isso usamos o elemento <a title="Element Location" href="http://msdn.microsoft.com/en-us/library/b6x6shw7.aspx" target="_blank"><location></a> e no atributo path indicamos onde está o ficheiro. Por fim, indicamos que todos os utilizadores (incluindo os anónimos) poderão aceder a ele, fazendo assim com que a nossa página de login consiga ir buscar os estilos a serem carregados.

<div>
  <span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><</span></span></span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">location</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"> </span></span><span style="font-size:x-small;color:#ff0000;"><span style="font-size:x-small;color:#ff0000;">path</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">=</span></span><span style="font-size:x-small;">&#8220;</span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">estilos.css</span></span><span style="font-size:x-small;">&#8220;</span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">><br /> <</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">system.web</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">><br /> <</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">authorization</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">><br /> <</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">allow</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"> </span></span><span style="font-size:x-small;color:#ff0000;"><span style="font-size:x-small;color:#ff0000;">users</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">=</span></span><span style="font-size:x-small;">&#8220;</span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">*</span></span><span style="font-size:x-small;">&#8220;</span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">/><br /> </</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">authorization</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">><br /> </</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">system.web</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">><br /> </</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">location</span></span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">></span></span></span></span></span></span>
</div>