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

~~~CMake
cmake_minimum_required(VERSION 3.0)
project(Nome_do_projeto)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE AUTORCC ON)
set(CMAKE_CXX_STANDARD 14)
find_package(Qt5 COMPONENTS Core Quick)
add_executable(Executable_name main.cpp layouts/layouts.qrc)
target_link_libraries(Executable_name Qt5::Core Qt5::Quick)
~~~

Na primeira linha a configuração do CMake é marcada como 3.0 no mínimo, pois segundo a documentação do [Qt5](https://doc.qt.io/qt-5/cmake-manual.html){:target="_blank"} se for uma versão menor é necessário uma configuração adicional. Na terceira linha é marcadada a variável [CMAKE_INCLUDE_CURRENT_DIR](https://cmake.org/cmake/help/v3.0/variable/CMAKE_INCLUDE_CURRENT_DIR.html){:target="_blank"} como enable para incluir no diretório de build os paths das seguintes variavéis ${CMAKE_CURRENT_SOURCE_DIR} e ${CMAKE_CURRENT_BINARY_DIR}. 



