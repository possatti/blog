---
title: "ZettaJS"
description: "Introdução a internet das coisas, e exemplo de aplicação utilizando o Zetta."
date: 2015-03-31 01:11:36
tags: [IoT, javascript]
comments: true
share: true
---

![](/images/zetta-logo.svg)

Recentemente fiz um trabalho, na faculdade, sobre Internet das Coisas. Para esse trabalho eu tive de escrever um documento explicando sobre o assunto, e também o Zetta, que é uma plataforma para a Internet das Coisas. Além de desenvolver uma pequena aplicação que ilustrasse o tema. Então resolvi aproveitar para escrever um post sobre isso.

A maior parte do que eu escrevi aqui, já era parte do meu trabalho. Mas eu aperfeiçoei alguns trechos para que algumas coisas ficassem mais claras, e o texto ficasse mais fácil de acompanhar.

Eu irei começar falando sobre Internet das Coisas, de forma geral. E em seguida vou apresentar o Zetta, explicando para que serve e quais os seus pontos fortes e fracos. E no final, demonstrarei como colocar em execução uma aplicação do Zetta que eu desenvolvi.

## Introdução a Internet das Coisas

Um conceito que tem ganhado popularidade mais recentemente é o de [Internet das Coisas][IoT] (IoT, da sigla em inglês para "Internet of Things"). Esse conceito diz respeitos a objetos embarcados com dispositivos eletrônicos que podem se conectar através de uma rede e trocar informações entre si.

Em Internet das Coisas, as "coisas" podem ser inúmeros tipos de dispositivos. Como equipamentos para monitoramento de batimentos cardíacos, luminosidade, velocidade, controladores, luzes, motores, veículos, e muitas outras coisas. Esses dispositivos tem a capacidade de coletar informações úteis e deixá-las fluir entre os outros dispositivos conectados, de forma autônoma. Tudo isso ficará mais claro ao executar a aplicação que desenvolvi.

Para a aplicação do conceito de IoT, existem muitas plataformas e ferramentas. Como, por exemplo: Carriots, Xively, ZettaJS, ThingSpeak, Zatar e muitas outras. Mas aqui eu irei tratar apenas do Zetta.

## Sobre o Zetta

O Zetta é uma plataforma Open Source construída em cima do [Node.js][node] para criar servidores para aplicação de Internet das Coisas. Tendo seu primeiro commit feito no dia 18 de abril de 2014 em seu [repositório oficial][zetta-repo], o Zetta é uma plataforma relativamente nova, mas em constante desenvolvimento. Em sua [página principal][zetta], o Zetta declara utilizar uma API [RESTful][rest], [WebSockets][websockets] e [Programação Reativa][reactive-programming] para conectar diversos dispositivos e proporcionar a criação de aplicações com intensa manipulação de dados, em tempo real.

Como eu disse, o Zetta funciona em cima do Node.js (ou apenas Node), que é uma plataforma construída usando o V8, o Runtime de JavaScript do Google Chrome. O Node é útil para construir servidores web de forma rápida e fácil. E a linguagem JavaScript é utilizada para a programação.

É possível executar um servidor Zetta em uma variedade de dispositivos, desde um PC qualquer, até um Arduíno, ou um Raspberry Pi. Assim o servidor pode se comunicar com diversos equipamentos conectados, como LEDs, sensores, telas e basicamente tudo com o qual o seu dispositivo puder se comunicar.

Os desenvolvedores do Zetta, desenvolveram também o Zetta Browser. Que é uma aplicação web que pode ser utilizada para navegar através da API do Zetta. Assim, é possível apontá-lo para o endereço de um servidor Zetta e visualizar em tempo real todos os dados sendo coletados por aquele servidor. Bem como interagir com os dispositivos conectados a ele. Isso tudo em uma interface agradável e intuitiva. Contudo, o browser é um pouco instável, e as vezes apresenta algumas falhas. Mas, no geral, é uma ferramenta muito útil.

O Zetta objetiva proporcionar uma maneira fácil e rápida de projetar aplicações para a Internet das Coisas. E proporcionando flexibilidade para controlar detalhes da aplicação.

Porém um dos seus pontos fracos está na sua data de origem. Tendo surgido por volta do início de 2014, é de se esperar que não seja uma plataforma muito madura ainda. Ao navegar pelo site oficial, é fácil perceber pontos em que falta documentação, ou links que te levam a artigos completamente vazios. Ainda assim, a documentação no site é suficiente para iniciar o aprendizado. Porém, provavelmente chegará uma hora em que você sentirá falta de algumas informações, assim como eu senti.

