---
title: "Tutorial de Shell Script"
tags: [shell script, sh]
---

Neste post vou explicar um pouco de tudo o que eu sei sobre Shell Script. Eu comecei a usar Shell Script, porque eu queria automatizar algumas tarefas minhas e porque sentia que a linguagem tinha potêncial para fazer coisas muito legais. Também me senti atraído pela sintaxe mais estranha que uma linguagem provavelmente pode ter. Existem outras mais estranhas, porém Shell Script é uma das linguagens mais estranhas que você irá conhecer. Mas não deixe isso te desanimar.

Se você nunca aprendeu programação antes, te sugiro fortemente, e enfáticamente que você não começe por Shell Script. Aprenda Python, C/C++, Java, PHP ou qualquer outra linguagem e depois você volta aqui. Sério!

Para seguir com o que eu vou ensinar aqui, você deve estar um pouco acostumado a usar o terminal. Do contrário você vai ter um pouco de dificuldade.


## Diferentes Shells

Existem muitas [Shells][unix-shell] diferentes. Muitas mesmo! E cada uma delas tem uma linguagem de script diferente. Apesar disso a maioria das Shells apresentam algum nível de compatíbilidade com a [Bourne Shell][sh] (`sh`), que foi uma das primeiras. Algumas das Shells mais usadas são:
 - sh ([Bourne shell][sh])
 - bash ([Bourne-Again shell][bash])
 - dash ([Debian Almquist shell][dash])
 - fish ([friendly interactive shell][fish]. minha favorita)
 - zsh ([Z shell][zsh]. Muita gente gosta dessa para usar no dia-a-dia)
 - ksh ([Korn shell][ksh])
 - csh ([C shell][csh])
 - ... ad infinitum.

[unix-shell]: https://en.wikipedia.org/wiki/Unix_shell
[sh]: https://en.wikipedia.org/wiki/Bourne_shell
[bash]: https://en.wikipedia.org/wiki/Bash_(Unix_shell)
[dash]: https://en.wikipedia.org/wiki/Almquist_shell
[fish]: https://en.wikipedia.org/wiki/Friendly_interactive_shell
[zsh]: https://en.wikipedia.org/wiki/Z_shell
[ksh]: https://en.wikipedia.org/wiki/Korn_shell
[csh]: https://en.wikipedia.org/wiki/C_shell

A Shell mais popular provavelmente é o Bash. Se você usa Ubuntu e não sabe qual a sua Shell, ela provavelmente é o Bash. Ainda assim, algumas pessoas preferem outras Shells para usar interativamente. Eu prefiro o Fish, por exemplo, e conheço muitas pessoas que preferem utilizar o Zsh.

Apesar de todas as variações, a maioria delas possuem um subconjunto de operações e de sintáxe iguais ao do `sh`. Por isso vou ensinar `sh` puro aqui. Variações como o `bash` adicionam algumas coisas legais a linguagem, mas que não são compatíveis com as outras shells. Se você se restringir a `sh` puro, seu código vai funcionar na maioria das outras shells. (Não todas. O `fish`, por exemplo, não é compatível. Já sofri muito com isso. T.T)

No Ubuntu, quando você usa o `sh`, na verdade você está usando o `dash`. Mas... Whatever. O `dash` foi feito apenas para ser um versão mais rápida e leve da Bourne Shell, e é totalmente compatível com a Bourne shell (até onde eu sei).


## Hello World

Se você nunca mexeu com Shell Script antes, me acompanhe para fazer um Hello World. Abra o terminal (Ctrl+Shift+T), e use seu editor de texto favorito para criar um novo arquivo arquivo de texto. Aqui eu vou usar o `nano`.

```sh
 $ nano script.sh  # Nano é um editor de texto, use qualquer um que você prefira
```

Usando seu editor de texto, digite o seguinte código:

```sh
#!/bin/sh
echo Hello World
```

A primeira linha é um shebang (`#!`) e identifica o tipo de script que estamos criando, vou explicar melhor sobre o shebang depois. E na segunda `echo` irá imprimir "Hello World". Agora, salve e feche o editor de texto. (No nano você usa `Ctrl+O` (letra Ó) para salvar, e `Ctrl+X` para sair.) Depois, digite o comando seguinte no mesmo terminal e você verá o `Hello World`:


```sh
$ sh script.sh
Hello world
```

Voilà! Agora você já sabe como criar e executar um Shell Script. Você já pode pegar seu certificado de "Shell Script Noob" na recepção e ir embora, ou continuar com o resto do guia para ganhar o certificado de "Shell Script Master". ;)

## O básico

