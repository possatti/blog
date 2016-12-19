---
title: "Git Subtrees"
description: "Eu explico como usar o comando Git Subtree que pode ser usado para importar um repositório Git para dentro de outro repositório Git."
tags:
  - git
comments: true
share: true
thumbnail: /images/git-logo.png
date: 2016-12-19 12:05:00
---

Vamos supor que você deseja importar um repositório Git para dentro de outro repositório Git. Como você faria isso? Vou te dizer que você tem duas opções: Submodules, ou Subtrees.

Os dois tem suas vantagens e desvantagens. A primeira vez que eu caí nessa necessidade, optei por Submodules, porque foi a primeira opção que encontrei, e Subtrees pareciam complicados. Hoje a minha opinião é o inverso disso. Submodules são fáceis de iniciar no repositório, mas difíceis de manter (principalmente se você trabalha com outras pessoas). Enquanto Subtrees parecem mais complicados quando você bate o olho, mas na realidade são muito mais fáceis de manter. Isso tudo é minha opnião, mas buscando na internet percebi que esse é o consenso.

E não vou afirmar que Subtrees são melhores que Submodules em todas as situações. **Com certeza não são.** Mas eu acredito que boa parte das vezes eles são a melhor opção.

Uma coisa a esclarecer é que o nome Subtree vêm de uma [estratégia de *merge*][subtree-merge] do Git, chamada Subtree (Woow). Que envolve fazer um *merge* aplicado a um diretório específico. E é exatamente essa estratégia de *merge* que é usada pelo `git subtree`. Mas a partir de agora, sempre que eu falar "Subtree", estou me referindo ao comando `git subtree`.

[subtree-merge]: https://git-scm.com/book/en/v1/Git-Tools-Subtree-Merging

Nos exemplos abaixo, nós vamos trabalhar com dois repositórios Git. Então para ficar claro, vou explicar como vou me referir a cada um deles. Quando eu falar "repositório *container*", estou me referindo ao repositório mais abrangente, que vai conter a Subtree e todo o resto do seu código que não faz parte da Subtree. E quando eu falar "sub-repositório", estou me referindo ao repositório que têm os arquivos que você quer puxar para dentro do repositório *container* (a Subtree).

O Git tem um comando `git subtree` integrado, porém talvez você se surpreenda com o fato de que ele não é um comando nativo como `git pull`, `git merge` e etc. Ele é um script criado pela comunidade que com o tempo ficou popular o suficiente para ser incluído dentro do Git. É possível usar Subtrees [sem usar o comando `git subtree`][mastering-subtrees], mas é um pouco mais difícil.

[mastering-subtrees]: https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec#.qp8ny5efo


## Usando Subtrees

Existem quatro comandos importantes quando lidamos com Subtrees (na verdade três):

<!-- Escrever também sobre o `git subtree split` e `git subtree merge`. Ou talvez só apontar que eles existem. -->

```sh
git remote add <subrepo-remoto> <subrepo-url>
git subtree add --prefix <subrepo-path> <subrepo-remoto> master --squash
git subtree pull --prefix <subrepo-path> <subrepo-remoto> master --squash
git subtree push --prefix <subrepo-path> <subrepo-remoto> master

# <subrepo-remoto>: Nome do remoto para o sub-repositório. (Exemplo: assets-do-joao)
# <subrepo-url>: Url do sub-repositório (Exemplo: github.com/joao/super-assets)
# <subrepo-path>: Diretório onde você vai "jogar" os arquivos do sub-repositório. (Exemplo: src/assets)
```

Subtrees são algo relativamente novo para mim, mas eu estou usando há algum tempo no trabalho e vou falar do que entendi. Primeiro eu recomendo que os três últimos comandos sejam executados sempre da raiz do repositório *container*. Assim você não se perde colocando o caminho errado. E o primeiro comando é apenas para simplificar as operações seguintes. E repare que aqui eu estou usando o *branch* `master` para o sub-repositório. Mas se você tem uma necessidade diferente, troque o nome do *branch*.

Agora vamos ver cada um dos comandos com mais detalhes:

```sh
git remote add <subrepo-remoto> <subrepo-url>
```

O comando `git remote add` adiciona o sub-repositório aos remotos do nosso repositório *container*. (Sem nenhum mistério até aqui.)

<!-- Lembrando que isso tem que ser feito para cada máquina que vai mexer com a Subtree. -->

```sh
git subtree add --prefix <subrepo-path> <subrepo-remoto> master --squash
```

`git subtree add` inicia uma Subtree no nosso repositório. Deve ser usado uma única vez (a primeira). Ele puxa todos os *commits* do sub-repositório e comprime eles em um único *commit* (*squash*) e faz um *merge* desse *commit* com o nosso repositório. Após fazermos um `git push` do nosso repositório *container*, todos os arquivos da Subtree estarão vivendo dentro do nosso repositório no servidor. Então qualquer pessoa que fizer `git clone` ou `git pull` vai ter tudo pronto já, e não vai precisar nem se estressar em saber o que são Subtrees.

```sh
git subtree pull --prefix <subrepo-path> <subrepo-remoto> master --squash
```

`git subtree pull` puxa todos os *commits* do sub-repositório, comprime em um único *commit* (*squash*) e faz *merge* com nosso repositório. Após fazer `git push` todo mundo terá as alterações.

```sh
git subtree push --prefix <subrepo-path> <subrepo-remoto> master
```

`git subtree push` irá "caçar" todos os *commits* do nosso repositório *container* que mexem em alguma coisa no diretório da Subtree (`<subrepo-path>`); vai criar um novo *branch* temporário somente com esses *commits*; e vai mandar os *commits* desse *branch* para o servidor do sub-repositório. E agora a pessoa que mantém o sub-repositório terá nossas alterações.

A princípio pode ser difícil de entender o que o `git subtree push` está fazendo, mas você pode usar o comando [`git subtree split`][so-subtree-push] para ter uma ideia mais detalhada do que realmente está acontecendo.

[so-subtree-push]: http://stackoverflow.com/a/12819896


## Recomendações

Quando você estiver trabalhando no repositório *container* e mexer em alguma coisa do sub-repositório paralelamente a alterações nos arquivos do repositório *container*, separe as coisas. Faça primeiro um *commit* contendo as alterações que pertencem a Subtree, e depois um *commit* com as alterações do repositório *container*. Isso não é 100% necessário, pois o comando `git subtree` vai conseguir dar conta de qualquer situação, mas ajuda na sua organização, pois as mensagens de cada *commit* ficarão mais intuitivas.

Use sempre a barra do Unix `/` (eu nunca me lembro qual é a barra, e qual é a contra barra T.T). Mesmo no Windows, use `/` para separar os diretórios. Uma vez eu usei com "a barra do Windows" (`\`), e aparentemente tinha funcionado, mas logo depois tive problemas. Então fica a dica.


## Concluindo

Se você quiser uma outra leitura, para entender melhor o assunto, eu recomendo que você leia o post da Atlassian sobre Subtrees chamado [*The Power of Subtree*][the-power-of-subtree]. Além do post mais aprofundado de Christophe Porteneuve: [*Mastering Git subtrees*][mastering-subtrees]. Esses dois materiais me ajudaram muito.

[the-power-of-subtree]: https://developer.atlassian.com/blog/2015/05/the-power-of-git-subtree/