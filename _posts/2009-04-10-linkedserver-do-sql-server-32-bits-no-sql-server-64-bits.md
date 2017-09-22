---
title: 'LinkedServer  do SQL Server 32 bits no SQL Server 64 bits'
date: 2009-04-10T15:08:39+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - microsoft
  - sql
  - sql server
---
Esta semana andei mudar algumas base de dados que estavam no SQL Server 2005 e 2000 para o 2008.

Tudo a correr bem, até uma das base de dados incluir Stored Procedures que utilizavam um LinkedServer para o SQL Server 2000.

Testei uma simples query a uma das tabelas utilizando esse linkedserver e fui confrontado com o seguinte erro:

> OLE DB provider "SQLNCLI10" for linked server "MYSERVER" returned message "Erro não especificado".   
> OLE DB provider "SQLNCLI10" for linked server "MYSERVER" returned message "The stored procedure required to complete this operation could not be found on the server. Please contact your system administrator.".   
> Msg 7311, Level 16, State 2, Line 1   
> Cannot obtain the schema rowset "DBSCHEMA\_TABLES\_INFO" for OLE DB provider "SQLNCLI10" for linked server "MYSERVER". The provider supports the interface, but returns a failure code when it is used.

Andei um bom tempo de volta da criação do Linked Server sem qualquer sucesso, mas umas pesquisas no Google e fui encontrar a solução.

> USE [master]   
> CREATE PROCEDURE [sp\_tables\_info\_rowset\_64]   
> @table_name sysname   
> , @table_schema sysname = NULL   
> , @table_type nvarchar(255) = null   
> AS   
> DECLARE @Result int   
> SELECT @Result = 0 
> 
> EXEC @Result = sp\_tables\_info\_rowset @table\_name, @table\_schema, @table\_type   
> GO

Este erro acontece quando estamos a linkar num SQL Server de 64 bits para o Sql Server 32 bits.

Para resolver é necessário então criar este SP na tabela master do SQL Server 32 bits.