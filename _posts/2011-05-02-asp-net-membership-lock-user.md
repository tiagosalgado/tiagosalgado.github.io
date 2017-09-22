---
title: ASP.NET Membership Lock User
date: 2011-05-02T13:58:23+00:00
author: tiagosalgado
layout: post
categories:
  - asp.net
tags:
  - asp.net
---
Visto que não temos uma forma directa de indicar que determinado utilizador irá estar bloqueado, a única forma que arranjei para o fazer foi forçar o erro no login múltiplas vezes, até atingir o valor máximo de tentativas definidas no atributo “maxInvalidPasswordAttempts”.

Quando usamos o Membership do ASP.NET, no web.config teremos algo como:

<add name="AspNetSqlMembershipProvider" 

type="System.Web.Security.SqlMembershipProvider" 

connectionStringName="MyAppConnectionString" 

enablePasswordRetrieval="false" 

enablePasswordReset="true" 

requiresQuestionAndAnswer="false" 

requiresUniqueEmail="false" 

maxInvalidPasswordAttempts="5" 

minRequiredPasswordLength="6" 

minRequiredNonalphanumericCharacters="0" 

passwordAttemptWindow="10" applicationName="MyAppName" />

O código que utilizo é então o seguinte:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:812469c5-0cb0-4c63-8c15-c81123a09de7:7b992993-3bdb-4e84-a36b-2b56774e9723" class="wlWriterEditableSmartContent">
  <pre name="code" class="c#">public static bool LockUser(MembershipUser user)
{
    try
    {
        for (int i = 0; i &lt; Membership.MaxInvalidPasswordAttempts; i++)
            Membership.ValidateUser(user.UserName, "thisisandummypasswordonlytolocktheuser");

        return user.IsLockedOut;
    }
    catch (Exception)
    {
        throw;
    }
            
}</pre>
</div>

Espero que seja util.