---
title: "Git Pro Tip: Diretórios vazios"
date: 2015-03-25 21:05:29
tags: [git, protip]
comments: true
---

Nesse post vou falar sobre algo que me aborrecia quando comecei a usar o git. E que pode trazer dificuldades para outros que estejam iniciando com o git. Falarei sobre o fato de que ele não registra diretórios vazios no repositório.

## O problema

Vamos supor a seguinte situação. Você tem um repositório do git, e acabou de criar um diretório vazio dentro do repositório, e ao tentar gravar suas mudanças, o git simplesmente não detecta que você criou um novo diretório. Como no exemplo:

```bash
$ mkdir dir_vazio
$ ls -a
.  ..  dir_vazio  .git
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

Como podem ver, no exemplo, o diretório vazio foi criado. Mas quando usamos o comando `git status`, ele simplesmente não detecta que um novo diretório foi criado.

A razão disso é simples. O git busca apenas arquivos e não diretórios. Se a minha memória não me falha, no SVN você poderia criar diretórios vazios no repositório. Mas não é o caso do git. Pois tudo o que ele busca, para gravar, são arquivos.

## Porque isso me incomodaria?

Alguns podem estranhar, e se perguntar: "Mas e daí? O diretório está vazio mesmo!". Hoje eu concordo com isso. Mas no começo isso me atrapalhava um pouco, quando eu estava começando a usar o git.

Vou demonstrar uma situação em que isso pode ser um incomodo. Quando você está iniciando um novo projeto e você usa uma ferramenta para gerar o esqueleto do seu projeto. Assim, a ferramenta cria uma porção de diretórios onde você deve colocar seus arquivos.

Isso acontecia comigo quando eu criava um novo projeto do [maven][maven] pelo eclipse. O eclipse criava várias pastas vazias, segundo a estrutura de um projeto padrão do maven.

**E essa estrutura era importante, porque ditava a forma como eu deveria organizar meu projeto. Se eu colocasse um teste no diretório errado, por exemplo, o maven não iria incorporá-lo a build. Era necessár**

**Assim, não temos como salvar a estrutura de diretórios no repositório. E quando tivermos que usar uma outra máquina e consequentemente clonarmos o projeto, ele não virá com a estrutura que precisamos. Assim nós teríamos que puxar da nossa mente, como a estrutura do projeto era e recriar na mão os diretórios (não sei se o maven, pode fazer algo para nos ajudar neste caso, mas é só um exemplo).**

**Eu tive esse problema, e quando eu ia criar um arquivp de testes, eu precisava salva-lo em `src/test/`, mas eu não lembrava isso de cabeça. Então, eu tive que procurar pela internet e descobrir onde os testes deveriam ser colocados para se integrarem a build do projeto.**

## Solução

A verdade é que não há solução para isso. É uma característica do git que você deve se acostumar. Com o tempo, essa caracteŕitica passa a fazer sentido. Afinal de contas, para que gravar um diretório vazio? ... Ele tá vazio! Então não estamos perdendo nada.

Mas como eu expliquei, em algumas situações você pode querer subir um diretório vazio intencionalmente. Se esse for o seu caso, há algumas alternativas que podemos recorrer. Há uma [questão no Stack Overflow][stackoverflow-question] que tem excelentes sugestões de como fazer isso.

Uma delas que eu sugiro, é que você crie um arquivo vazio dentro da pasta, chamado `deletar-depois`.
Ou se é um diretório que você quer forçar que esteja *sempre* vazio, você pode criar um [`.gitignore`][gitignore] dentro da pasta, com o seguinte conteúdo:

```
# Ignora tudo neste diretório
*
# Exceto este arquivo
!.gitignore
```

Assim o git irá ignorar todos os arquivos dentro dessa pasta, com exceção do `.gitignore`.

Com isso eu encerro este post. Se tiverem dúvidas, ou quiserem compartilhar alguma experiência própria, podem colocar nos comentários.

[maven]: https://maven.apache.org/
[stackoverflow-question]:  http://stackoverflow.com/questions/115983/how-can-i-add-an-empty-directory-to-a-git-repository
[gitignore]: http://git-scm.com/docs/gitignore