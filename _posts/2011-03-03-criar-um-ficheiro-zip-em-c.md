---
title: 'Criar um ficheiro Zip em C#'
date: 2011-03-03T18:33:01+00:00
author: tiagosalgado
layout: post
categories:
  - c#
tags:
  - .net
  - 'C#'
---
Uma forma rápida de criarmos um ficheiro Zip, é recorrendo à classe ZipPackage do WindowsBase.dll.

Para tal, precisamos de adicionar a referencia a esta dll ao nosso projecto, que no meu caso está em &#8220;C:Program Files (x86)Reference AssembliesMicrosoftFramework.NETFrameworkv4.0ProfileClientWindowsBase.dll&#8221;.

Podemos criar agora uma classe com um método para criar o ficheiro Zip com os ficheiros que queremos.

<pre style="color: #000000; background: #ffffff;"><span style="color: #800000; font-weight: bold;">public</span> <span style="color: #800000; font-weight: bold;">static</span> <span style="color: #800000; font-weight: bold;">void</span> CreateZipFile<span style="color: #808030;">(</span><span style="color: #800000; font-weight: bold;">string</span> zipFilename<span style="color: #808030;">,</span> List<span style="color: #808030;">&lt;</span><span style="color: #800000; font-weight: bold;">string</span><span style="color: #808030;">&gt;</span> files<span style="color: #808030;">,</span> CompressionOption compression<span style="color: #808030;">,</span> <span style="color: #800000; font-weight: bold;">bool</span> deleteFilesAfterZip<span style="color: #808030;">)</span>
        <span style="color: #800080;">{</span>
            <span style="color: #800000; font-weight: bold;">long</span> bsize <span style="color: #808030;">=</span> <span style="color: #008c00;">4096</span><span style="color: #800080;">;</span>
            <span style="color: #800000; font-weight: bold;">byte</span><span style="color: #808030;">[</span><span style="color: #808030;">]</span> b <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">new</span> <span style="color: #800000; font-weight: bold;">byte</span><span style="color: #808030;">[</span>bsize<span style="color: #808030;">]</span><span style="color: #800080;">;</span>
            <span style="color: #800000; font-weight: bold;">int</span> bytesRead <span style="color: #808030;">=</span> <span style="color: #008c00;"></span><span style="color: #800080;">;</span>
            <span style="color: #800000; font-weight: bold;">long</span> bytesWritten <span style="color: #808030;">=</span> <span style="color: #008c00;"></span><span style="color: #800080;">;</span>
            <span style="color: #800000; font-weight: bold;">using</span> <span style="color: #808030;">(</span>Package zip <span style="color: #808030;">=</span> System<span style="color: #808030;">.</span>IO<span style="color: #808030;">.</span>Packaging<span style="color: #808030;">.</span>Package<span style="color: #808030;">.</span>Open<span style="color: #808030;">(</span>zipFilename<span style="color: #808030;">,</span> FileMode<span style="color: #808030;">.</span>OpenOrCreate<span style="color: #808030;">)</span><span style="color: #808030;">)</span>
            <span style="color: #800080;">{</span>
                <span style="color: #800000; font-weight: bold;">foreach</span> <span style="color: #808030;">(</span><span style="color: #800000; font-weight: bold;">string</span> file <span style="color: #800000; font-weight: bold;">in</span> files<span style="color: #808030;">)</span>
                <span style="color: #800080;">{</span>
                    Uri uri <span style="color: #808030;">=</span> PackUriHelper<span style="color: #808030;">.</span>CreatePartUri<span style="color: #808030;">(</span><span style="color: #800000; font-weight: bold;">new</span> Uri<span style="color: #808030;">(</span>file<span style="color: #808030;">,</span> UriKind<span style="color: #808030;">.</span>Absolute<span style="color: #808030;">)</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
                    <span style="color: #800000; font-weight: bold;">if</span> <span style="color: #808030;">(</span>zip<span style="color: #808030;">.</span>PartExists<span style="color: #808030;">(</span>uri<span style="color: #808030;">)</span><span style="color: #808030;">)</span>
                        zip<span style="color: #808030;">.</span>DeletePart<span style="color: #808030;">(</span>uri<span style="color: #808030;">)</span><span style="color: #800080;">;</span>

                    PackagePart part <span style="color: #808030;">=</span> zip<span style="color: #808030;">.</span>CreatePart<span style="color: #808030;">(</span>uri<span style="color: #808030;">,</span> <span style="color: #800000;">"</span><span style="color: #800000;">"</span><span style="color: #808030;">,</span> compression<span style="color: #808030;">)</span><span style="color: #800080;">;</span>
                    <span style="color: #800000; font-weight: bold;">using</span> <span style="color: #808030;">(</span>FileStream stream <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">new</span> FileStream<span style="color: #808030;">(</span>file<span style="color: #808030;">,</span> FileMode<span style="color: #808030;">.</span>Open<span style="color: #808030;">,</span> FileAccess<span style="color: #808030;">.</span>Read<span style="color: #808030;">)</span><span style="color: #808030;">)</span>
                    <span style="color: #800080;">{</span>
                        <span style="color: #800000; font-weight: bold;">using</span> <span style="color: #808030;">(</span>Stream dest <span style="color: #808030;">=</span> part<span style="color: #808030;">.</span>GetStream<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #808030;">)</span>
                        <span style="color: #800080;">{</span>
                            <span style="color: #800000; font-weight: bold;">while</span> <span style="color: #808030;">(</span><span style="color: #808030;">(</span>bytesRead <span style="color: #808030;">=</span> stream<span style="color: #808030;">.</span>Read<span style="color: #808030;">(</span>b<span style="color: #808030;">,</span> <span style="color: #008c00;"></span><span style="color: #808030;">,</span> b<span style="color: #808030;">.</span>Length<span style="color: #808030;">)</span><span style="color: #808030;">)</span> <span style="color: #808030;">!</span><span style="color: #808030;">=</span> <span style="color: #008c00;"></span><span style="color: #808030;">)</span>
                            <span style="color: #800080;">{</span>
                                dest<span style="color: #808030;">.</span>Write<span style="color: #808030;">(</span>b<span style="color: #808030;">,</span> <span style="color: #008c00;"></span><span style="color: #808030;">,</span> bytesRead<span style="color: #808030;">)</span><span style="color: #800080;">;</span>
                                bytesWritten <span style="color: #808030;">+</span><span style="color: #808030;">=</span> bsize<span style="color: #800080;">;</span>
                            <span style="color: #800080;">}</span>
                        <span style="color: #800080;">}</span>
                    <span style="color: #800080;">}</span>
                <span style="color: #800080;">}</span>

            <span style="color: #800080;">}</span>

            <span style="color: #800000; font-weight: bold;">if</span> <span style="color: #808030;">(</span>deleteFilesAfterZip<span style="color: #808030;">)</span>
                DeleteFiles<span style="color: #808030;">(</span>files<span style="color: #808030;">)</span><span style="color: #800080;">;</span>
        <span style="color: #800080;">}</span>
</pre>

Adicionei um parametro (deleteFilesAfterZip) que vai remover todos os ficheiros que foram incluidos no Zip, apenas para me facilitar o trabalho, mas não é obrigatório.

A função DeleteFiles() será algo parecido com isto:

<pre style="color: #000000; background: #ffffff;"><span style="color: #800000; font-weight: bold;">private</span> <span style="color: #800000; font-weight: bold;">static</span> <span style="color: #800000; font-weight: bold;">void</span> DeleteFiles<span style="color: #808030;">(</span>List<span style="color: #808030;">&lt;</span><span style="color: #800000; font-weight: bold;">string</span><span style="color: #808030;">&gt;</span> files<span style="color: #808030;">)</span>
        <span style="color: #800080;">{</span>
            <span style="color: #800000; font-weight: bold;">foreach</span> <span style="color: #808030;">(</span><span style="color: #800000; font-weight: bold;">string</span> file <span style="color: #800000; font-weight: bold;">in</span> files<span style="color: #808030;">)</span>
            <span style="color: #800080;">{</span>
                FileInfo f <span style="color: #808030;">=</span> <span style="color: #800000; font-weight: bold;">new</span> FileInfo<span style="color: #808030;">(</span>file<span style="color: #808030;">)</span><span style="color: #800080;">;</span>
                <span style="color: #800000; font-weight: bold;">if</span> <span style="color: #808030;">(</span><span style="color: #808030;">!</span>f<span style="color: #808030;">.</span>Exists<span style="color: #808030;">)</span>
                    <span style="color: #800000; font-weight: bold;">throw</span> <span style="color: #800000; font-weight: bold;">new</span> FileNotFoundException<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>

                f<span style="color: #808030;">.</span>Delete<span style="color: #808030;">(</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
            <span style="color: #800080;">}</span>
        <span style="color: #800080;">}</span>
</pre>

Por fim, para chamarmos a função que cria o ficheiro Zip basta um simples

<pre style="color: #000000; background: #ffffff;">ZIP<span style="color: #808030;">.</span>CreateZipFile<span style="color: #808030;">(</span><span style="color: #800000;">"</span><span style="color: #0000e6;">c:xpto.zip</span><span style="color: #800000;">"</span><span style="color: #808030;">,</span> <span style="color: #800000; font-weight: bold;">new</span> List<span style="color: #808030;">&lt;</span><span style="color: #800000; font-weight: bold;">string</span><span style="color: #808030;">&gt;</span><span style="color: #808030;">(</span><span style="color: #808030;">)</span> <span style="color: #800080;">{</span> <span style="color: #800000;">"</span><span style="color: #0000e6;">c:xpto.txt</span><span style="color: #800000;">"</span><span style="color: #808030;">,</span> <span style="color: #800000;">"</span><span style="color: #0000e6;">c:xpto1.txt</span><span style="color: #800000;">"</span> <span style="color: #800080;">}</span><span style="color: #808030;">,</span> System<span style="color: #808030;">.</span>IO<span style="color: #808030;">.</span>Packaging<span style="color: #808030;">.</span>CompressionOption<span style="color: #808030;">.</span>Normal<span style="color: #808030;">,</span> <span style="color: #800000; font-weight: bold;">false</span><span style="color: #808030;">)</span><span style="color: #800080;">;</span>
</pre>

Referências:

<a href="http://msdn.microsoft.com/en-us/library/system.io.packaging.aspx" target="_blank">http://msdn.microsoft.com/en-us/library/system.io.packaging.aspx</a>

<a href="http://msdn.microsoft.com/en-us/library/system.io.packaging.zippackage.aspx" target="_blank">http://msdn.microsoft.com/en-us/library/system.io.packaging.zippackage.aspx</a>

<a href="http://weblogs.asp.net/jgalloway/archive/2007/10/25/creating-zip-archives-in-net-without-an-external-library-like-sharpziplib.aspx" target="_blank">http://weblogs.asp.net/jgalloway/archive/2007/10/25/creating-zip-archives-in-net-without-an-external-library-like-sharpziplib.aspx</a>