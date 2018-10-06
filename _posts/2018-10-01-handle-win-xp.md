---
layout: post
title: "Controle de herança de Handle em subprocessos"
description: "Controle de herança de Handles"
date: 2018-10-01 22:01:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tag: [Windows, C] 
---

Na criação de um processo filho no Windows existe a possíbilidade do processo pai compartilhar ou não algum dos seus Handles com o processo filho. 
Esse comportamento de compartilhar ou não é controlado pelo paramêtro `bInheritHandles` na criação do processo filho. Por padrão todos os Handles
herdáveis serão herdados, esse comportamento pode ser modificado,  mas somente da versão vista em diante.

Vamos observar o comportamento de herança do Handles usando um hipotético servidor TPC/IP com as seguintes caracteriticas: Multithread; cria um novo subprocesso a cada nova sessão; o processo pai recebe a reposta do filho através do Handle output da struct `STARTUPINFO`; após a execução do subprocessso, o servidor retorna a resposta para o cliente e fecha a sessão.

### Comportamento padrão

#### Single Thread
A cada nova sessão iniciada um Handle `device/afd` é criado pelo kernel para a gerenciar a transferência de pacotes do socket TCP referenciando um `Objeto A` e um Handle `File` em `STARTUPINFO` para fazer a parte de IPC entre  pai e filho referenciando um `Objeto B`, então durante a execução do pai e filho ambos terão Handles que referenciam os objetos `A` e `B`. Quando o processo filho é finalizado os Handles que foram duplicados para ele serão destruídas e no final da sessão os do pai também serão.

#### Multithread
Com multithread o comportamento é igual a single thread na maioria dos casos, porem existe um cenário que o comportamento pode ser indejeado; Se enquanto tivermos uma sessão em andamento outra for crianda antes da finalização da anterior a sessão seguinte irá herdar os Handles que foram herdados pelo primeiro subprocesso, então teremos um "vazamento de handles" indesejado o que pode ocasionar do objeto que aquele Handle esta referenciando não ser destruído no momento que deveria.

#### 
### Referências

[artigo por Raymond Chen em 2011](https://blogs.msdn.microsoft.com/oldnewthing/20111216-00/?p=8873/){:target="_blank"}.
[CreateProcessA](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessa){:target="_blank"}.
[Device AFD](https://www.codemachine.com/article_tdi.html){:target="_blank"}.
[IPC](https://docs.microsoft.com/en-us/windows/desktop/ipc/interprocess-communications){:target="_blank"}.
[Object e Handle](https://docs.microsoft.com/en-us/windows/desktop/sysinfo/handles-and-objects){:target="_blank"}.