Ainda assim, o Zetta aparenta ser uma plataforma simples, robusta e estável. É possível utilizá-lo para realizar uma enormidade de projetos. E é uma ótima escolha para aqueles que estão iniciando com a Internet das Coisas, ou apenas querem matar a curiosidade.

Outra vantagem para o Zetta é que, diferente de muitas plataformas com propósitos puramente comerciais, esse é um projeto Open Source (utilizando a licensa MIT) e sem fins comerciais (apesar de que você pode, sim, usá-lo para criar aplicações comerciais). E é um projeto que está em constante desenvolvimento.

Mas a falta de uma grande empresa por trás do Zetta pode dificultar o seu uso em aplicações de larga escala. Por falta, por exemplo, de um suporte dedicado exclusivamente a atender seus clientes e a falta de garantia de funcionamento.

## Arquitetura do Zetta

A arquitetura de uma aplicação Zetta é basicamente a seguinte: existe um servidor Zetta central executando na nuvem (no Heroku, por exemplo), que está conectado a alguns Hubs (como um Raspberry Pi ou um Beaglebone) que rodam instâncias locais de um servidor Zetta. Esses Hubs por sua vez estão conectados a equipamentos eletrônicos e sensores (como exemplo, uma célula fotovoltaica e uma LED). Vejam isso ilustrado na **INCRÍVEL** figura que eu fiz:

![Arquitetura do Zetta](/images/arquitetura-do-zetta.jpg)

Dessa maneira, dados são obtidos dos diversos equipamentos e sensores e levados aos servidores dos hubs, que os encaminha ao servidor central. Isso funciona de forma geo-distribuída, então é possível que, por exemplo, um sensor fotovoltaico lá no japão faça com que uma lâmpada acenda aqui no Brasil.

Essa lógica, de como as coisas acontecem e em função do quê, está toda nos servidores, podendo estar até mesmo distribuída entre os hubs e o servidor central.

## Demonstração

Para exemplificar o uso do Zetta, eu desenvolvi uma aplicação simples, baseada no [tutorial para iniciantes][hello-zetta] disponível no site oficial. E adicionalmente, incrementei com uma funcionalidade a mais (controlar o player do meu PC), para exemplificar a criação de drivers.

O exemplo disponibilizado no site ensina a criar um servidor local que irá servir para ilustrar uma aplicação que controla a luminosidade de um local. No exemplo é utilizado uma mock LED juntamente com uma mock photocell (célula voltaica), de maneira que quando a photocell detecta uma baixa luminosidade, a LED acende. E quando a photocell detecta elevada luminosidade, a LED apaga.

Para aqueles que não estão familiarizados com o termo, "mock" nesse contexto, quer dizer que não há hardware sendo utilizado. Nem para a photocell, e nem para a LED. Os drivers utilizados procuram simular os comportamentos desses dispositivos. Por isso, é necessário apenas um computador para executar o projeto e ver tudo funcionando.

Apesar do exemplo, parecer algo simples demais, este exemplo serve bem para ilustrar uma possível aplicação na vida real. Para fazer algo mais útil, você poderia colocar uma célula voltaica verdadeira no telhado da sua casa, e no lugar de uma LED, você usaria algumas lâmpadas da sua casa. Dessa forma, quando a noite chegasse, as lâmpadas da sua casa acenderiam automaticamente. E quando o dia chegasse, elas apagariam, sem a necessidade de qualquer intervenção.

Se você fosse utilizar um dispositivo real qualquer, conectado ao seu hub, então você precisaria de um driver específico que fizesse a comunicação entre o servidor local e o dispositivo. Há uma variedade de drivers já criados para vários dispositivos e plataformas diferentes. Mas se você não encontrar um que atenda sua necessidade, poderá desenvolver o seu próprio. No projeto que eu desenvolvi, eu exemplifiquei a criação de um driver simples. Porém dependendo da sua necessidade, um pouco de conhecimento de eletrônica poderá ser necessário. Algo que eu não sei, e não tenho como ensinar.

No projeto, além da funcionalidade básica de controle de luminosidade, eu desenvolvi um driver que pudesse controlar o player de mídia do meu computador (o [Banshee][banshee]). Infelizmente, devido a forma como eu desenvolvi o driver, essa última parte irá funcionar apenas em sistemas operacionais Linux. E que tenham o Banshee instalado... é claro!

O driver permite pausar a reprodução e voltar a reproduzir a faixa atual. Além de permitir pular para a próxima faixa ou voltar a anterior.

