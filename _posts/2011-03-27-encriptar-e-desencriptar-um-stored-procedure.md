---
title: Encriptar e desencriptar um Stored Procedure
date: 2011-03-27T00:57:53+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - sql server
---
Quando não queremos que tenham acesso ao código dos Stored Procedures, Triggers ou Views, que implementamos numa base de dados, podemos criar e encriptar facilmente, bastando para isso adicionar um “WITH ENCRYPTION”.

Segue um exemplo para a criação de um Stored Procedure:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:812469c5-0cb0-4c63-8c15-c81123a09de7:cf553843-dc71-42fa-aac0-86cfbe39a808" class="wlWriterEditableSmartContent">
  <pre name="code" class="sql">CREATE PROCEDURE encrypted_SP(@var varchar(10))
WITH ENCRYPTION
AS
	-- this is an encrypted stored procedure
	print @var</pre>
</div>

Desta forma, o nosso SP aparecerá no Object Explorer da seguinte forma:

![](http://img694.imageshack.us/img694/3120/sqlobjectexplorer.png)

Como podem ver, é muito simples criar um stored procedure encriptado.

Mas por vezes, até dava (mesmo) jeito conseguirmos ver o código, e para isso é necessário recorrer a ferramentas de terceiros.

É aqui que entra o <a href="http://optillect.com/products/sqldecryptor/overview.html" target="_blank">SQL Decryptor</a> da <a href="http://optillect.com" target="_blank">Optillect</a>. Acedendo à nossa base de dados por esta aplicação, com apenas um duplo clique sobre o Stored Procedure que está encriptado, rapidamente conseguimos ver o código que tanto pretendemos <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-smile" alt="Smile" src="http://blog.tiagosalgado.com/wp-content/uploads/2011/03/wlEmoticon-smile.png" />

<img src="http://img22.imageshack.us/img22/7672/decryptsp.png" width="640" height="480" />