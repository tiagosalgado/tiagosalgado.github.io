---
title: HttpWebRequest + forçar validação de certificado
date: 2008-07-07T10:06:45+00:00
author: tiagosalgado
layout: post
categories:
  - Uncategorized
tags:
  - .net
  - visual studio
  - web
  - www
---
Se abrirmos num browser, ex: IE7, um site cujo certificado não é considerado válido, aparece uma mensagem com essa indicação e é necessária a indicação de que queremos avançar para o site (ou não).

Acontece o mesmo quando tentamos aceder a esse mesmo site usando o HttpWebRequest. Por exemplo:

> Dim site As String = &#8220;<https://www.site.com/pagina.aspx>&#8221;
  
> Dim req As HttpWebRequest = WebRequest.Create(site)
  
> Dim c As CredentialCache = New CredentialCache
  
> c.Add(New Uri(site), &#8220;Basic&#8221;, New NetworkCredential(&#8220;username&#8221;, &#8220;password&#8221;))
  
> req.Credentials = c
  
> Dim response As WebResponse = Nothing
  
> response = req.GetResponse()

Ao executar será retornado o seguinte erro:

> The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.

Ou seja, é necessário forçar a validação do certificado para dar a volta a isto. Como? Com a propriedade <a title="ServerCerificateValidationCallback" href="http://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.servercertificatevalidationcallback.aspx" target="_blank">ServerCertificateValidationCallback</a>.

Primeiro, criar uma função que indique que indique se vamos ou não aceitar o certificado (neste caso indico que será sempre aceite).

 

 

> Private Function ValidateCertificate(ByVal sender As Object, ByVal certificate As X509Certificate, ByVal chain As X509Chain, ByVal sslPolicyErrors As SslPolicyErrors) As Boolean
  
> Return True
  
> End Function

Por fim, adicionado ao Form_Load o seguinte:

<span style="font-size:x-small;"></p> 

<blockquote>
  <p>
    <span style="font-size:x-small;"><font size="2">ServicePointManager.ServerCertificateValidationCallback = New RemoteCertificateValidationCallback(AddressOf ValidateCertificate)</p> 
    
    <p>
      </font></span> 
    </p></blockquote> 
    
    <p>
      </span>o que fará com que aceite sempre os pedidos de validação do certificado.
    </p>