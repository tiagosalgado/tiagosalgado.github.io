---
id: 698
title: GridView ShowHeaderWhenEmpty
date: 2011-03-26T17:29:00+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - .net
  - asp.net
---
Até à versão 3.5 da .NET Framework, para mostrarmos os cabeçalhos de uma gridview quando esta não iria conter qualquer resultado, teriamos que recorrer a soluções como esta por exemplo:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:812469c5-0cb0-4c63-8c15-c81123a09de7:5b640521-0a18-4ecb-93b4-b7b2456090ad" class="wlWriterSmartContent">
  <pre class="c#" name="code">List&lt;string&gt; rows = new List&lt;string&gt;(
    new string[] { "line1", "line2", "line3" });

rows.Clear();
if (rows.Count &gt; 0)
{
    gv.DataSource = rows;
    gv.DataBind();
}
else
{
    rows.Add("");
    gv.DataSource = rows;
    gv.DataBind();
    gv.Rows[0].Visible = false;
}</pre>
</div>

Como é obvio, isto vai sempre executar a condição “else”, mas é apenas para exemplificar como forçar a apresentação do Header na GridView mesmo que o Datasource não contenha qualquer registo.

Na versão 4.0, foi introduzida uma nova propriedade que faz com que o Header seja sempre apresentado sem recorrer a este tipo de truques. A propriedade é a ShowHeaderWhenEmpty.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:812469c5-0cb0-4c63-8c15-c81123a09de7:c7842711-cfde-4108-9c7d-0bb732a7c6a7" class="wlWriterSmartContent">
  <pre class="c#:showcolumns" name="code">&lt;asp:GridView runat="server" ID="gv" ShowHeaderWhenEmpty="true"&gt;
&lt;/asp:GridView&gt;</pre>
</div>