No seu uso mais básico, Shell Script é usado para executar um comando após o outro. Igual que você estivesse usando o terminal, e digitando um comando após o outro. Mas colocando esses comandos em um script, você pode automatizar suas tarefas.

Por exemplo, depois de formatar meu computador eu tenho vários scripts que instala algumas coisas que eu costumo usar no dia-a-dia:

```sh
#!/bin/sh
sudo apt-get -y install guake
sudo apt-get -y install htop
sudo apt-get -y install tree
sudo apt-get -y install fish
sudo apt-get -y install texlive-latex-base
sudo apt-get -y install python-pip
sudo apt-get -y install transmission
sudo apt-get -y install git
```

Isso me poupa o trabalho de ter que me lembrar e de ter que digitar manualmente cada um desses comandos.

Você também pode executar mais de um comando na mesma linha, separando os comandos por `;`. O `;` é opcional no final de uma linha.

```sh
echo -n "Hello"; echo -n "World"; echo "!";
```

E a identação também não importa.

```sh
echo -n "Hello"
	echo -n "World"
		echo -n "!"
```

Mas, por favor, idente seu código de forma intuitiva e organizada. Não é só porque você está usando a linguagem mais feia já inventada que você precisa escrever o código mais feio já inventado.

### Shebang (#!)

No linux, é muito comum você colocar um [shebang][shebang] na primeira linha de um script. Ele serve para que seu computador identifique qual programa roda aquele script. Ele só funciona se for a primeira linha do arquivo. Aqui estão alguns exemplos:

[shebang]: https://pt.wikipedia.org/wiki/Shebang

```sh
#!/bin/sh
#!/usr/bin/python
#!/usr/bin/ruby
#!/bin/bash
#!/usr/bin/perl
```

Agora você já percebeu que ele toma o formato `#!` + `<executável>`. Sendo que o executável é o programa que vai rodar o seu script. Se você não colocar um shebang no seu script, você vai precisar de executar ele da seguinte forma:

```sh
 $ sh script.sh
```

Se você colocar o shebang você pode fazer:

```sh
 $ chmod +x script.sh # Apenas da primeira vez, para transformar o arquivo em executável
 $ ./script.sh # Se você estiver no mesmo diretório que o script
 $ script.sh # Se o script estiver no seu $PATH
```

Eu costumo colocar o shebang e rodar o script com `sh script.sh` mesmo, porque ficar fazendo `chmod +x` para cada script que eu crio é muito enjoado. Mas é uma boa prática colocar o shebang, então não deixe de colocar.

### Comandos úteis

Alguns comandos você irá usar com mais frequência que outros. É importante que você pelo menos saiba que alguns deles existam, para consultá-los quando você precisar. Alguns deles são:
 - `ls`: Lista os arquivos do diretório atual.
 - `cd`: Troca o diretório atual.
 - `rm`: Apaga um arquivo ou diretório.
 - `mv`: Move um arquivo ou diretório.
 - `cp`: Copia um arquivo ou diretório.
 - `echo`: Excreve um texto na tela.
 - `test`: Veremos na parte de condicionais.
 - `grep`: Imprime linhas que correspondem a um padrão.
 - `sed`: Modifica e filtra texto.
 - `read`: Lê um texto e salva numa variável
 - `pwd`: Imprime o diretório local.
 - `find`: Busca arquivos.

### Manuais

Quando você tiver dúvida sobre como um comando funciona, ou qual a sua interface, você deveria consultar sua página no manual: `man <comando>`. Por exemplo, estou com dúvida no `ls`, então eu digito `man ls`.

**Você deve se acostumar a ler os manuais**. Acostume-se a encontrar os subcomandos e as opções (`--opcao`) que você precisa. E também compreender mais ou menos a seção `SYNOPSIS` das páginas dos manuais.

E tenha em mente que as páginas dos manuais podem ser diferentes dependente da shell que você está usando. A maioria dos programas é a mesma coisa, mas comandos mais básicos como `echo`, `ls` comumente funcionam um pouco diferente dependendo da shell. Eu uso o `fish` diáriamente, e, quando estou fazendo um script, vez ou outra tenho que entrar no `bash` só para consultar o manual de algum comando. Isso já me deu dor de cabeça algumas vezes. Se você usa o `bash`, você não deve ter muitos problemas com isso, mas fica esperto.


## Variáveis

Criar um variável é simples:

```sh
variavel="Conteúdo da variavel"
```

**Importante:** não coloque espaço entre o `=`; não começe com números; não use hífen `-`.

```sh
# Não use nomes que nêm esses:
98bottles="98"
bad-variable="crap"
```

```sh
# Use nomes que nêm esses:
bottles_n_98="98"
good_variable="(y)"
CamelCase="Camelo"  # Máu gosto, mas é permitido.
```

