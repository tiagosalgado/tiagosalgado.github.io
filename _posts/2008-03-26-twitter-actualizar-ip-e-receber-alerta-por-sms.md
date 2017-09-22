---
title: Twitter &#8211; actualizar IP e receber alerta por SMS
date: 2008-03-26T00:52:21+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - .net
  - microsoft
  - twitter
  - visual studio
---
Lembrei-me de andar a brincar um bocadinho com a <a target="_blank" href="http://groups.google.com/group/twitter-development-talk/web/api-documentation">API</a> do <a target="_blank" href="http://twitter.com">Twitter</a> e nada melhor que um exemplo prático para testar 🙂

O que o código abaixo faz é o seguinte:

1º &#8211; Usa o site <http://checkip.dyndns.org/> para extrair o IP público actual.

2º &#8211; Saca o arquivo de estados da conta do <a target="_blank" href="http://twitter.com">Twitter</a> e vai buscar o último IP (ultimo estado)

3º &#8211; Actualiza o estado da conta do <a target="_blank" href="http://twitter.com">Twitter</a> com o novo IP no caso de ser diferente do devolvido no ponto 2.

<pre>'###############################################
        ' update your ip address <span style="color:#0000ff;">to</span> a twitter account
        '###############################################

        <span style="color:#0000ff;">Dim</span> c <span style="color:#0000ff;">As</span> CredentialCache = <span style="color:#0000ff;">New</span> CredentialCache
        <span style="color:#0000ff;">Dim</span> w <span style="color:#0000ff;">As</span> WebClient = <span style="color:#0000ff;">New</span> WebClient
        <span style="color:#0000ff;">Dim</span> newIP <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">String</span> = "<span style="color:#8b0000;"></span>"
        <span style="color:#0000ff;">Dim</span> oldIP <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">String</span> = "<span style="color:#8b0000;"></span>"

        <span style="color:#0000ff;">Try</span>
            ' <span style="color:#0000ff;">get</span> external ip address
            newIP = w.DownloadString("<span style="color:#8b0000;">http://checkip.dyndns.org/</span>")
            <span style="color:#0000ff;">Dim</span> regex <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">New</span> Regex("<span style="color:#8b0000;">&lt;[^&gt;]*&gt;</span>")
            newIP = regex.Replace(newIP, "<span style="color:#8b0000;"></span>")
            newIP = newIP.Substring(newIP.IndexOf("<span style="color:#8b0000;">:</span>") + 1).Trim

            'carrega <span style="color:#0000ff;">as</span> credenciais
            c.Add(<span style="color:#0000ff;">New</span> Uri("<span style="color:#8b0000;">http://twitter.com</span>"), "<span style="color:#8b0000;">Basic</span>",
<span style="color:#0000ff;">New</span> NetworkCredential("<font color="#8b0000">NomeUtilizador</font>", "<span style="color:#8b0000;">Password</span>"))
            w.Credentials = c

            ' load archive <span style="color:#0000ff;">to</span> <span style="color:#0000ff;">get</span> last ip address
            <span style="color:#0000ff;">Dim</span> archive <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">String</span> =
w.DownloadString("<span style="color:#8b0000;">http://twitter.com/account/archive.xml</span>")
            <span style="color:#0000ff;">Dim</span> _archive <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">New</span> XmlDocument
            _archive.LoadXml(archive)

            '<span style="color:#0000ff;">get</span> ip address from first childnode (last update node)
            oldIP = _archive.SelectSingleNode("<span style="color:#8b0000;">statuses</span>").
FirstChild("<span style="color:#8b0000;">text</span>").InnerText.Trim

            ' compare <span style="color:#0000ff;">new</span> <span style="color:#0000ff;">and</span> old ip address <span style="color:#0000ff;">and</span> update <span style="color:#0000ff;">if</span> &lt;&gt;
            <span style="color:#0000ff;">If</span> <span style="color:#0000ff;">Not</span> <span style="color:#0000ff;">String</span>.Equals(newIP, oldIP) <span style="color:#0000ff;">Then</span>
                ' prepare postvar
                <span style="color:#0000ff;">Dim</span> pd <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">String</span> = "<span style="color:#8b0000;">status=</span>" + newIP.ToString
                <span style="color:#0000ff;">Dim</span> pdArray <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">Byte</span>() = Encoding.ASCII.GetBytes(pd)
                <span style="color:#0000ff;">Dim</span> responseArray <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">Byte</span>() =
w.UploadData("<span style="color:#8b0000;">http://twitter.com/statuses/update.xml</span>", "<span style="color:#8b0000;">POST</span>", pdArray)

                <span style="color:#0000ff;">Dim</span> _status <span style="color:#0000ff;">As</span> <span style="color:#0000ff;">New</span> XmlDocument
                'load into xmldocument the http response
                _status.LoadXml(Encoding.ASCII.GetString(responseArray))
                'update successful ?
                <span style="color:#0000ff;">If</span> <span style="color:#0000ff;">String</span>.Equals(_status.SelectSingleNode("<span style="color:#8b0000;">status/text</span>").
InnerText.Trim, newIP) <span style="color:#0000ff;">Then</span>
                    ' yeah o/ i've done it !!!
                <span style="color:#0000ff;">End</span> <span style="color:#0000ff;">If</span>
            <span style="color:#0000ff;">Else</span>
                ' no update necessary ...
            <span style="color:#0000ff;">End</span> <span style="color:#0000ff;">If</span>

        <span style="color:#0000ff;">Catch</span> ex <span style="color:#0000ff;">As</span> Exception
            Debug.Print(ex.ToString)
        <span style="color:#0000ff;">End</span> <span style="color:#0000ff;">Try</span></pre>

Para isto funcionar, cria uma conta no <a target="_blank" href="http://twitter.com">Twitter</a> só para ter os IPs actualizados e depois na conta principal vai a esta conta e faz o Follow ( activar a opção &#8220;Device Updates&#8221; e <a target="_blank" href="https://twitter.com/devices">associar o nº de telemovel</a> à conta ).

**Podem fazer o download da solução com este código na** <a target="_blank" href="http://blog.tiagosalgado.com/projectos/"><strong>página de projectos</strong></a>**.**