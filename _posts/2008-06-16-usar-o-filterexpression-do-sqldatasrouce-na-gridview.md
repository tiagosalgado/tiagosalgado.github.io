---
title: Usar o FilterExpression do SqlDataSrouce na GridView
date: 2008-06-16T13:25:45+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - .net
---
Se quiser dar a possibilidade do utilizador filtrar os dados que está a ver na gridview, uma das formas é usar o FilterExpression do SqlDataSource ligado à gridview.

Algo do genero: 

<span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">if</span></span></span></span><span style="font-size:x-small;">(!</span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">string</span></span><span style="font-size:x-small;">.IsNullOrEmpty(txtFiltro.Text.Trim()))<br /> </span><span style="font-size:x-small;">{<br /> </span><span style="font-size:x-small;">sqlDsTeste.FilterExpression = </span><span style="font-size:x-small;color:#2b91af;"><span style="font-size:x-small;color:#2b91af;">String</span></span><span style="font-size:x-small;">.Format(</span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">&#8220;nome like &#8216;%{0}%'&#8221;</span></span><span style="font-size:x-small;">, txtFiltro.Text);</span><span style="font-size:x-small;"><br /> }<br /> </span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">else<br /> </span></span><span style="font-size:x-small;">{<br /> sqlDsTeste.FilterExpression = </span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">&#8220;&#8221;</span></span><span style="font-size:x-small;">;</span><span style="font-size:x-small;"><br /> </span><span style="font-size:x-small;">}</span>

O problema está quando o filtro retorna por exemplo um nº de resultados que seja superior ao definido para mostrar por página, fazendo com que ao mudar de página o filtro seja &#8220;esquecido&#8221; devido ao PostBack e mostre novamente todos os resultados da query no SelectCommand do SqlDataSource.

Para contornar isto usei o ViewState e adicionei após definir a FilterExpression o seguinte:

<div>
  <span style="font-size:x-small;"><span style="font-size:x-small;">ViewState.Add(</span></span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">&#8220;filtro&#8221;</span></span><span style="font-size:x-small;">, sqlDsTeste.FilterExpression);</span>
</div>

<div>
  <span style="font-size:x-small;">E depois no Page_Load adiciono o seguinte para aplicar o filtro definido antes do refresh à página:</span>
</div>

<div>
  <span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;"></p> 
  
  <div>
  </div>
  
  <p>
    </span>
  </p>
  
  <div>
    <span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;">(ViewState[</span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">&#8220;filtro&#8221;</span></span><span style="font-size:x-small;">] != </span><span style="font-size:x-small;color:#0000ff;"><span style="font-size:x-small;color:#0000ff;">null</span></span><span style="font-size:x-small;">)<br /> {<br /> sqlDsTeste.FilterExpression = ViewState[</span><span style="font-size:x-small;color:#a31515;"><span style="font-size:x-small;color:#a31515;">&#8220;filtro&#8221;</span></span><span style="font-size:x-small;">].ToString();<br /> }</span></span>
  </div>
  
  <p>
     
  </p>
  
  <p>
    </span></div> 
    
    <p>
      <span style="font-size:x-small;">Se conhecerem uma forma mais rápida e/ou correcta avisem 🙂</span>
    </p>
    
    <p>
       
    </p>
    
    <p>
       
    </p>