Para usar uma variável é que tem uma pegadinha. Você deve usar `$` para acessar o valor de qualquer variável.

```sh
texto="Hello World"
echo texto  # Pegadinha. Vai imprimir "texto".
echo $texto  # Vai imprimir "Hello World".
```

As strings podem ser de dois tipos: `'` e `"`. Usando `"` o valor da variável é colocado no lugar do seu nome. Usando `'` o nome dá variável fica do jeito que tá na strings. O `'` ignora a existência de variáveis.

```sh
echo '$HOME'  # Imprime: $HOME
echo "$HOME"  # Imprime, ex: /home/possatti
echo "A home do usuário '$USER' é '$HOME'" # ex: A home do usuário 'possatti' é '/home/possatti'
```

Para ler uma variável do `stdin` (mais tarde eu explico sobre File Descriptors), ou seja, algum valor que o usuário tenha digitado, use o comando `read`. Exemplo:

```sh
echo -n "Digite seu nome: "
read nome
echo "Olá, $nome!"
```


## Condicionais

Esse é o `if` mais feio que você provavelmente vai escrever na sua vida. Mas vamos nessa.

```sh
#!/bin/sh

echo -n "Digite sua idade: "
read idade

if [ $idade -lt 18 ]; then
	echo "Você é menor de idade."
else
	echo "Você é maior de idade."
fi
```

Eu avisei. No script acima, o usuário digita sua idade. Se ele tiver menos que 18 anos, imprimimos que ele é menor de idade. Caso contrário, imprimimos que ele é maior de idade.

Note que os espaços entre `[` e `]` são necessários. **Não** faça `[$idade -lt 18]`, faça `[ $idade -lt 18 ]`. Se você não colocar os espaços você terá um erro.

Agora vamos olhar como esse `if` funciona de verdade. Que . Na Bourne Shell, quando escrevemos `[ $idade -lt 18 ]`, isso é a mesma coisa que `test $idade -lt 18`. Na verdade, verdade, verdadeira, ele usa um comando para avaliar a nossa expressão. Os valores `$idade`, `-lt` e `18` estão sendo passados como argumentos para o programa `test`, e ele vai avaliar a expressão. Se a expressão for verdadeira, `test` termina a execução com o valor `0` (`exit 0`). Se a expressão for falsa, ele termina com um valor diferente (geralmente `1`).

Agora que você já sabe que o comando `test` está sendo usado, você já pode consultar o manual para saber que tipos de condições você pode construir: `man test`. O `test` pode realizar vários tipos de comparações, aqui estão algumas operações possíveis.

```sh
test 42 -eq 42 # eq: equal = os inteiros são iguais
test 3 -gt 2 # gt: greater-than = maior-quê
test 3 -ge 2 # ge: greater-than or iqual = maior ou igual
test 2 -lt 3 # lt: less-than = menor-quê
test 2 -le 3 # le: less-than or iqual = menor ou igual
test 0 -ne 1 # ne: not-iqual = os inteiros não são iguais
test -n "Texto"  # n: String tem mais que zero caractéres
test -z ""  # z: String tem zero carácteres
test "Goiaba" == "Goiaba" # ==: As Strings são iguais
test "Goiaba" != "Mamão" # ==: As Strings são diferentes
test -d "/home"  # Verifica se o diretório existe
test -f saci-pererê.txt  # f: Arquivo existe
```

Podemos reescrever o condicional do código anterior usando o `test` explicitamente. Dá na mesma coisa:

```sh
if test $idade -lt 18; then
	echo "Você é menor de idade."
else
	echo "Você é maior de idade."
fi
```

Todo comando que é executado retorna algum valor. Se tudo ocorreu bem, retorna `0`. Se deu algum erro, retorna `1` ou outro valor. Em shell script `0` significa verdadeiro, e os outros valores significam falso. E como eu disse, todo comando retorna um valor para a shell, inclusive o seu script! Você usa `exit` para especificar que valor deve ser retornado. Se você não especificar, o retorno do último comando do seu script será usado.

E existe uma variável especial, chamada `$?` que guarda o retorno do último comando executado. Pode ser útil, as vezes.

```sh
if rm saci-pererê.txt; then echo "Saci foi apagado."; fi
test -f saci-pererê.txt  # Testa se o arquivo existe
if $?; then  # O valor do comando anterior (test) entra no lugar de $?
	echo "Saci ainda existe!"
	exit 1  # Saímos com erro, pq o Saci não devia mais existir
else
	exit 0  # Saci deixou de existir. Sucesso!
fi
```


## Switch case

