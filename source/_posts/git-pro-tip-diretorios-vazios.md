---
title: "Git Pro Tip: Diretórios vazios"
date: 2015-03-25 21:05:29
tags: [git, protip]
comments: true
share: true
---

![](/images/git-logo.png)

Nesse post vou falar sobre algo que me aborrecia quando comecei a usar o git. E que pode trazer dificuldades para outros que estejam iniciando com o git. Falarei sobre o fato de que ele não registra diretórios vazios no repositório.

## O problema

Vamos supor a seguinte situação. Você tem um repositório do git, e acabou de criar um diretório vazio dentro dele. E ao tentar gravar suas mudanças, o git simplesmente não detecta que você criou um novo diretório. Como no exemplo:

```bash
# Cria um diretório vazio
$ mkdir novo_dir
# Revela o que há no diretório atual
$ ls
novo_dir
# Verifica o status do repositório
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

Como podem ver, no exemplo, o diretório vazio foi criado. Mas quando usamos o comando `git status`, ele simplesmente não detecta que um novo diretório foi criado.

A razão disso é simples. O git registra apenas arquivos e não diretórios. Se a minha memória não me falha, no SVN você poderia criar diretórios vazios no repositório. Mas não é o caso do git. Pois tudo o que ele busca, para gravar, são arquivos. Então ele nem mesmo se preocupa com o fato de que você criou um novo diretório. Mas se você colocasse um arquivo ali dentro, aí sim, ele detectaria o arquivo e consequentemente o diretório que o contém.

```bash
# Cria um arquivo no novo diretório
$ touch novo_dir/exemplo.txt
# Verifica o status do repositório
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	novo_dir/

nothing added to commit but untracked files present (use "git add" to track)
```

Vejam que neste último comando ele detectou o diretório, mas somente por causa do arquivo. O que ele irá gravar, na realidade, é o arquivo. E uma das informações relevantes para gravar o arquivo é onde ele está.

## Porque isso me incomodaria?

Alguns podem estranhar, e se perguntar: "Mas e daí? O diretório está vazio mesmo!". Hoje eu concordo com isso. Mas no começo isso me atrapalhava um pouco, quando eu estava começando a usar o git.

Como, por exemplo, quando eu estava iniciando um novo projeto e usava uma ferramenta para gerar o esqueleto do meu projeto. Assim, a ferramenta criava uma porção de diretórios onde eu deveria colocar meus arquivos. Mas se eu ainda não tivesse desenvolvido nada ainda, eu teria que deixá-los vazio por algum tempo.

Isso costumava acontecer comigo quando eu criava um novo projeto do [maven][maven] pelo eclipse. O eclipse criava várias pastas vazias, segundo a estrutura de um projeto padrão do maven. E essa estrutura era importante, porque ditava a forma como eu deveria organizar meu projeto. Se eu colocasse um teste no diretório errado, por exemplo, o maven não iria incorporá-lo a build. E por esse motivo, as coisas precisavam estar em seu devido lugar.

E quando eu tentava gravar as alterações no repositório, o git simplesmente não detectava as novas pastas que haviam sido criadas, apenas os arquivos, como o `pom.xml`. Então se eu fosse usar um outro computador, e tivesse que clonar o projeto, ele não viria com a estrutura de pastas que eu precisava. Então eu teria que resgatar tudo da minha memória, ou gastar algum tempo procurando na internet como a estrutura deveria ser.

## Solução

A verdade é que não existe uma solução elegante para isso. É uma característica do git que você deve se acostumar. E com o tempo, essa característica realmente passa a fazer sentido. Afinal de contas, para que gravar um diretório vazio? ... Ele está vazio! Então não estamos perdendo nada.

Mas como eu expliquei, em algumas situações você pode querer subir um diretório vazio intencionalmente. Se esse for o seu caso, há algumas alternativas que podemos recorrer. Há uma [questão no Stack Overflow][stackoverflow-question] que tem excelentes sugestões de como fazer isso. E eu vou descrever aqui duas delas que considero mais importantes.

Uma das possíveis soluções, é que você crie um arquivo vazio dentro da pasta, chamado `deletar-depois`, `placeholder`, ou qualquer coisa do gênero. O importante é que fique claro para você, e para sua equipe, que esse arquivo deve ser deletado assim que o diretório for preenchido com algum conteúdo importante.

Outra alternativa é para o caso de você querer **forçar** com que o diretório esteja **sempre** vazio (sabe se lá, porque você iria querer isso xD ). Para esta situação, você pode criar um [`.gitignore`][gitignore] dentro da pasta, com o seguinte conteúdo:

```
# Ignora tudo neste diretório
*
# Exceto este arquivo
!.gitignore
```

Nessa última alternativa, ninguém nunca irá conseguir gravar um arquivo nessa pasta! A não ser que o `.gitignore` seja apagado, posteriormente. Isso pois o git irá ignorar todos os arquivos ali dentro, com exceção do `.gitignore`.

Dada essas duas soluções, eu encerro aqui. Espero que essa dica seja útil. E se tiverem dúvidas, ou quiserem compartilhar alguma experiência própria, podem escrever aqui nos comentários.

[maven]: https://maven.apache.org/
[stackoverflow-question]:  http://stackoverflow.com/questions/115983/how-can-i-add-an-empty-directory-to-a-git-repository
[gitignore]: http://git-scm.com/docs/gitignore
