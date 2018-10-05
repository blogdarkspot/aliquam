---
layout: post
title: "Subprocesso e herança de handle"
description: "Comportamento da herança de handle no windows xp e outras versões"
date: 2018-10-01 22:01:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tag: [Windows, C] 
---

No Windows quando queremos criar um processo filho, usamos a função CreateProcess.
 Essa função tem muitas configurações, uma dela é a possibilidade de herdar handles do processo pai através do parâmetro `bInheritHandles` setado para `TRUE`.

 ### O problema do bInheritHanddles == TRUE

 Quem desenvolve para windows em C/C++ sabe que nas configurações do projeto podemos escolher se dará ou não suporte ao windows XP, e no Windows XP essa funcionalidade pode gerar um pouco de dor de cabeça. 
 O problema é que na época do Windows XP não existia a liberdade de escolha de qual Handle poderia ou não ser herdado pelo subprocesso, 
 e sem esse controle comportamentos inesperados podem ocorrer em ambientes de multithread, como descrito nesse [artigo por Raymond Chen em 2011](https://blogs.msdn.microsoft.com/oldnewthing/20111216-00/?p=8873/){:target="_blank"}.

 O que acontece é, se um `processo A` cria uma nova thread e cria um `subprocesso B` com herança de Handles, em seguida cria um novo `subprocesso C` também com herança de Handles, o `subprocesso C` ira herdar os Handles que foram herdados pelo `subprocesso B` podendo gerar um comportamento inesperado no ciclo de vida do objeto a qual o Handle esta referenciando, igual no exemplo a seguir;

 Vamos imaginar que temos um servidor TCP/IP que a cada nova conexão aceita cria um subprocesso para fazer uma tarefa, responde para o cliente e depois termina, esse servidor trabalha com multithread.