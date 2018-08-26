---
layout: post
title: "Configurando Toolset xp via linha de comando"
description: "Configuração de ambiente para compilação de projetos não suportado diretamente pelo Visual Studio"
data: 2018-06-02
author: "Fábio da Silva Santana"
category: programação
comments: true
tags: [c++, "visual studio"]
---

Compilar um projeto C/C++ para windows quando não tem um projeto criado para o visual studio as vezes pode ser um pouco complicado. O motivo é configurar o ambiente da maneira correta
no seu cmd com a versão certa do visual studio e suas flags, mas o time do Visual Studio não nos deixou na mão e disponibiliza junto com a ferramenta alguns scripts para ajudar a 
montar o ambiente. Veja o exemplo abaixo.

#### Configuração: Visual Studio 2015 x86  Windows SDK 10.0.16299.0

---

O script disponibilizado pelo visual studio é o vcvars<arquitetura_aqui>.bat. 

```
cd C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build 
...
30/05/2018  15:32                39 vcvars32.bat				  
30/05/2018  15:32                39 vcvars64.bat				 
30/05/2018  15:32             9.187 vcvarsall.bat				
30/05/2018  15:32                43 vcvarsamd64_x86.bat 		
30/05/2018  15:32                43 vcvarsx86_amd64.bat	
```

Usando o script `vcvarsall.bat` ele retorna o seguinte output.

```
.\vcvarsall.bat

output:

[ERROR:vcvarsall.bat] Error in script usage. The correct usage is:
Syntax:
    vcvarsall.bat [arch] [platform_type] [winsdk_version] [-vcvars_ver=vc_version]
where :
    [arch]: x86 | amd64 | x86_amd64 | x86_arm | x86_arm64 | amd64_x86 | amd64_arm | amd64_arm64
    [platform_type]: {empty} | store | uwp
    [winsdk_version] : full Windows 10 SDK number (e.g. 10.0.10240.0) or "8.1" to use the Windows 8.1 SDK.
    [vc_version] : {none} for default VS 2017 VC++ compiler toolset |
                   "14.0" for VC++ 2015 Compiler Toolset |
                   "14.1x" for the latest 14.1x.yyyyy toolset installed (e.g. "14.11") |
                   "14.1x.yyyyy" for a specific full version number (e.g. 14.11.25503)

The store parameter sets environment variables to support Universal Windows Platform application
development and is an alias for 'uwp'.

For example:
    vcvarsall.bat x86_amd64
    vcvarsall.bat x86_amd64 10.0.10240.0
    vcvarsall.bat x86_arm uwp 10.0.10240.0
    vcvarsall.bat x86_arm onecore 10.0.10240.0 -vcvars_ver=14.0
    vcvarsall.bat x64 8.1
    vcvarsall.bat x64 store 8.1

Please make sure either Visual Studio or C++ Build SKU is installed.

```

Usando o exemplo do usage do script podemos configurar o ambiente da seguinte maneira  `.\vcvarsall.bat 10.0.16299.0 -vcvars_ver=14.0` . Assim temos um ambinete pronto configurado para compilar usando o toolset do Visual Studio 2015.

#### Ajustando as confiugrações para usar support para XP

A maior dificuldade foi fazer o ambinete compilar com supporte ao XP. Fazendo algumas pesquisas achei dois valores importantes que devem ser marcados na hora de compilar.

* Nas flags de compilação é necessário setar `CFLAGS /D_USING_V110_SDK71_` . Ainda não encontrei muita informação sobre essa flag, mas ela foi a responsável por mudar meu toolset de v140 para v140_xp.

* Se Você só fizer isso e tentar rodar no xp vai dar um erro informando que o executável não é um programa. Então é necessário colocar na variável de link o seguinte valor `LDFLAGS /subsystem:console,5.01`. Assim informamos que nosso programa é um aplicativo de console e que vai rodar em x86. 

> NOTA: Sobre colocar as opções de configuração é sempre bom ficar atentendo se o gerador do Makefile colocou elas no devido lugar,já que cada projeto pode ser configurado de uma maneira, verifique o Makefile gerado se o CFLAGS está configurado devidamente e o LDFLAGS também

#### Conclusão

Usei essas configurações para fazer a compilação do OpenSSL para visual studio 2015 com suporte ao XP  e funcionou muito bem. Acredito que deva funcionar de maneira similar para os outros projetos também, mudando algumas configurações especificas dos próprios projetos.


#### Referências

1 - [Configurando Toolset](https://blogs.msdn.microsoft.com/vcblog/2012/10/08/windows-xp-targeting-with-c-in-visual-studio-2012/)
