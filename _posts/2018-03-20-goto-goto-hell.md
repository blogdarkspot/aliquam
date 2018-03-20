---
layout: post
title: "goto? goto hell?"
description: "Uma breve análise do goto e por que não se deve usar...(brincadeira)"
date: 2018-03-20
author: "Fábio da Silva Santana"
category: programação
comments: true
tags: [c++]
---


Uma coisa interessante sobre a época de faculdade é que tinha muito bar e junto com ele muitas discussões sobre assunto do qual não entendiamos nada ou tinhamos pouco conhecimento pelo fato de estar inciando em uma nova carreira e ser meros estudantes. O uso do `goto` era um desses assuntos polêmico e vinha seguido de um mantra que era "não use pois não é uma boa prática de programação... ohmmmmmm". 

Agora quero compartilhar o seguinte exemplo de implmenteção de um método:

~~~c++

/**
 * versão com lambda
 */
void EstacaoRecebimento::findItem()
{
    auto errorMSG = [&]()
    {
        QMessageBox::critical(this, tr("Error"),
                              tr("Item %1 não pode ser encontrado").arg(m_ui->leItem->text()), QMessageBox::Ok);
    };

    if(m_model != nullptr && m_ui->leItem->text().size() > 0)
    {
        if(m_ui->cbRemover->isChecked())
        {
            if(!m_model->checkIfSubItemMetadataExists(m_ui->leItem->text()))
                errorMSG();
	    else
                m_model->removeItem(m_ui->leItem->text());
        }
        else
        {
            if(!m_model->findItemAndSubItens(m_ui->leItem->text()))
                errorMSG();
	    else
                subItemFound(true);
        }
    }

    m_ui->cbRemover->setChecked(false);
    m_ui->leItem->clear();
}


/**
* versão com goto, não use se quiser seguir as boas práticas
*/
void EstacaoRecebimento::findItem()
{
    if(m_model != nullptr && m_ui->leItem->text().size() > 0)
    {
        if(m_ui->cbRemover->isChecked())
        {
            if(!m_model->checkIfSubItemMetadataExists(m_ui->leItem->text()))
                goto ERROR;

            m_model->removeItem(m_ui->leItem->text());
            goto DONE;
        }
        else
        {
            if(!m_model->findItemAndSubItens(m_ui->leItem->text()))
                goto ERROR;

            subItemFound(true);
            goto DONE;
        }
    }

ERROR:
    QMessageBox::critical(this, tr("Error"),
                          tr("Item %1 não pode ser encontrado").arg(m_ui->leItem->text()), QMessageBox::Ok);
DONE:
    m_ui->cbRemover->setChecked(false);
    m_ui->leItem->clear();
}
~~~

Comparando as duas versões a com goto é bem mais fluida, você não necessita voltar no código para entender o que está acontecendo e sendo assim para mim e para o revisor dos meus PR's a melhor versão para se ler e compreender o código.

Não é por que muitas pessoas no passado usaram o recurso de uma maneira ruim que ele é um mau recurso. Em vez de usar os péssimos exemplos e ficar repetindo mantras sem questioná-los, podemos procurar bons exemplos e aprender como usar os recursos de maneira mais eficiente.
