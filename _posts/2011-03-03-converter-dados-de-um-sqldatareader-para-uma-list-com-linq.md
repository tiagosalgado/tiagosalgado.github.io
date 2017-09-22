---
title: Converter dados de um SqlDataReader para uma List com LINQ
date: 2011-03-03T10:36:37+00:00
author: tiagosalgado
layout: post
categories:
  - c#
---
<div class="posterous_autopost">
  <p>
    Recorrendo ao SqlDataReader, facilmente conseguimos retornar dados de uma BD para a nossa aplicação.<br /> Um exemplo muito rápido da sua utilização poderia ser algo como:
  </p>
  
  <pre style="color: #000000; background: #ffffff;"><span style="color: #800000; font-weight: bold;">public</span> <span style="color: #800000; font-weight: bold;">class</span> Person
    <span style="color: #800080;">{</span>
        <span style="color: #800000; font-weight: bold;">public</span> <span style="color: #800000; font-weight: bold;">string</span> Nome <span style="color: #800080;">{</span> get<span style="color: #800080;">;</span> set<span style="color: #800080;">;</span> <span style="color: #800080;">}</span>
        <span style="color: #800000; font-weight: bold;">public</span> <span style="color: #800000; font-weight: bold;">int</span> Idade <span style="color: #800080;">{</span> get<span style="color: #800080;">;</span> set<span style="color: #800080;">;</span> <span style="color: #800080;">}</span>
    <span style="color: #800080;">}</span>
