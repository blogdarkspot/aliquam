---
layout: post
title: "Testando DLL Parte 1"
description: Injetando Mock na DLL
date: 2017-07-12 23:20:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tags: [ c++, visual studio, dll ]
---
## A motivação de tudo isso.

Voltando a trabalhar em um projeto antigo na atual empresa onde presto serviço, resolvi atacar os problemas dessa Issue de uma maneira mais profissional possível, usando unit test. Afinal no meu último projeto que eu estava trabalhando a falta de testes gerou muita dor de cabeça, senti na pele o que é bug resolvido aparecer novamente e isso é frustrante.

![charge tmux](../img/posts/unit-test-meme.jpeg)

## Sobre o Projeto.

O projeto é uma DLL que foi desenvolvida com o sdk do visual studio 2010 e meu objetivo era desenvolver uma lógica onde essa DLL tinha que ficar procurando arquivos específicos na máquina e enviando para o servidor.

## Dificuldade para gerar os Testes.

Como a DLL é um ambiente fechado onde o único ponto de entrada são aqueles que ela disponibiliza meu problema era: Como gerar um Mock que não é acessado diretamente pelos pontos de entrada da DLL. A idéia era simular o serviço funcionando e fazendo as buscas nas pastas e enviando para o servidor, mas isso está encapsulado pela DLL e eu não tenho acesso direto a esse objeto.

## Solução.

O único jeito que encontrei foi criar um ponto para injetar o mock dentro da DLL igual estou demonstrando no código abaixo:

 gist blogdarkspot/cd34538142d90e6015e006f2c9950ad4 EntryPoint.h 

 gist blogdarkspot/70b6381a108485cca2f873ff27357087 EntryPoint.cpp

Nota: Acabei omitindo a parte do GTest, pois estou assumindo que esse conhecimento já foi adquirido.

Como podemos ver no código acima foi criado um ponto de entrada chamado InjectMock que recebe um ponteiro void, isso porque estamos usando  o compilador de C que não entende classes. Na função StartDLL só vou tentar pegar uma nova instância se o serviço for NULL, então é importante SEMPRE chamar a função InjectMock antes de dar o start na DLL. Também adicionei uma função para Desativar a DLL.

## Conclusão.

Usando esse ponto de entrada esse vai ser o único código extra que você vai precisar usar para injetar o Mock na sua DLL. Isso vai facilitar a parte de simulação de testes sem a necessidade de ter que colocar um monte de código extra para debugar e testar tudo.
