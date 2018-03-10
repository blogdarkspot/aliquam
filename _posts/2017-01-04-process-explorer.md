---
layout: post
title: "Analise de Processo com Process Explorer"
description: "Encontrado o processo na ferramenta (Parte I)"
date: 2016-11-06 21:20:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tags: [ c++, windows]
---

# Introdução

Vamos começar com uma definição bem simples e talvez nada formal sobre o que é um processo no Windows. Processo é um container que contém um conjunto de recursos quando uma instância de um programa é executada. 

Segue uma figurinha abaixo para o post ficar mais bonitinho.

![imagem-1](../img/posts/2018-01-04-process-explorer/image-1.png)


Olhando para essa imagem bonitinha podemos ver alguns desses recursos que existem dentro de um processo. 

# Ferramenta de Análise

Process Explorer é uma ferramenta foda pra caralho que te passa todo tipo de informação que tu precisa e não precisa sobre os processos que estão executando atualmente em sua máquina, ex: 

DLL que foram carregadas, HANDLE, Threads, Processos filhos, ID do processo, estado do processo e toda uma caralhada de coisas. Você pode adquirir essa ferramenta [aqui](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer).

Segue uma fotinha dele para o post continuar mais bonitinho.

![imagem-1](../img/posts/2018-01-04-process-explorer/image-2.png)

# O programa

Como foi dito na introdução um processo é uma instância de um programa em execução. Como eu sou hackudo, vou escrever meu próprio programa e postar um exemplo dele aqui.

~~~c++
#include "stdafx.h"
#include <iostream>

int main()
{
	int finish = 0;
	std::cout << "Running" << std::endl;
	std::cin >> finish;

    return 0;
}
~~~

Pronto. Agora só compilar esse programa foda para gerar um executável e assim podemos criar uma instância desse programa através desse *.exe.


![imagem-1](../img/posts/2018-01-04-process-explorer/image-3.png)

Com o programa em execução é possível encontrar ele no Process Explorer através do nome do executável. No meu caso o nome do executável é ProcessExperiments.exe, ele está marcado em amarelo na foto abaixo.

![imagem-1](../img/posts/2018-01-04-process-explorer/image-4.png)

Uma maneira de localizar o processo na árvore de processos é clicar e segura no ícone de alvo do menu, a janela do Process Explorer irá sumir e o ícone do mouse irá virar um alvo que você deve arrasta para cima da janela do programa que você deseja localizar o processo.

Após localizar o processo, já temos algumas informações logo de cara, como: PID, uso de CPU, Memória, processos filhos e pais, etc.

# Resumo

O Process Explorer é uma ferramenta potente para análise de processos em execução auxiliando e muito o administrador e desenvolvedor de sistemas. 

Irei fazer uma série de posts baseado nas ferramentas de SysInternals do Windows  me baseando na Literatura Windows Internals. Por esse motivo não vou detalhar muito o uso dessa ferramenta nesse momento e vou explorando ela aos poucos conforme a necessidade.
