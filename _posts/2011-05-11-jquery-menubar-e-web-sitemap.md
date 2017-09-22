---
title: jQuery Menubar e web.sitemap
date: 2011-05-11T22:37:16+00:00
author: tiagosalgado
layout: post
categories:
  - asp.net
tags:
  - asp.net
  - jquery
---
Após o HTML 5 Microsoft WebCamp Portugal, surgiu-me algum interesse em explorar o plugin [Menubar](http://wiki.jqueryui.com/w/page/38666403/Menubar).

Este plugin transforma uma lista num menu bastante agradável, e como tal, gostava de o implementar num projecto em que utilizo o web.sitemap.

Para tal, precisei de recorrer a um XLST para transformar o web.sitemap, que não é mais que um XML, numa lista no formato que o Menubar necessita.

Segue então um exemplo do web.sitemap:

<?xml version="1.0" encoding="utf-8"?>   
<siteMap xmlns="<http://schemas.microsoft.com/AspNet/SiteMap-File-1.0">>   
&#160;&#160;&#160; <siteMapNode url="~/" title="Home" description="Home">   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <siteMapNode url="" title="Menu 1" description="Menu 1" >   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <siteMapNode url="<http://www.google.pt"> title="Submenu 1" description="Submenu 1" />   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </siteMapNode>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <siteMapNode url="" title="Menu 2" description="Menu 2">   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <siteMapNode url="<http://www.microsoft.com"> title="Submenu 2" description="Submenu 2" />   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <siteMapNode url="<http://www.apple.com"> title="Submenu 3" description="Submenu&#160; 3" />   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </siteMapNode>   
&#160;&#160;&#160; </siteMapNode>   
</siteMap>

Para transformar o XML numa lista, usei o seguinte XSLT

<?xml version="1.0" encoding="utf-8"?>   
<xsl:stylesheet version="1.0" xmlns:xsl="<http://www.w3.org/1999/XSL/Transform"> xmlns:map="<http://schemas.microsoft.com/AspNet/SiteMap-File-1.0">   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; exclude-result-prefixes="map">   
&#160; <xsl:output method="xml" encoding="utf-8" indent="yes"/>

&#160; <xsl:template match="map:siteMapNode">   
&#160;&#160;&#160; <li>   
&#160;&#160;&#160;&#160;&#160; <a href="{@url}" title="{@description}">   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <xsl:value-of select="@title"/>   
&#160;&#160;&#160;&#160;&#160; </a>   
&#160;&#160;&#160;&#160;&#160; <xsl:if test="map:siteMapNode">   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <ul>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <xsl:call-template name="mapNode"/>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </ul>   
&#160;&#160;&#160;&#160;&#160; </xsl:if>   
&#160;&#160;&#160; </li>   
&#160; </xsl:template>

&#160; <xsl:template name="mapNode" match="/\*/\*">   
&#160;&#160;&#160; <xsl:apply-templates/>   
&#160; </xsl:template>   
</xsl:stylesheet>

Desta forma, irei obter uma lista igual à seguinte:

<?xml version="1.0" encoding="utf-16" ?>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <li><a href="" title="Menu 1">Menu 1</a>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <ul>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <li><a href="<http://www.google.pt"> title="Submenu 1">Submenu 1</a></li>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </ul>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </li>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <li><a href="" title="Menu 2">Menu 2</a>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <ul>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <li><a href="<http://www.microsoft.com"> title="Submenu 2">Submenu 2</a></li>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <li><a href="<http://www.apple.com"> title="Submenu&#160; 3">Submenu 3</a></li>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </ul>   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </li>

Por fim, basta-nos adicionar um controlo XML que irá fazer o render da lista.

<ul id="bar1" class="menubar">   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <asp:Xml runat="server" ID="xmlSiteMapViewer" DocumentSource="~/web.sitemap" TransformSource="~/sitemap.xslt" />   
&#160;&#160;&#160; </ul>

A inserção do controlo entre uma “unordered list” foi propositado, pois só assim é que teremos o menu a funcionar correctamente e teremos um resultado identico ao que está no seguinte site:

<http://view.jqueryui.com/master/demos/menubar/default.html>