</pre>
  
  <pre style="color: #000000; background: #ffffff;"><span style="color: #800000; font-weight: bold;">public</span> List<span style="color: #808030;">&lt;</span>Person<span style="color: #808030;">&gt;</span> GetPersons<span style="color: #808030;">(</span><span style="color: #808030;">)</span>
        <span style="color: #800080;">{</span>
            List<span style="color: #808030;">&lt;</span>Person<span style="color: #808030;">&gt;</span> persons <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">new</span> List<span style="color: #808030;">&lt;</span>Person<span style="color: #808030;">&gt;</span><span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
            <span style="color: #800000; font-weight: bold;">using</span> <span style="color: #808030;">(</span>SqlCommand cmd <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">new</span> SqlCommand<span style="color: #808030;">(</span><span style="color: #800000;">"</span><span style="color: #0000e6;">SELECT * FROM Persons</span><span style="color: #800000;">"</span><span style="color: #808030;">,</span> <span style="color: #800000; font-weight: bold;">new</span> SqlConnection<span style="color: #808030;">(</span><span style="color: #800000;">"</span><span style="color: #0000e6;">a_minha_connectionstring</span><span style="color: #800000;">"</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span>
            <span style="color: #800080;">{</span>
                cmd<span style="color: #808030;">.</span>Connection<span style="color: #808030;">.</span>Open<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
                <span style="color: #800000; font-weight: bold;">using</span> <span style="color: #808030;">(</span>SqlDataReader dr <span style="color: #808030;">=</span> cmd<span style="color: #808030;">.</span>ExecuteReader<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span>
                <span style="color: #800080;">{</span>
                    <span style="color: #800000; font-weight: bold;">if</span> <span style="color: #808030;">(</span><span style="color: #808030;">!</span>dr<span style="color: #808030;">.</span>HasRows<span style="color: #808030;">)</span>
                        <span style="color: #800000; font-weight: bold;">return</span> <span style="color: #800000; font-weight: bold;">null</span><span style="color: #800080;">;</span>
                    <span style="color: #800000; font-weight: bold;">while</span> <span style="color: #808030;">(</span>dr<span style="color: #808030;">.</span>Read<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span>
                    <span style="color: #800080;">{</span>
                        persons<span style="color: #808030;">.</span>Add<span style="color: #808030;">(</span><span style="color: #800000; font-weight: bold;">new</span> Person<span style="color: #808030;">(</span><span style="color: #808030;">)</span>
                        <span style="color: #800080;">{</span>
                            Nome <span style="color: #808030;">=</span> dr<span style="color: #808030;">[</span><span style="color: #800000;">"</span><span style="color: #0000e6;">Nome</span><span style="color: #800000;">"</span><span style="color: #808030;">]</span><span style="color: #808030;">.</span>ToString<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #808030;">,</span>
                            Idade <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">int</span><span style="color: #808030;">.</span>Parse<span style="color: #808030;">(</span>dr<span style="color: #808030;">[</span><span style="color: #800000;">"</span><span style="color: #0000e6;">Idade</span><span style="color: #800000;">"</span><span style="color: #808030;">]</span><span style="color: #808030;">.</span>ToString<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span>
                        <span style="color: #800080;">}</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
                    <span style="color: #800080;">}</span>
                    <span style="color: #800000; font-weight: bold;">return</span> persons<span style="color: #800080;">;</span>
                <span style="color: #800080;">}</span>
            <span style="color: #800080;">}</span>
        <span style="color: #800080;">}</span>
</pre>
  
  <p>
    Nada de mal com este código, pois criamos uma lista que vai ter o tipo Person, e vamos adicionando no ciclo while() várias instâncias da classe Person.
  </p>
  
  <p>
    Mas podemos torna-lo muito mais simples recorrendo ao LINQ e à Interface IDataRecord. Assim, o método GetPersons() seria algo como:
  </p>
  
  <pre style="color: #000000; background: #ffffff;"><span style="color: #800000; font-weight: bold;">public</span> List<span style="color: #808030;">&lt;</span>Person<span style="color: #808030;">&gt;</span> GetPersons<span style="color: #808030;">(</span><span style="color: #808030;">)</span>
        <span style="color: #800080;">{</span>
            <span style="color: #800000; font-weight: bold;">using</span> <span style="color: #808030;">(</span>SqlCommand cmd <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">new</span> SqlCommand<span style="color: #808030;">(</span><span style="color: #800000;">"</span><span style="color: #0000e6;">SELECT * FROM Persons</span><span style="color: #800000;">"</span><span style="color: #808030;">,</span> <span style="color: #800000; font-weight: bold;">new</span> SqlConnection<span style="color: #808030;">(</span><span style="color: #800000;">"</span><span style="color: #0000e6;">a_minha_connectionstring</span><span style="color: #800000;">"</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span>
            <span style="color: #800080;">{</span>
                cmd<span style="color: #808030;">.</span>Connection<span style="color: #808030;">.</span>Open<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
                <span style="color: #800000; font-weight: bold;">return</span> <span style="color: #808030;">(</span>from IDataRecord p <span style="color: #800000; font-weight: bold;">in</span> cmd<span style="color: #808030;">.</span>ExecuteReader<span style="color: #808030;">(</span><span style="color: #808030;">)</span>
                        select <span style="color: #800000; font-weight: bold;">new</span> Person<span style="color: #808030;">(</span><span style="color: #808030;">)</span>
                        <span style="color: #800080;">{</span>
                            Nome <span style="color: #808030;">=</span> p<span style="color: #808030;">[</span><span style="color: #800000;">"</span><span style="color: #0000e6;">Nome</span><span style="color: #800000;">"</span><span style="color: #808030;">]</span><span style="color: #808030;">.</span>ToString<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #808030;">,</span>
                            Idade <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">int</span><span style="color: #808030;">.</span>Parse<span style="color: #808030;">(</span>p<span style="color: #808030;">[</span><span style="color: #800000;">"</span><span style="color: #0000e6;">Idade</span><span style="color: #800000;">"</span><span style="color: #808030;">]</span><span style="color: #808030;">.</span>ToString<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span>
                        <span style="color: #800080;">}</span><span style="color: #808030;">)</span><span style="color: #808030;">.</span>ToList<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
            <span style="color: #800080;">}</span>
        <span style="color: #800080;">}</span>
</pre>
  
  <p>
    Muito mais claro, menos código, e o resultado será igual.
  </p>
</div>