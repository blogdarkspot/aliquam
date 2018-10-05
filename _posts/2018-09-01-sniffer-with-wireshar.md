---
layout: post
title: "Usando Wireshark para monitorar a rede"
description: "Wireshark como ferramenta de auxílio no Debug de programas"
date: 2018-09-01 10:50:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tag: [Debug, Tools]
---

Essa semana me deparei com um bug um pouco chato de resolver. Os dados aparentemente saem do
 servidor, mas não chegam ao cliente. Debugando o código do servidor e cliente, não era possível encontrar um error logo de cara. 
 Para ter mais detalhes do problema, pensei em observar a transferência de dados do servidor para o cliente. 
 Lembrei do [Wireshark](https://www.wireshark.org/){:target="_blank"}, usei ela algumas vezes para monitorar trafego em usb. 

## Iniciando o Wireshark

Na página inicial do Wireshark 2.6.3 teremos as interfaces disponiveis para monitorar, imagem 1. 
Inicie a caputra dos pacotes selecionando a interface deseja e clicando na barbatana azul, imagem 2.

 ![Wireshark](../img/posts/2018-09-01-wireshark/wireshark_one.PNG)

 ![Wireshark_2](../img/posts/2018-09-01-wireshark/wireshark_two.PNG)

## Filtrando as informações

Se olharmos a imagem 2 toda informação que passa pela interface é capturada, mas podemos reduzir a quantidade de informações 
 usando ![Filtros](https://wiki.wireshark.org/DisplayFilters){:target="_blank"}. 

 ![Wireshark_3](../img/posts/2018-09-01-wireshark/wireshark_three.PNG)

Na imagem acima temos o seguinte filtro `ip.src == 192.168.164.129 and tcp.srcport == 3000 `, que pode ser interpretado da seguite forma; Moste os pacotes que são enviados pelo `ip 192.168.164.129` através da `porta tcp 3000`.

> Observação: Filtros é um assunto bem extenso, para mais informações de como criar veja   
 ![aqui](https://wiki.wireshark.org/DisplayFilters){:target="_blank"}.

 ## Entendendo em tela

Após fazer o filtro e alguns requests, nossa tela será populada com informações divididas em 3 espaços que são: 

- *Package List* com o histórico de envios e recebimentos dos pacotes;

- *Package Details* com informações detalhadas de algum pacote selecionado no package list;

- *Package Data* que mostra os bytes que foram enviados com o pacote.

 ![Wireshark_4](../img/posts/2018-09-01-wireshark/wireshark_four.PNG)


## Conclusão

Wireshark é uma ferramenta ótima para observar a transferência de dados, podendo usar ela como uma ferramenta complementar de debug.







