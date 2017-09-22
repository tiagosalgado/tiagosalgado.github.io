---
title: Problema ao aceder ao servidor devido ao limite de ligações simultaneas
date: 2009-04-23T13:28:33+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - microsoft
  - windows
---
Onde trabalho já aconteceu mais que uma vez tentar aceder a um servidor e não conseguir por estarem ocupadas todas as ligações permitidas.

[<img class="aligncenter size-full wp-image-356" title="server_connections_exceeded" src="http://oito.files.wordpress.com/2009/04/server_connections_exceeded.jpg" alt="server_connections_exceeded" width="461" height="117" />](http://oito.files.wordpress.com/2009/04/server_connections_exceeded.jpg)

Em certos casos pode causar um grande transtorno pois é necessário fazer alguma intervenção e não é possivel deslocarmo-nos fisicamente ao servidor e fazer o login no mesmo.

Para contornar o problema, podemos pela linha de comandos terminar uma das sessões e conseguindo assim aceder com a nossa.

Para tal, basta ir à linha de comandos do Windows e fazer o seguinte:

  1. net-use /user:NOME_UTILIZADOR [NOME_SERVIDOR](//NOME_SERVIDOR) (abre a ligação remota em modo de consola)
  2. query session /server:NOME_SERVIDOR (isto vai listar todas as sessões do servidor)
  3. reset session [ID] /server:NOME_SERVIDOR (faz o reset da sessão com o ID indicado, ID este que é visto no output do comando anterior)

Por fim, já podemos aceder remotamente pelo utilitário de acesso remoto do windows.