Imaginando o contexto de Internet das Coisas, podemos imaginar várias aplicações. A minha localização poderia ser monitorada pelo GPS do meu celular, por exemplo. E quando eu estivesse voltando do trabalho, ao chegar próximo de casa, uma música poderia começar a tocar automaticamente em casa. E da mesma forma, parasse quando eu estivesse saindo.

## Execução do projeto

Todo o [código da aplicação][github-server] está hospedado no github. E também o [driver para o Banshee][github-driver]. Para executar o projeto, entre com os comandos abaixo na sua linha de comando. Você precisará instalar no seu computador o [Node][node] e o [NPM][npm] (que geralmente já vem com o Node).

```bash
# Clone o projeto
$ git clone https://github.com/possatti/sample-zettajs-server.git
# Entre no diretório baixado
$ cd sample-zettajs-server
# Instale as dependências necessárias (inclusive o driver
# para o banshee)
$ npm install
# Inicie o servidor:
$ npm start
```

Se você não tiver o git instalado em seu computador. Não há problema. Você pode baixar o [projeto no github][zetta-server-zip] e continuar com `npm install` e `npm start`.

Com isso, o servidor irá iniciar em localhost, na porta 1337, por padrão. E você, de imediato, irá notar, no log da linha de comando, que a LED está acendendo e apagando toda hora. Isso está correto, a célula fotovoltaica está ocasionando esse comportamento.

Para visualizar tudo o que está acontecendo, eu recomendo o uso do Zetta Browser. Para abrir o Zetta Browser apontando para o seu servidor local (127.0.0.1:1337), entre com o seguinte endereço no seu navegador:

http://browser.zettajs.io/#/overview?url=http://127.0.0.1:1337

Mas lembre que isso que você está visualizando é tudo local, apenas. Para realmente ser capaz de acessar este hub através da internet, o servidor local precisa se conectar a um servidor na nuvem. Coisa que esse projeto já está configurado para fazer. Ao ter iniciado o servidor local, ele automaticamente já se conectou ao servidor em hello-zetta.herokuapp.com (isto está configurado no código do projeto). Este servidor do [Heroku][heroku] é disponibilizado pela equipe desenvolvedora do Zetta, para que iniciantes possam usá-lo. Então qualquer um pode criar sua aplicação e se conectar a ele.

Para acessar a API do servidor local através da internet, use o Zetta Browser apontando para o servidor do Heroku:

http://browser.zettajs.io/#/overview?url=http://hello-zetta.herokuapp.com

Com essa URL, você poderá usar o seu celular, por exemplo, para visualizar a LED e a photocell. Além de poder interagir com o Banshee, se você o tiver instalado.

Mas, por favor, tenha em mente, que como esta é apenas uma aplicação de demonstração, nenhuma etapa de autenticação é feita. Isto quer dizer que qualquer pessoa em qualquer parte do mundo pode acessar a API que o seu servidor local está provendo. Apesar de que isso não é tão alarmante, pois as únicas coisas que eles podem fazer é controlar a mock LED e o Banshee no seu computador.

## E agora?

Para aprender mais, você pode continuar interagindo com o servidor, e investigar o código, para ver como o projeto funciona. Conhecimentos de NodeJS e JavaScript com certeza irão lhe ajudar. Para aprender mais sobre o Zetta, um bom ponto de partida é o seu próprio [site][zetta], apesar de que, como eu disse, a documentação do projeto falha em alguns pontos.

E se estiver interessado, você pode tentar construir sua própria aplicação, segundo uma necessidade ou um desejo que você tem. Comesse com algo simples e vá aperfeiçoando a medida que desejar.

[zetta]: http://www.zettajs.org/
[zetta-repo]: https://github.com/zettajs/zetta/
[hello-zetta]: http://www.zettajs.org/projects/2014/10/13/Hello-World.html

[node]: https://nodejs.org/
[npm]: https://www.npmjs.com/
[banshee]: http://banshee.fm/
[heroku]: https://heroku.com/

[rest]: http://pt.wikipedia.org/wiki/REST
[IoT]: http://pt.wikipedia.org/wiki/Internet_das_Coisas
[reactive-programming]: http://en.wikipedia.org/wiki/Reactive_programming
[websockets]: http://pt.wikipedia.org/wiki/WebSocket

[github-server]: https://github.com/possatti/sample-zettajs-server/
[github-driver]: https://github.com/possatti/zetta-banshee-driver
[zetta-server-zip]: https://github.com/possatti/sample-zettajs-server/archive/v0.1.zip
