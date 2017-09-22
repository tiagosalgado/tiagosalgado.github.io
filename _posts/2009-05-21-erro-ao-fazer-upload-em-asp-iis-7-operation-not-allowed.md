---
title: Erro ao fazer upload em ASP + IIS 7 – Operation not Allowed
date: 2009-05-21T22:25:19+00:00
author: tiagosalgado
layout: post
categories:
  - asp.net
tags:
  - asp
  - iis
  - microsoft
---
Ao tentar fazer o upload de um ficheiro numa aplicação em ASP (sim, ainda se vai mexendo nisso), surgia o erro “Operation Not Allowed”.

O problema estava no limite máximo permitido de tamanho para upload de um ficheiro (200kb +/-) no IIS, e para contornar bastou alterar esse limite para um valor superior.

IIS Manager > SERVIDOR > ASP > Limits Properties > Maximum Requesting Entity Body Limit