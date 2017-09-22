---
title: Apagar items de uma dropdownlist e manter um default
date: 2008-09-22T22:50:39+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - .net
  - asp.net
  - extension methods
  - web
---
Uma situação vulgar é termos uma <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx" target="_blank">DropDownList</a> com uma lista de opções retornadas da base de dados para o utilizador poder seleccionar uma delas.

Para evitar que uma das opções está seleccionada por defeito, recorre-se normalmente à opção &#8220;&#8211; Seleccione &#8211;&#8221; ou &#8220;&#8211; Escolha uma opção &#8211;&#8220;, etc etc etc.

Para evitar que tenhamos um <a href="http://msdn.microsoft.com/en-us/library/ms180026.aspx" target="_blank">UNION</a> na nossa query que retorna essas mesmas opções ou mesmo um registo na tabela para tal, adicionando ao output a tal opção &#8220;default&#8221;, podemos adicionar esse mesmo item à <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx" target="_blank">DropDownList</a>.

[<img style="border-right:0;border-top:0;border-left:0;border-bottom:0;" height="81" alt="image" src="http://oito.files.wordpress.com/2008/09/image-thumb.png" width="512" border="0" />](http://oito.files.wordpress.com/2008/09/image.png) 

Para actualizar os dados, recorrendo ao metodo <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.databind.aspx" target="_blank">DataBind()</a>, fará com que sejam adicionados os items retornados aos que já lá estão. Ou seja, antes de fazer o <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.databind.aspx" target="_blank">DataBind()</a> é necessário apagar todos os items (usando o xpto.Items.Clear()) e só depois então faremos o <a href="http://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.databind.aspx" target="_blank">DataBind()</a>.

Nesta altura, a opção &#8220;&#8211; Seleccione &#8211;&#8221; foi à vida e temos que adiciona-la novamente à dropdownlist.

Uma forma de evitar isto é adicionar um método (recorrendo aos <a href="http://msdn.microsoft.com/en-us/library/bb383977.aspx" target="_blank">Extension Methods</a>) para apagar os items e preservar o item que queremos que apareça sempre por defeito.

[<img style="border-right:0;border-top:0;border-left:0;border-bottom:0;" height="194" alt="image" src="http://oito.files.wordpress.com/2008/09/image-thumb1.png" width="512" border="0" />](http://oito.files.wordpress.com/2008/09/image1.png) 

Desta forma, o método **ClearItems** aparece agora no Intellisence e basta passar o parâmetro **preserveDefaultItem** a _true_.

[<img style="border-right:0;border-top:0;border-left:0;border-bottom:0;" height="168" alt="image" src="http://oito.files.wordpress.com/2008/09/image-thumb2.png" width="512" border="0" />](http://oito.files.wordpress.com/2008/09/image2.png) 

<a href="http://www.box.net/shared/nilatrupmx" target="_blank">Exemplo</a>