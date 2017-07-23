---
layout: post
title: "Testando DLL parte 2"
description: "Classe Fake e Métodos chamando Métodos"
date: 2017-07-17 23:58:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tags: [ c++, visual studio, dll ]
---

## Descrição do problema.

No meu [último post](https://blogdarkspot.github.io/Inject-mock-dll) eu falei sobre como injetar um mock dentro de uma dll para testar um código fechado. Agora o problema é outro. Essa minha dll tem uma thread que chama vários métodos da classe para executar uma lógica. Então agora temos o problema que é métodos que chamam métodos, como resolver isso, já que os métodos mocks não tem implementação nenhuma?

![test-yoo](../img/posts/you-need-some-tests-yo.jpg)

## Minha tática para atacar o problema.

Por incresça que parivel eu achei uma parte da solução do problema na documentação… Sim, isso é sério, as vezes ler a documentação ajuda. Existe um tópico que fala sobre a criação de classes fakes para executar alguma lógica mais complexa que só o mock não trata.

gist blogdarkspot/7ce44306dfb3b8630839fba00a1eddcb google_mock_example.h 

Com essa solução acima consegui achar uma luz no fim do mock. Posso criar uma classe fake e implementar a thread para simular o funcionamento.

gist blogdarkspot/a01d70abac7822bd1b18c8d92ef73214 mock_call_mock_one.h

Então eu basicamente repliquei a solução exemplo do google mocks para o meu projeto, porém ainda existe mais um problema, preciso chamar os outros métodos do mock normal dentro da classe Fake. Para resolver esse problema eu passei o ponteiro do meu mock para dentro da classe Fake.

 gist blogdarkspot/6d3bf61142f86d21d4079649774d3cf5 mock_call_mock_final.h 

Agora eu adicionei mais um método dentro da classe fake com o intuito de passar uma referência do mock instanciado, assim é possível chamar o mock dentro da classe fake e isso resolve o problema de chamar métodos dentro de métodos com mock sem precisar em alterar em nada a lógica do programa original.

## Conclusão

Fazer teste com mock no começo é bem complicado, mas usando algumas técnicas simples para expor o mínimo possível sua implementação ajuda bastante. Nesse caso eu tenho uma Interface que foi criada somente para poder fazer essa ponte entre o Mock e o Objeto Original, outro motivo também de criar essa interface foi simplificar ao máximo as dependências do projeto de teste eu poderia fazer isso com a classe normal, mas teria que colocar um monte de lib dentro do projeto teste onde não será utilizado.



