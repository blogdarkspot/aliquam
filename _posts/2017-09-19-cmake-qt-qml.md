---
layout: post
title: "CMake, QT e QML"
description: "Criando projeto QT com QML usando CMake"
date: 2017-09-19 23:58:00
author: "Fábio da Silva Santana"
category: programacao
comments: true
tags: [ c++, cmake, qt, qml ]
---




Esse é provavelmente o primeiro de alguns outros posts a respeito de configurar um projeto usando CMake. Nesse artigo será as configurações básicas para compilar um projeto usando CMake,QT e QML, depois testes e por último gerar instaladores.

## Configurações Mínimas do CMake

~~~cmake
cmake_minimum_required(VERSION 3.0)
project(Nome_do_projeto)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE AUTORCC ON)
set(CMAKE_CXX_STANDARD 14)
find_package(Qt5 COMPONENTS Core Quick REQUIRED)
add_executable(Executable_name main.cpp layouts/layouts.qrc)
target_link_libraries(Executable_name Qt5::Core Qt5::Quick)
~~~

Na terceira linha é marcadada a variável [CMAKE_INCLUDE_CURRENT_DIR](https://cmake.org/cmake/help/v3.0/variable/CMAKE_INCLUDE_CURRENT_DIR.html){:target="_blank"} como enable para incluir no diretório de build os paths das seguintes variavéis ${CMAKE_CURRENT_SOURCE_DIR} e ${CMAKE_CURRENT_BINARY_DIR}. A quarta linha roda o [moc](http://doc.qt.io/qt-4.8/moc.html){:target="_blank"} automaticamente quando necessário. Para adicionar [Qt Resource System](doc.qt.io/qt-5/resources.html){:target="_blank"} é necessário ativar o [CMAKE_AUTORCC](https://cmake.org/cmake/help/v3.0/prop_tgt/AUTORCC.html){:target="_blank"}, se isso não for marcado como ON os arquivos .qml não serão carregados na hora da compilação. A partir da versão 5.7 do Qt é necessário compilar com suporte ao C++11, com antes da versão 3.1.0 do CMake isso não é marcado por default é possível fazer isso através da variável [CMAKE_CXX_STANDARD](https://cmake.org/cmake/help/v3.1/prop_tgt/CXX_STANDARD.html){:target="_blank"} onde é possível setar 98, 11 ou 14. [find_package](https://cmake.org/cmake/help/v3.0/command/find_package.html){:target="_blank"} é um recurso interessante que ajuda a procurar os includes necessários, para o qml é necessário usar o [QT Quick](http://doc.qt.io/qt-5/qtquick-index.html){:target="_blank"}. Também é necessário adicionar todos os arquivos .qrc junto do [ADD_EXECUTABLE](https://cmake.org/cmake/help/v3.0/command/add_executable.html?highlight=add_executable){:target="_blank"}.