Outra estrutura com uma sintáxe curiosa. Se você achava o `if` estranho, é melhor se sentar. Surpreendemente ele é mais útil do que parece, pela forma como ele trata as Strings. Você pode usar wildcards (`*` e `?`) para enriquecer as expressões usadas no switch.

```sh
#!/bin/sh

echo -n "Em que planeta você mora? "
read planeta

case $planeta in
	"terra")
		echo "Você é terráqueo."
		;;

	"marte")
		echo "Você é marciano."
		;;
	*)
		echo "Você é um lixo das galáxias! Sorry..."
		;;
esac  # "case" ao contrário para fechar o switch
```

Repare que você fecha o `case` usando `esac` (`case` ao contrário). E não me pergunte porquê você tem que colocar `;;` no final de cada caso! Deixo isso como um dever de casa para você. Boa sorte.

## Argumentos

A maioria dos programas de linha de comando recebem e processam argumentos que são passados pelo usuário que estão invocando o programa.

```sh
$ comando arg1 arg2 arg3 arg4 
```
E para acessarmos os argumentos, usamos as variáveis `$1`, `$2`, e etc para acessarmos o primeiro argumento, o segundo argumento e daí em diante. `$0` é o nome do seu script (faz sentido, né?). E `$@` representa todos os argumentos juntos em sequência (sem o `$0`).

```sh
echo $0  # Imprime o nome do script.
echo $1 $2 $3  # Imprime os primeiros três argumentos.
echo $@  # Imprime todos os argumentos.
```

Quando um argumento tem a forma `-e` ou `--exemplo`, ele é chamado de uma opção, e geralmente é... opcional. Você usou opções este tempo inteiro, deve saber como elas funcionam. Mas só para o caso de você não saber, vou explicar um pouquinho. Algumas opções devem ser acompanhadas de um valor como: `--garrafas=12`, `--garrafas 12`, `-g12`. Ou: `--arquivo="file.txt"`, `--arquivo "file.txt"`. As opções podem ser usadas não importa a ordem: `cmd --input "i.txt" --output "o.txt"` deveria ser a mesma coisa que `cmd --output "o.txt" --input "i.txt"`. E as opções geralmente são misturadas com argumentos: `echo "hello" -n` (`n` é uma opção e `"hello"`, um argumento).

E repare que apesar de `--arquivo "file.txt"` ser uma única opção, na shell eles são visto como dois argumentos e separados. Shell script não diferencia argumentos de opções para você. Para o shell script, tudo é argumento.

Se você for criar um script que tenha uma interface extensa e complexa, que precisa diferenciar e tratar argumentos e opções, você terá que usar altos recursos *imaginísticos* para alcançar seus objetivos. Eu particularmente gosto de usar um switch case para esse fim. Se você precisar de inspiração, consulte o meu repositório [pokemonsay][pokemonsay] no Github.

[pokemonsay]: https://github.com/possatti/pokemonsay/blob/master/pokemonsay.sh


## Loops

Temos duas opções de loop aqui: `while` e `for`. Cada um útil em uma situação específica.

### For loop

```sh
#!/bin/sh

# Pega todos os "txt" do diretório local. Mais sobre essa sintáxe depois.
arquivos=`ls *.txt`

for arquivo in $arquivos; do
	rm $arquivo
done
```

```sh
for i in words; do
	break
done
```

### While

```sh
## Observação: Fazer um loop para vigiar se um arquivo foi criado
## é uma das piores coisas que você pode fazer durante sua existência
## na terra. Isso vai usar todo o seu processador. Não faça isso! É
## só um exemplo.

while [ ! -f saci-pererê.txt ]; do  # Enquanto arquivo *não* existe
	echo 'Vigiando o Saci ser criado.'
done

echo "Alguém criou o Saci!! Apaga o Saci!"
rm saci-pererê.txt
```

## Pipe e redireção


## Funções


Declarando funções você irá usá-las como pequenos comandos

Add() {
	
}

## Matemática
De vez em quando precisamos fazer uma conta ou outra em Shell Script. A forma como fazemos isso é usando qualquer comando que faça contas. Yeah! Alguns dos mais úteis são: `expr`, `bc`
## Manipulação de texto
Umas das coisas mais comuns que você vai fazer em Shell Script é manipular texto. Existem várias formas, minha sugestão é que você aprenda bem, um dos seguintes: `sed`, `awk`, `perl`
## Substituição de comandos
Assim: `$()` ` `` `
[wiki-command-substitution]: https://en.wikipedia.org/wiki/Command_substitution
## Wildcards (Globs)
Como: `*` , `?`
## Here documents
[wiki-here-document]: https://en.wikipedia.org/wiki/Here_document
```sh
$ cat << EOF
Texto
EOF
``