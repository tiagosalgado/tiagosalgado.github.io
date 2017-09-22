---
title: SSMS 2008 + Alterar nr de registos a retornar nas opções SELECT e EDIT do menu de contexto da tabela
date: 2009-05-28T22:48:35+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - dicas
  - microsoft
  - software
  - sql server 2008
---
Na ultima versão do SQL Server Management Studio (SSMS), ao abrirmos o menu de contexto de uma tabela na nossa base de dados, as opções de SELECT e EDIT têm um limite de registos que irão ser retornados definido por defeito, 1000 e 200 respectivamente.

<a href="http://oito.files.wordpress.com/2009/05/sql_server_2k8_select_edit_rows_limits.jpg" target="_blank"><img style="border-bottom:0;border-left:0;display:inline;border-top:0;border-right:0;" title="sql_server_2k8_select_edit_rows_limits" border="0" alt="sql_server_2k8_select_edit_rows_limits" src="http://oito.files.wordpress.com/2009/05/sql_server_2k8_select_edit_rows_limits_thumb.jpg" width="777" height="696" /></a> 

Isto faz com que sempre que quisermos retornar os resultados de uma tabela com mais de 200 (EDIT) e 1000 (SELECT), temos que ir à query gerada e retirar/alterar o “TOP N”.

Se para nós estes valores não são suficientes e queremos alterá-los, ou até mesmo ignora-los e retornarmos todos os registos da tabela, podemos fazê-lo no menu **Tools > Options > SQL Server Object Explorer > Commands** e alterar os valores lá definidos ou simplesmente atribuir o valor 0 (zero) fazendo com que não seja incluida a expressão TOP N.

<a href="http://oito.files.wordpress.com/2009/05/image7.png" target="_blank"><img style="border-bottom:0;border-left:0;display:inline;border-top:0;border-right:0;" title="image" border="0" alt="image" src="http://oito.files.wordpress.com/2009/05/image_thumb6.png" width="747" height="428" /></a> 

Após esta alteração, já podem usar ambas opções com os novos valores definidos.