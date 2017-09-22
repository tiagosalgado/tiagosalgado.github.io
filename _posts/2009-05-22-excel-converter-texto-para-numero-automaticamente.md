---
title: 'Excel &#8211; Converter Texto para Número automáticamente'
date: 2009-05-22T13:22:43+00:00
author: tiagosalgado
layout: post
categories:
  - others
tags:
  - asp
  - excel
  - macros
  - microsoft
  - software
---
Já por várias vezes me aconteceu ter que importar um ficheiro em Excel para uma tabela no SQL Server e perder dados na importação devido a células que deveriam ser números, estarem assumidas como texto.

Quando isto acontece, o Excel dá-nos essa indicação e permite-nos converter esse mesmo texto para número.

[<img class="aligncenter size-full wp-image-397" title="Excel_Numero_Para_Texto" src="http://oito.files.wordpress.com/2009/05/excel_numero_para_texto.png" alt="Excel_Numero_Para_Texto" width="356" height="282" />](http://oito.files.wordpress.com/2009/05/excel_numero_para_texto.png)

Quando temos poucos dados, esta correcção torna-se fácil e não perdemos muito tempo. O problema é quando temos um ficheiro com milhares de linhas e dezenas de colunas e temos que andar à procura de todas as células que possam ter esse erro.

Para me resolver esse problema, criei uma Macro que vai percorrer todas as células no intervalo definido e verificar se existe este erro, e caso exista vai corrigi-lo.

Para saber que determinada célula está neste estado, as funções IsText() e IsNumeric() terão que retornar um valor verdadeiro, e para corrigir basta alterar o valor da célula para o valor actual 🙂

A Macro pode ser algo como isto:

> Sub ConverterTextoParaNumero()
  
> On Error GoTo Erro
> 
> For Each celula In Range(&#8220;A:A&#8221;)
  
> If IsNumeric(Range(celula.Address)) And Application.WorksheetFunction.IsText(Range(celula.Address)) Then
  
> celula.Interior.ColorIndex = 36
  
> celula.Value = celula.Value
  
> End If
  
> Next celula
  
> MsgBox &#8220;Processo terminado&#8221;
> 
> Exit Sub
  
> Erro:
  
> MsgBox &#8220;Erro no processo&#8221;
  
> End Sub

Este código vai percorrer todo o intervalo definido e alterar todas as células no estado de &#8220;Número copiado como texto&#8221; para um fundo amarelo e corrigir o erro.