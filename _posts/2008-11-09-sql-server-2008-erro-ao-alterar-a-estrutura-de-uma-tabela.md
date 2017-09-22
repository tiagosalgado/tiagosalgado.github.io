---
title: SQL Server 2008 – erro ao alterar a estrutura de uma tabela
date: 2008-11-09T00:41:49+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - microsoft
  - sql server 2008
---
Ao tentar alterar o tipo de dados de uma coluna, surgiu-me um erro ao tentar gravar estas alterações, não me permitindo fazer o que pretendia.

[<img title="sql2k8_savedialogwarnig" style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" height="194" alt="sql2k8_savedialogwarnig" src="http://oito.files.wordpress.com/2008/11/sql2k8-savedialogwarnig-thumb.jpg" width="244" border="0" />](http://oito.files.wordpress.com/2008/11/sql2k8-savedialogwarnig.jpg) 

Para contornar isto, temos que desactivar a opção “Prevent saving changes that require table re-creation”.

Para isso vao ao menu Tools > Options > Designers > Table and Database Designers e desmarcam essa opção.

[<img title="sql2k8_optionsdialog" style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" height="371" alt="sql2k8_optionsdialog" src="http://oito.files.wordpress.com/2008/11/sql2k8-optionsdialog-thumb.jpg" width="644" border="0" />](http://oito.files.wordpress.com/2008/11/sql2k8-optionsdialog.jpg)