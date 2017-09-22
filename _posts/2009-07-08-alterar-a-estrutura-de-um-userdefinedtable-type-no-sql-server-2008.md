---
title: Alterar a estrutura de um UserDefinedTable Type no SQL Server 2008
date: 2009-07-08T13:19:09+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - dicas
  - microsoft
  - sql server
  - sql server 2008
---
Uma das novidades do SQL Server 2008, foi o aparecimento do <a href="http://technet.microsoft.com/en-us/library/bb522526.aspx" target="_blank">UserDefinedTable Type</a>, permitindo assim criar uma estrutura de uma tabela e usa-la como um table-value parameter.

Num projecto em que estou a trabalhar actualmente utilizo este tipo de dados e precisei de o alterar após já o ter referenciado num stored procedure. Como está bem explicito <a href="http://technet.microsoft.com/en-us/library/bb522526.aspx" target="_blank">aqui</a>, não podemos alterar a estrutura do tipo de dados após te-lo criado.

> The user-defined table type definition cannot be modified after it is created.

Para o fazer, teremos que remover e criar novamente o nosso tipo com as alterações pretendidas. Extra-trabalho quando já o temos referenciado, pois como seria de esperar não deixa antes de removermos essas mesmas referências.

Para contornar o problema segui os seguintes passos:

  1. Criar uma novo tipo igual ao que pretendo remover com um novo nome
  2. Alterar para o novo nome todas as referências do que pretendemos alterar
  3. Remover o tipo que existia inicialmente
  4. Criar o novo com as alterações pretendidas e voltar a substituir todas as referências
  5. Remover o tipo criado no ponto 1.

Não me parece a melhor solução para este problema, mas para já é que se arranja.

Se existir uma melhor agradeço que me digam. 🙂

>