---
layout: post
title: "Serializando dados de maneira prática, eficiente e rápida"
description: ""
date: 2016-11-06 21:20:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tags: [ c++, protobuff ]
---
Pelo titulo parece que o assunto desse artigo é como fazer as coisas de maneira prática rápida e eficiente, porém essas são as características desse tipo de serialização de dados (O processo, só é pratico e eficiente depois que você se acostuma).

A alguns meses atrás estava me aventurando em um projeto em python na empresa que presto serviço e foi onde eu conheci esse método, conhecido como Protocol Buffer.

## O que é o Protocol Buffer? Por que eu utilizaria isso?

Protocol Buffer é uma linguagem criada pela Google que tem a função de serializar estrutura de dados. Parecido como escrever um arquivo XML e depois enviar ele, só que o proto buffer é muito mais rápido (20 a 100 vezes), menor (3 a 10 vezes) e você utiliza ele como uma classe que é muito mais fácil de programar, que que ter que ficar navegando naqueles nós do XML, como você acessar os dados via classe o código fica também mais legível. Abaixo temos um exemplo de declaração de um arquivo Proto Buffer que tem a extensão `.proto`.

gist blogdarkspot/d8d3f62bf5d2a9a9ffec87be6d24cbe2 addressbook.proto


De cara já podemos ver que essa declaração é muito mais amigável e intuitiva que um arquivo XML (pelo menos para mim é). A primeira linha é importante já que atualmente existe duas versões do Protocol Buffer (proto2 e proto3), caso linha syntax seja omitida o compilador irá usar o proto2 como default.

A segunda linha temos o package, isso serve para evitar conflito com outros projetos.

Em seguida temos a declaração message que é um conjunto de campos que define nossa estrutura, esses campos é formando pelo seu tipo e um par chave-valor. Existe vários tipos como int32, int64, float, string, boolean, também é possível criar enuns e usar outras messages como tipo também. A parte chave valor é o nome do campo e um inteiro que deve ser único, isso auxilia na hora da serialização quando a estrutura vira um binário. Os campos no [Proto2](https://developers.google.com/protocol-buffers/docs/proto) e [Proto3](https://developers.google.com/protocol-buffers/docs/proto3) são declarados de maniera diferente, você pode encontrar toda a descrição no site oficial.

Após a definição das estrturas é necessário compilar esse arquivo, com o compilador [protoc](https://github.com/google/protobuf) nesse ponto você poderá escolher para qual linguagem sua estrutura será compilada. Atualmente o Proto3 aceita as linguagens da tabela abaixo:

| linguagem   |
|-------------|
| C++         |
| Java        |
| Python      |
| Objective-C |
| C#          |
| JavaNano    |
| JavaScript  |
| Ruby        |
| Go          |
| PHP         |


Comando de compilação:

```
protoc -I=$SRC_DIR --cpp_out=$DST_DIR $SRC_DIR/addressbook.proto
```

O primeiro parametro é o caminho para o source da sua aplicação, `--cpp_out` é o destino do arquivo compilado(para gerar uma compilação para python por exemplo é só trocar --cpp_out para --python_out) e o ultimo parametro é o seu arquivo `.proto`.


Assim que a compilação ocorrer com sucesso será gerado os arquivos necessários para a implementação da classe baseado na estrutura definida no arquivo proto e a linguagem escolhida, minha escolha foi o C++ e os arquivos gerados foram `addressbook.pb.h` e `addressbook.pb.cc`. Incluindo esses arquivos no meu projeto eu posso manipular a estrutura definida de maneira bem simples e legível.

A seguir um exemplo de utilização do protocol buffer.

Programa de escrita de arquivo

 gist blogdarkspot/f2a3e51d79690d3f60c61e983875855d write.cc 

Programa de leitura de arquivo.

 gist blogdarkspot/c16e63303875dae19207601546196db6 read.cc


##Conclusão

Protocol Buffer é mais fácil de entender na parte do código, mais rápido na hora do parse e tem um tamanho reduzido em comparação ao binário de um XML, mas como o próprio site da Google comenta em alguns pontos o protocol buffer não é tão vantajoso como trabalhar trabalhar junto com HTML por exemplo e ele não é tão compreensível se você tentar olhar a classe gerada pelo compilador, somente a estrutura do arquivo .proto é bem legível. Em projetos que envolvem comunicação backend e precisa de um bom desempenho ele se mostrou bem favorável!.



#Referências

[Protocol Buffer](https://developers.google.com/protocol-buffers/)

