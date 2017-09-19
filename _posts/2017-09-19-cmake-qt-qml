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

~~~c++
using ::testing::_;
using ::testing::Invoke;

class Foo {
 public:
  virtual ~Foo() {}
  virtual char DoThis(int n) = 0;
  virtual void DoThat(const char* s, int* p) = 0;
};

class FakeFoo : public Foo {
 public:
  virtual char DoThis(int n) {
    return (n > 0) ? '+' :
        (n < 0) ? '-' : '0';
  }

  virtual void DoThat(const char* s, int* p) {
    *p = strlen(s);
  }
};

class MockFoo : public Foo {
 public:
  // Normal mock method definitions using Google Mock.
  MOCK_METHOD1(DoThis, char(int n));
  MOCK_METHOD2(DoThat, void(const char* s, int* p));

  // Delegates the default actions of the methods to a FakeFoo object.
  // This must be called *before* the custom ON_CALL() statements.
  void DelegateToFake() {
    ON_CALL(*this, DoThis(_))
        .WillByDefault(Invoke(&fake_, &FakeFoo::DoThis));
    ON_CALL(*this, DoThat(_, _))
        .WillByDefault(Invoke(&fake_, &FakeFoo::DoThat));
  }
 private:
  FakeFoo fake_;  // Keeps an instance of the fake in the mock.
};
~~~

Com essa solução acima consegui achar uma luz no fim do mock. Posso criar uma classe fake e implementar a thread para simular o funcionamento.

~~~c++
class ActiveMQManagerInterface
{

public:

	virtual void connectToServer() = 0;
	virtual void sendMessageToServer() = 0;
	virtual bool isConnected() = 0;
	virtual bool start() = 0;
	virtual bool stop() = 0;
	virtual void addQueue(const std::string&) = 0;
	virtual ~ActiveMQManagerInterface()
	{
	}
};

class ActiveMQManagerFake : public ActiveMQManagerInterface {

public:
	virtual void connectToServer() {
	};

	virtual void sendMessageToServer() {
	};

	virtual bool isConnected() {
		return true;

	};

	virtual bool start() {
		return true;
	};

	virtual bool stop() {
		return true;
	};

	virtual void addQueue(const std::string&) {
	};
};


class ActiveMQManagerMock : public ActiveMQManagerInterface {

private:
	ActiveMQManagerFake fake;
public:
	MOCK_METHOD0(getInstance, ActiveMQManagerInterface*());
	MOCK_METHOD0(isConnected, bool());
	MOCK_METHOD0(start, bool());
	MOCK_METHOD0(stop, bool());
	MOCK_METHOD1(addQueue, void(const string&));	
	MOCK_METHOD0(connectToServer, void());
	MOCK_METHOD0(sendMessageToServer, void());
  
  void delegateToFake()
	{
		ON_CALL(*this, connectToServer()).WillByDefault(Invoke(&fake, &ActiveMQManagerFake::connectToServer));
	}
	
};
~~~

Então eu basicamente repliquei a solução exemplo do google mocks para o meu projeto, porém ainda existe mais um problema, preciso chamar os outros métodos do mock normal dentro da classe Fake. Para resolver esse problema eu passei o ponteiro do meu mock para dentro da classe Fake.

~~~c++
class ActiveMQManagerFake : public ActiveMQManagerInterface {
private:
	ActiveMQManagerInterface* mock;
public:

	void setMock(ActiveMQManagerInterface* mock) {
		if (mock != NULL) {
			this->mock = mock;
		}
	}

	virtual void connectToServer() {
		if (mock)
		{
			mock->sendMessageToServer();
			mock->stop();
		}
	};

	virtual void sendMessageToServer() {
	};

	virtual bool isConnected() {
		return true;

	};

	virtual bool start() {
		if (mock)
		{
			if (mock->isConnected())
			{
				mock->sendMessageToServer();
			}
			else
			{
				mock->connectToServer();
				if (mock->isConnected())
				{
					mock->sendMessageToServer();
				}
			}
			return true;
		}
		return false;
	};

	virtual bool stop() {
		return true;
	};

	virtual void addQueue(const std::string&) {
	};

};

class ActiveMQManagerMock : public ActiveMQManagerInterface {

private:
	ActiveMQManagerFake fake;
public:
	MOCK_METHOD0(getInstance, ActiveMQManagerInterface*());
	MOCK_METHOD0(isConnected, bool());
	MOCK_METHOD0(start, bool());
	MOCK_METHOD0(stop, bool());
	MOCK_METHOD1(addQueue, void(const string&));
	MOCK_METHOD0(connectToServer, void());
	MOCK_METHOD0(sendMessageToServer, void());

	void delegateToFake()
	{
		fake.setMock(this);
		ON_CALL(*this, start()).WillByDefault(Invoke(&fake, &ActiveMQManagerFake::start));
	}
	
};
~~~

Agora eu adicionei mais um método dentro da classe fake com o intuito de passar uma referência do mock instanciado, assim é possível chamar o mock dentro da classe fake e isso resolve o problema de chamar métodos dentro de métodos com mock sem precisar em alterar em nada a lógica do programa original.

## Conclusão

Fazer teste com mock no começo é bem complicado, mas usando algumas técnicas simples para expor o mínimo possível sua implementação ajuda bastante. Nesse caso eu tenho uma Interface que foi criada somente para poder fazer essa ponte entre o Mock e o Objeto Original, outro motivo também de criar essa interface foi simplificar ao máximo as dependências do projeto de teste eu poderia fazer isso com a classe normal, mas teria que colocar um monte de lib dentro do projeto teste onde não será utilizado.



