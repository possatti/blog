---
title: Tutorial de Shell Script
description: "Neste post eu explico sobre várias estruturas usadas em shell script como: loops, condicionais, pipes, redireção, globs, substituição de comandos e mais."
tags:
  - shell script
comments: true
share: true
thumbnail: /images/sh.png
date: 2016-06-30 12:41:53
---


Neste post vou explicar um pouco de tudo o que eu sei sobre shell script. Eu comecei a usar shell script porque eu queria automatizar algumas tarefas minhas. E meu gosto pela linguagem começou pela minha fascinação. Eu lia alguns scripts, mas não entendia nada! Além disso eu percebia o potencial da linguagem, as coisas que eu poderia criar com aquilo.

Shell script é uma linguagem com uma sintaxe extranhíssima e muito diferente de qualquer outra linguagem que eu conhecia antes. Eu tinha experiência com linguagens como Python, Java, um pouco de C e outras. E estudando shell script eu aprendi muita coisa que eu não conhecia, e que pode ser aplicado em outras linguagens. Então depois de estudar, sinto que conheço mais sobre programação de forma geral. Estudar shell script também me permitiu conhecer melhor como funciona a arquitetura de programas e processos do Linux. Antes disso, eu não sabia o que era `stdout` e `stdin` por exemplo. E hoje eu percebo que esse é um conhecimento bem útil e que eu não tinha antes.

Apesar da sintaxe estranha, shell script é uma linguagem forte e capaz de muita coisa. Imagine o seguinte. Em seu computador você tem programas de todos os tipos. Alguns escritos em C, outros que foram escritos em Python, Java, Perl e etc. Agora imagine utilizar o poder de cada programa desses em coletivo para construir algo maior. É isso que você vai fazer. Usando shell script você irá orquestrar chamadas a esses programas, para cumprir um objetivo específico. Uau, isso é poderoso. É claro que você pode fazer isso em qualquer linguagem, mas shell script foi feito para isso!

Se você se interessou até aqui, continue lendo. Garanto que você vai aprender algo interessante. Porém, antes de começar, algumas sugestões:
 - Se você nunca aprendeu programação antes, te sugiro fortemente, e *enfáticamente* que você não comece por shell script. Aprenda Python, C/C++, Java, PHP ou qualquer outra linguagem e depois volte aqui. Sério!
 - E para seguir com o que eu vou explicar aqui, você deve estar um pouco acostumado a usar o terminal. Do contrário você vai ter um pouco de dificuldade.

Você irá perceber que muitas vezes eu vou ser breve e sucinto em certos assuntos. Principalmente aqueles que você provavelmente já viu em outras linguagens de programação. Outras vezes será porque não tive tempo de expandir e escrever da melhor forma possível. Se você perceber que eu não abordei algo importante, ou se algum dos exemplos está errado, ou se tem qualquer outra sugestão, por favor, deixe um comentário.

Vamos começar.

## Sumário

 - [Diferentes Shells](#Diferentes-Shells)
 - [Hello World](#Hello-World)
 - [O básico](#O-basico)
   - [Shebang (#!)](#Shebang)
   - [Comandos úteis](#Comandos-uteis)
   - [Manuais](#Manuais)
   - [Wildcards (Globs)](#Wildcards-Globs)
 - [Variáveis](#Variaveis)
 - [Substituição de comandos](#Substituicao-de-comandos)
 - [Condicionais](#Condicionais)
 - [Switch case](#Switch-case)
 - [Loops](#Loops)
   - [For](#For)
   - [While](#While)
 - [Argumentos](#Argumentos)
 - [Pipe e redireção](#Pipe-e-redirecao)
 - [Funções](#Funcoes)
 - [Matemática](#Matematica)
 - [Manipulação de texto](#Manipulacao-de-texto)
 - [Conclusão](#Conclusao)


## Diferentes Shells

Existem muitas [Shells][unix-shell] diferentes. Muitas mesmo! E cada uma delas tem uma linguagem de script diferente. Apesar disso a maioria das Shells apresentam algum nível de compatibilidade com a [Bourne Shell][sh] (`sh`), que foi uma das primeiras a existir. Algumas das Shells mais usadas são:
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

A Shell mais popular provavelmente é o Bash. Se você usa Ubuntu e não sabe qual é a sua Shell, ela provavelmente é o Bash. Ainda assim, algumas pessoas preferem outras Shells para usar interativamente. Eu prefiro o Fish, por exemplo, e conheço muitas pessoas que preferem utilizar o Zsh.

Apesar de todas as variações, a maioria delas possuem um subconjunto de operações e de sintáxe iguais ao do `sh`. Por isso vou ensinar `sh` puro aqui. Variações como o `bash` adicionam algumas coisas legais à linguagem, mas que não são compatíveis com as outras shells. Se você se restringir a `sh` puro, seu código vai funcionar na maioria das outras shells. (Não todas. O `fish`, por exemplo, não é compatível. Já sofri muito com isso. T.T)

No Ubuntu, quando você usa o `sh`, na verdade você está usando o `dash`. Mas... Whatever. O `dash` foi feito apenas para ser um versão mais rápida e leve da Bourne Shell, e é totalmente compatível com a Bourne shell (até onde eu sei).


## Hello World

Se você nunca mexeu com shell script antes, me acompanhe para fazer um Hello World. Abra o terminal (`Ctrl+Shift+T`), e use seu editor de texto favorito para criar um novo arquivo arquivo de texto. Aqui eu vou usar o `nano`.

```sh
 $ nano script.sh  # Nano é um editor de texto, use qualquer um que você prefira
```

Usando seu editor de texto, digite o seguinte código:

```sh
#!/bin/sh
echo Hello World
```

A primeira linha é um shebang (`#!`) e identifica o tipo de script que estamos criando, vou explicar melhor sobre o shebang depois. E na segunda linha, `echo` irá imprimir "Hello World". Agora, salve e feche o editor de texto. (No `nano` você usa `Ctrl+O` (letra Ó) para salvar, e `Ctrl+X` para sair.) Depois, digite o comando seguinte no mesmo terminal e você verá o `Hello World`:


```sh
$ sh script.sh
Hello world
```

*Voilà*! Agora você já sabe como criar e executar um shell script. Você já pode pegar seu certificado de "Shell Script Noob" na recepção e ir embora, ou continuar com o resto do guia para ganhar o certificado de "Shell Script Master". ;)


## O básico

No seu uso mais básico, shell script é usado para executar um comando após o outro. Igual que você estivesse usando o terminal, e digitando um comando após o outro. Mas colocando esses comandos em um script, você pode automatizar suas tarefas.

Por exemplo, depois de formatar meu computador eu tenho [vários scripts][my-setup-scripts] que instalam algumas coisas que eu costumo usar no dia-a-dia, exemplo:

[my-setup-scripts]: https://github.com/possatti/my-setup-scripts

```sh
#!/bin/sh
sudo apt-get -y install git
sudo apt-get -y install python-pip
sudo apt-get -y install fish
sudo apt-get -y install guake
sudo apt-get -y install htop
sudo apt-get -y install tree
sudo apt-get -y install transmission
sudo apt-get -y install texlive-latex-base
```

Quando eu executar o script acima, todos esses programas serão instalados pelo `apt-get`. Isso me poupa o trabalho de ter que me lembrar e de ter que digitar manualmente cada um desses comandos.

Você também pode executar mais de um comando na mesma linha, separando os comandos por `;`. O `;` é opcional no final de uma linha.

```sh
echo -n "Hello"; echo -n "World"; echo "!";
```

E a indentação também não importa.

```sh
echo -n "Hello"
	echo -n "World"
		echo -n "!"
```

Mas, por favor, indente seu código de forma intuitiva e organizada. Não é só porque você está usando a linguagem mais feia já inventada que você precisa escrever o código mais feio já inventado.

### Shebang (#!)

No linux, é muito comum você colocar um [shebang][shebang] (`#!`) na primeira linha de um script. Ele serve para que seu computador identifique qual programa roda aquele script. Ele só funciona se for a primeira linha do arquivo. Aqui estão alguns exemplos:

[shebang]: https://pt.wikipedia.org/wiki/Shebang

```
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

Se você colocar o devido shebang você pode fazer:

```sh
 $ chmod +x script.sh # Apenas da primeira vez, para transformar o arquivo em executável
 $ ./script.sh # Se você estiver no mesmo diretório que o script
 $ script.sh # Se o script estiver no seu $PATH
```

Eu costumo colocar o shebang e rodar o script com `sh script.sh` mesmo, porque ficar fazendo `chmod +x` para cada script que eu crio é muito enjoado. Mas é uma boa prática colocar o shebang, então não deixe de colocar.

### Comandos úteis

Alguns comandos você irá usar com mais frequência do que outros. É importante que você saiba que alguns deles existam, para consultá-los quando você precisar. Alguns deles são:
 - `ls`: Lista os arquivos do diretório atual.
 - `cd`: Troca o diretório atual.
 - `rm`: Apaga um arquivo ou diretório.
 - `mv`: Move um arquivo ou diretório.
 - `cp`: Copia um arquivo ou diretório.
 - `echo`: Escreve um texto na tela.
 - `test`: Veremos na parte de condicionais.
 - `grep`: Imprime linhas que correspondem à um padrão.
 - `sed`: Modifica e filtra texto.
 - `tr`: Troca ou deleta caracteres.
 - `read`: Lê um texto digitado pelo usuário e salva numa variável.
 - `pwd`: Imprime o diretório local.
 - `find`: Busca arquivos.

### Manuais

Quando você tiver dúvida sobre como um comando funciona, ou qual a sua interface, você deveria consultar sua página no manual: `man <comando>`. Por exemplo, estou com dúvida no `ls`, então eu digito `man ls`.

**Você deve se acostumar a ler os manuais**. Acostume-se a encontrar os subcomandos existentes e as opções (e.g. `--opcao`) que você precisa. E busque compreender mais ou menos a seção `SYNOPSIS` das páginas dos manuais.

E tenha em mente que as páginas dos manuais podem ser diferentes dependendo da shell que você está usando. A maioria dos programas é a mesma coisa, porém alguns comandos como `read`, por exemplo, podem funcionar um pouco diferente dependendo da Shell. Eu uso o `fish` diáriamente, e, quando estou fazendo um script, vez ou outra, tenho que entrar no `bash` só para consultar o manual de algum comando. Isso já me deu dor de cabeça algumas vezes. Se você usa o `bash`, você não deve ter muitos problemas com isso, mas fica esperto.

### Wildcards (Globs)

[*Globs*][wiki-globs] são um tipo de [*Wildcard*][wildcards], e são usados para selecionar arquivos em sistemas Unix. São caracteres que representam uma sequência genérica de caracteres. `?` representa qualquer caractere. E `*`, qualquer quantidade de qualquer caractere.

[wildcards]: https://en.wikipedia.org/wiki/Wildcard_character
[wiki-globs]: https://en.wikipedia.org/wiki/Glob_%28programming%29

Vamos supor que você tem um diretório com os seguintes arquivos: `script.pl`, `script.sh`, `script.pl`, `script.test.sh`, `test.c` e `test.java`. Se você entrar nesse diretório e usar o comando `rm test.*`, você irá remover os arquivos `test.c` e `test.java`, mas os outros arquivos ficarão intactos.

Isso funciona por uma expansão que ocorre antes mesmo do programa ser executado. No caso anterior, `rm test.*` será expandido para `rm test.c test.java`, e então o programa é invocado com estes dois argumentos.

Outro exemplo. Se usamos o comando `mv *.txt textfiles/` moveremos todos os arquivos com a extensão `.txt` para o diretório `textfiles/`.

```sh
rm script.*  # Apaga arquivos tipo: 'script.py', 'script.sh', 'script.pl', e etc.
rm "script.*"  # Globs não funcionam dentro de aspas.
rm $HOME/fotos/viagem/*-2015-01-??.jpg  # Remove as fotos de janeiro de 2015.
rm $HOME/fotos/viagem/*.jpg  # Remove todas as fotos da viagem.
```

*Globs* são muito úteis, e você deveria aprender a usar bem, pelo menos, o `*`.


## Variáveis

Chega daquelas discussões sobre o que é melhor: tipagem forte, ou tipagem fraca. Ao contrário da maioria das linguagens de programação, em que você tem vários tipos de variáveis (integer, string, boolean, etc), em shell script você tem apenas um tipo de variável... Strings!

Criar uma variável é bem simples:

```sh
variavel="Conteúdo da variável"
```

**Importante:** não coloque espaços ao redor do `=`; não comece com números; não use hífen `-`; não use caracteres especiais como `ç`, `á`, `火災` e nem emojis `😀`, `😂`.

```sh
# Não use nomes que nem esses:
98bottles="98"  # Não comece com números.
bad-variable="crap"  # Não use hífen '-'.
erro = "erro"  # Não coloque espaços ao redor do '='.
maçã="maca"  # Não use caracteres especiais nos nomes.

# Use nomes que nem esses:
bottles_n_98=98  # As aspas são opcionais quando o conteúdo da variável não contém espaços.
good_variable="(y)"  # Use underline '_' como alternativa para o hífen '-'.
CamelCase="Camelo"  # Mau gosto, porém é permitido.
maca=maçã-火災-😀  # No conteúdo você pode usar caracteres especiais. 👍
```

Agora, na hora de usar as variáveis é que tem uma pegadinha. Você deve usar `$` para acessar o valor de qualquer variável. Então a variável `fruta=maçã` deve ser acessada como `$fruta`.

```sh
texto="Hello World"
echo texto  # Pegadinha. Vai imprimir "texto".
echo $texto  # Vai imprimir "Hello World".
```

Quando você estiver escrevendo strings, você pode usar `'` ou `"`. Usando `"` o valor das variáveis são colocados no lugar dos seus nomes. Usando `'` o nome dá variável fica do jeito que está na string. O `'` ignora a existência de variáveis.

```sh
echo '$HOME'  # Imprime: $HOME
echo "$HOME"  # Imprime, ex: /home/possatti
echo "A home do usuário '$USER' é '$HOME'" # ex: A home do usuário 'possatti' é '/home/possatti'
```

Para ler uma variável do `stdin` (mais tarde eu explico sobre *File Descriptors*), ou seja, algum valor que o usuário tenha digitado, use o comando `read`. Exemplo:

```sh
echo -n "Digite seu nome: "
read nome
echo "Olá, $nome!"
```

Além das variáveis que você cria dentro do script, já existem algumas prontas para você usar. Nós chamamos elas de variáveis de ambiente (environment variables). Você pode abrir um terminal, digitar `$` e apertar tab duas vezes, e uma lista de variáveis vai aparecer para você. Algumas das mais úteis são: `$HOME`, `$HOSTNAME`, `$LANG`, `$RANDOM`, `$PWD`, `$PATH`, `$SHELL`, `$USER`, `$USERNAME`.

Você também pode passar variáveis de ambiente temporárias para um script, invocando ele como `VARIAVEL1=whatever VARIAVEL2=whatever ./script.sh`.

Também é uma convenção usar nomes de variáveis em letras maiúsculas. E geralmente é uma ótima ideia seguir as convenções. Mas você vai perceber que eu não tô nem aí pra essa convenção especificamente... kkk. Isso porque eu acho que escrever o nome das minhas variáveis em minúsculo diferencia elas melhor das variáveis de ambiente.


## Substituição de comandos

As vezes é útil guardarmos a saída de algum programa. Ao invés de imprimir na tela, gostaríamos de pegar esse valor e, por exemplo, guardar em uma variável. Para isso, usamos [substituição de comandos][wiki-command-substitution]: `$(prog)`, ou <code>&#x0060;prog&#x0060;</code>. Até onde eu sei, não há diferença entre as duas formas. Eu, particularmente, prefiro o segundo.

```sh
echo `pwd`  # Imprime o diretório atual
echo $(pwd)  # Imprime o diretório atual
arquivos_de_texto=$(ls *.txt)
echo $arquivos_de_texto  # Imprime todos os "txt" do diretório atual
echo "2 + 2 = $(expr 2 + 2)"  # Imprime '2 + 2 = 4'
```

[wiki-command-substitution]: https://en.wikipedia.org/wiki/Command_substitution


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

Eu avisei.

No script acima, o usuário digita sua idade. Se ele tiver menos que 18 anos, imprimimos que ele é menor de idade. Caso contrário, imprimimos que ele é maior de idade.

Note que os espaços entre `[` e `]` são necessários. **Não** use `[$idade -lt 18]`, use `[ $idade -lt 18 ]`. Se você não colocar os espaços, você terá um erro.

Agora vamos olhar como esse `if` funciona de verdade. Na Bourne Shell, quando escrevemos `[ $idade -lt 18 ]`, isso é a mesma coisa que `test $idade -lt 18`. Na verdade, verdade verdadeira, ele usa um comando para avaliar a nossa expressão. Os valores `$idade`, `-lt` e `18` estão sendo passados como argumentos para o programa `test`, e ele vai avaliar a expressão. Se a expressão for verdadeira, `test` termina a execução com o valor `0` (`exit 0`). Se a expressão for falsa, ele termina com um valor diferente (geralmente `1`).

Agora que você já sabe que o comando `test` está sendo usado, você já pode consultar o manual para saber que tipos de condições você pode construir: `man test`. O `test` pode realizar vários tipos de comparações, aqui estão algumas operações possíveis.

```sh
test 42 -eq 42 # eq: equal = os inteiros são iguais
test 3 -gt 2 # gt: greater-than = maior-quê
test 3 -ge 2 # ge: greater-than or equal = maior ou igual
test 2 -lt 3 # lt: less-than = menor-quê
test 2 -le 3 # le: less-than or equal = menor ou igual
test 0 -ne 1 # ne: not-equal = os inteiros não são iguais
test -n "Texto"  # n: String tem mais que zero caracteres
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

Todo comando que é executado retorna algum valor. Se tudo ocorreu bem, retorna `0`. Se deu algum erro, retorna `1` ou outro valor. Em shell script, `0` significa verdadeiro, e os outros valores significam falso. E como eu disse, todo comando retorna um valor para a shell, inclusive o seu script! Você usa `exit` para especificar que valor deve ser retornado. Se você não especificar, o retorno do último comando do seu script será usado.

E existe uma variável especial, chamada `$?` que guarda o retorno do último comando executado. Pode ser útil, às vezes.

```sh
if rm saci-pererê.txt; then echo "Saci foi apagado."; fi

test -f saci-pererê.txt  # Testa se o arquivo existe.
if [ $? -eq 0 ]; then  # O valor do comando anterior (test) entra no lugar de '$?'.
	echo "Saci ainda existe!"
	exit 1  # Saímos com erro, porque o Saci não devia mais existir.
else
	exit 0  # Saci deixou de existir. Sucesso!
fi
```


## Switch case

Outra estrutura com uma sintaxe curiosa. Se você achava o `if` estranho, é melhor se sentar. Surpreendentemente ele é mais útil do que parece, pela forma como ele trata as strings. Você pode usar wildcards (`*` e `?`) para enriquecer as expressões usadas no switch.

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


## Loops

Temos duas opções de loop aqui: `while` e `for`. Cada um é útil em uma situação específica.

### For

Na sua forma mais simples, você descrimina uma série de valores para o `for`, e a iteração acontecerá em cima desses valores. Veja o exemplo abaixo:

```sh
#!/bin/sh
# O script irá imprimir 'goiaba', 'abacaxi', e 'banana'
# cada um em uma linha separada.
for fruta in goiaba abacaxi banana; do
	echo $fruta
done
```

Abaixo está um exemplo levemente mais útil. O script apagará todos os arquivos do diretório atual, exceto o arquivo `critico.txt`.


```sh
#!/bin/sh

# Pega todos os "txt" do diretório local.
arquivos_txt=`ls *.txt`

for arquivo in $arquivos_txt; do
	if [ $arquivo == "critico.txt" ]; then
		continue  # Pula 'critico.txt' para que ele não seja apagado
	fi
	rm $arquivo
done
```

A variável `$arquivos_txt` é uma string que contém o nome de todos os arquivos `.txt` do diretório atual, separados por espaço. O `for` irá quebrar essa string em múltiplos pedaços, separando pelos espaços e pelas quebras de linha (`\n`). Esses múltiplos pedaços serão passados para a variável `$arquivo`, um de cada vez.

Você também pode usar as palavras chaves `continue` para pular uma iteração e continuar do começo, e `break` para sair do loop.

Quando você desejar iterar sob uma sequência de números, você pode usar o comando `seq`. Exemplo: `seq 3` irá imprimir `1 2 3` (separados por `\n`, na verdade) e `seq 0 3` irá imprimir `0 1 2 3`. No exemplo abaixo, nós criamos uma sequência de 0 à 10, e elevamos cada um dos números ao quadrado, e imprimimos.

```sh
#!/bin/sh
for i in $(seq 0 10); do
	i_quadrado=$(expr $i '*' $i)
	echo "$i ao quadrado é igual a: $i_quadrado"
done
```


### While

```sh
#!/bin/sh
# Observação: Fazer um loop para vigiar se um arquivo foi criado
# é uma das piores coisas que você pode fazer durante sua existência
# na terra. Isso vai usar todo o seu processador. Não faça isso! É
# só um exemplo.

while [ ! -f saci-pererê.txt ]; do  # Enquanto o arquivo *não* existe
	echo 'Vigiando o Saci ser criado.'
done

echo "Alguém criou o Saci!! Apaga o Saci!"
rm saci-pererê.txt
```

No exemplo, nós constantemente verificamos se o arquivo `saci-pererê.txt` existe. Enquanto o arquivo não existir, nós continuamos no loop imprimindo uma mensagem. Assim que o arquivo é criado, o loop termina, e nós apagamos o arquivo do Saci.

Se você quiser trollar um amigo que usa Ubuntu, execute o script seguinte no computador dele. Coloque o arquivo em sua `$HOME` com o nome de `dull-boy.sh` (o nome é importante).

```sh
#!/bin/sh

# Número de ciclos do while para criar uma nova instância
# do gnome-terminal.
ciclos_para_nova_instancia=1000

ciclos=0
while true
do
	echo "All work and no play makes Jack a dull boy"
	# Incrementa o número de ciclos.
	ciclos=`expr $ciclos + 1`
	# Verifica se chegou o momento de criar uma nova instância.
	if [ "$ciclos" -eq "$ciclos_para_nova_instancia" ]
	then
		# Cria uma nova instância do gnome-terminal, executando o mesmo script.
		gnome-terminal -x sh dull-boy.sh
		# Reinicia a contagem dos ciclos.
		ciclos=0
	fi
done
```

O script irá imprimir `"All work and no play makes Jack a dull boy"` indeterminadamente. E a cada mil iterações, irá instanciar um novo terminal executando o mesmo script. Então seguindo uma curva exponencial, em pouco tempo você terá dezenas ou centenas de terminais na tela, todos imprimindo `"All work and no play makes Jack a dull boy"` ininterruptamente.

Você pode tentar parar cada script com `Ctrl+C`. Porém chega uma hora que a única solução razoável é esperar que o sistema operacional congele os processos, ou, mais fácil, reiniciar o computador.

Se você quiser trollar seu amigo em dobro, coloque uma linha `cd $HOME; sh dull-boy.sh` no final do arquivo `.bashrc` (se ele usa o Bash), assim o script irá executar toda vez que ele abrir o terminal. Mas cuidado, pois você pode não ter um amigo depois disso.


## Argumentos

A maioria dos programas de linha de comando recebem e processam argumentos que são passados pelo usuário que está invocando o programa.

```sh
$ comando arg1 arg2 arg3 arg4 
```
E para acessarmos os argumentos, usamos as variáveis `$1`, `$2`, `$3` e etc para acessarmos o primeiro argumento, o segundo argumento e daí em diante. `$0` é o nome do seu script (até que faz sentido, né?). E `$@` representa todos os argumentos juntos e em sequência (sem o `$0`). E `$#` é o número de argumentos recebidos (também sem o `$0`).

```sh
echo "\$0: $0"  # Imprime o nome do script.
echo "\$@: $@"  # Imprime todos os argumentos.
echo "\$#: $#"  # Imprime o número de argumentos.
echo "\$1 \$2 \$3: $1 $2 $3"  # Imprime os primeiros três argumentos.
```

Se o seu script tiver recebido apenas dois argumentos e você tentar acessar, digamos, o argumento `$3`, o `$3` será substituído por uma string vazia. Para evitar isso, você pode testar se `$3` não é uma string vazia: `test -n "$3"` (as aspas são importantes nesse caso).

Quando um argumento tem a forma `-e` ou `--exemplo`, ele é chamado de uma opção, e geralmente é... opcional na chamada de um programa. Você usou opções este tempo inteiro (`echo -n`, `rm -rf`, etc), deve saber como elas funcionam. Mas só para o caso de você não saber, vou explicar um pouquinho.

Algumas opções são chamadas de forma isolada, como `--quiet`, e `--help`, e outras devem ser acompanhadas de um valor como: `--garrafas=12`, `--garrafas 12`, `-g12`. Ou: `--arquivo="file.txt"`, `--arquivo "file.txt"`. As opções podem ser usadas, não importa a ordem: `cmd --input "i.txt" --output "o.txt"` deveria ser a mesma coisa que `cmd --output "o.txt" --input "i.txt"`. E as opções geralmente são misturadas com argumentos: `echo "hello" -n` (`n` é uma opção e `"hello"`, um argumento). E muitas opções que são escritas por extenso também tem uma forma abreviada, como: `--help` é equivalente à `-h`; e `--quiet` é equivalente à `-q`. Quando você usa a forma abreviada, muitas vezes você também pode aglutinar as formas abreviadas, por exemplo `rm -rf` é equivalente à `rm -r -f`.

É claro que nem todos os programas vão seguir essas regras para suas interfaces, mas essas são regras que você vai observar na maioria dos programas de linha de comando. Existem exceções, como exemplo, o programa `java` não têm opções abreviadas e as opções extensas usam um único hífen, tipo: `java -version` ou `java -help`. (Para o caso de dúvida, programas feitos em Java podem ter qualquer interface que eles quiserem. Eu só quis dizer que o executável `java` funciona dessa forma.)

E repare que apesar de `--arquivo "file.txt"` ser uma única opção, na shell eles são visto como dois argumentos separados. Shell script não diferencia argumentos de opções por você. Para o shell script, tudo é argumento.

Se você quiser iterar sobre todos os argumentos, você pode usar um `for` para isso:

```sh
#!/bin/sh
for arg in $@; do
	echo "Argumento $arg."
done
```

Outra opção semelhante é usar o comando `shift` juntamente com um `while`. Quando você usa `shift`, o `$2` será colocado no lugar do `$1`; o `$3` no lugar do `$2`; e assim em diante. Dessa forma você pode ler o primeiro argumento através do `$1`; fazer um `shift`, ler o segundo argumento através do `$1`; `shift`, ler terceiro argumento através do `$1` também; e etc.

```sh
#!/bin/sh

# Irá imprimir "arg1", "arg2", "arg3", etc.
while [ -n "$1" ]; do
	echo "Argumento $1."
	shift
done
```

Se você for criar um script que tenha uma interface extensa e complexa, e que precisa diferenciar e tratar argumentos e opções, você terá que usar altos recursos *imaginísticos* para alcançar seus objetivos. Há algum tempo atrás eu tive que fazer isso, e tive sucesso usando um `while` com um `case` e o comando `shift`. Se você precisar de inspiração, consulte o meu repositório ["pokemonsay"][pokemonsay] no Github.

[pokemonsay]: https://github.com/possatti/pokemonsay/blob/master/pokemonsay.sh


## Pipe e redireção

Essa é provavelmente a coisa mais interessante que você pode fazer em shell script. É usando *Pipes* (literalmente, canos, ou tubos) e redireção que você vai conseguir libertar os verdadeiros poderes do shell script.

Antes de falar sobre isso, eu tenho que explicar uma coisa mais básica: *File Descriptors*. No mundo do Unix e Linux existe o que nós chamamos de *"file descriptor"*. Qualquer programa têm três *file descriptors*: Standard Input, Standard Output, e Standard Error. Comumente abreviados: `stdin`, `stdout` e `stderr`. O programa irá ler dados do `stdin`, irá escrever em `stdout`, e irá escrever os erros para `stderr`. Muitas vezes, o texto vindo de `stdin` será o texto digitado pelo usuário no teclado. Mas muitas outras vezes, esse texto será recebido de forma programática.

Quando usamos pipe `|`, nós estamos conectando o `stdout` do comando à esquerda, com o `stdin` do comando à direita. Também é possível fazer vários pipes em sequência. Você deve imaginar que o texto está fluindo da esquerda para a direita, e que cada comando está modificando o texto, ou agindo de alguma forma sobre ele. Vamos à um exemplo simples:

```sh
# Imprime "laranja_123" (minúsculo)
echo "LARANJA_123" | tr '[:upper:]' '[:lower:]'
```

O `tr` é um programa que troca alguns caracteres por outros (`man tr`). Perceba que o `echo` escreveu o texto em seu `stdout` (normalmente seria impresso, mas não foi devido ao pipe), e seu `stdout` foi redirecionado para o `stdin` de `tr`.  E o `tr`, após manipular o texto (substituir maiúsculas por minúsculas), escreveu o resultado em seu `stdout`, que por sua vez foi impresso na tela.

```sh
# Imprime "laranja"
echo "LARANJA_123" | tr '[:upper:]' '[:lower:]' | tr -d '[_0-9]'
```

Mais um pipe agora. Dessa vez, o `stdout` do primeiro `tr` não é impresso, mas é redirecionado para o `stdin` do segundo `tr`. O segundo `tr` irá ler o texto de seu `stdin`, modificá-lo (remover os números e underline `_`), e escrever em seu `stdout`. Como não há mais nenhuma redireção, seu `stdout` será impresso.

Também podemos fazer redireções usando arquivos. `>` é usado para redirecionar o `stdout` para um arquivo, porém apaga o conteúdo do arquivo se ele já existir. `>>` faz o mesmo que `>`, porém não apaga o conteúdo original do arquivo. Ao final do script abaixo, teremos um arquivo com três frutas: Caju, Mamão e Pêra.

```sh
# Escreve "Banana" no arquivo "frutas.txt".
echo "Banana" > frutas.txt
# Apaga o conteúdo do arquivo inteiro, e depois escreve "Caju" nele.
echo "Caju" > frutas.txt
# Escreve "Mamão" no final do arquivo, sem apagar seu conteúdo.
echo "Mamão" >> frutas.txt
# Escreve "Pêra_123" no final do arquivo, sem apagar seu conteúdo.
echo "Pêra_123" >> frutas.txt
```

Equivalente ao `>` temos o `<` que faz exatamente o contrário. Ele serve para "puxarmos" o texto de um arquivo e fornecer como entrada para o `stdin` de um programa. Exemplo:

```sh
# Transforma o texto de 'frutas.txt' para maiúsculas.
tr '[:lower:]' '[:upper:]' < frutas.txt
```

O texto contido em `frutas.txt` será direcionado para o `stdin` de `tr`, que irá modificar o texto e imprimir na tela.

Também é comum usarmos `<`, `>` e `|` tudo junto. É um pouco difícil de se acostumar com a leitura. Mas é algo comum e útil. Veja o exemplo abaixo.

```sh
# Transforma o texto de 'frutas.txt' para maiúsculas, remove números e '_'
# e grava em 'FRUTAS.TXT'.
# Perceba que nada é impresso. Pois tudo é gravado em 'FRUTAS.TXT'.
tr '[:lower:]' '[:upper:]' < frutas.txt | tr -d '[_0-9]' > FRUTAS.TXT
```

Pêra! (huehue) Se existe `>` e `>>`, deve existir também `<<`, já que existe `<`. Sim, senhor. E o nome disso é ["Here Document"][wiki-here-document]. Ao invés de ler de um arquivo (como `<`) o texto será lido do próprio script.

[wiki-here-document]: https://en.wikipedia.org/wiki/Here_document

```sh
tr '[:lower:]áãçó' '[:upper:]ÁÃÇÓ' << EOF
O empenho em analisar o aumento do diálogo entre os diferentes
setores produtivos estimula a padronização dos modos de operação
convencionais.
Desta maneira, o julgamento imparcial das eventualidades cumpre
um papel essencial na formulação do impacto na agilidade decisória.
EOF
```

Perceba que depois do `<<` temos um token (`EOF`) que abre o texto do [lerolero.com][lerolero], e em seguida o **mesmo token** deverá ser repetido em sua própria linha, para fechar o texto. O texto que está entre os dois `EOF` (*end of file*, fim de arquivo), será usado como entrada de dados para o `tr`, que por usa vez imprimirá o texto inteiro em letras maiúsculas. É comum usarmos a sigla `EOF` como token, mas pode ser qualquer palavra, como `LEROLERO`, ou `HELLO_WORLD!`.

[lerolero]: http://www.lerolero.com/

Agora um pequeno exercício mental. Tente entender que parte de texto está servindo de entrada para qual comando. Boa sorte.

```sh
#!/bin/sh
sh << MIND
sh << BLOWING
echo "Mind blowing."
BLOWING
MIND
```

Cada *file descriptor* tem um número associado: `stdin`, `0`; `stdout`, `1`; e `stderr`, `2`. É comum redirecionarmos o `stderr` de um programa para o `stdout` do mesmo programa. Fazemos isso usando `2>&1`. Isso é muito útil quando temos um programa que escreve coisas importantes para `stderr`, porém nós queremos gravar em um arquivo, por exemplo. Para isso fazemos `prog 2>&1 > meu.log`. Ou ainda podemos gravar o `stdout` e o `stderr` em diferentes arquivos: `prog 1> meu.log 2> erros.log`.

E se quisermos direcionar o `stdout` para `stderr`, usamos `1>&2`. Você pode usar isso para escrever em `stderr` no seu script através de `echo 1>&2`.

```sh
echo "Hello"  # Imprime através do `stdout`
echo "World" 1>&2  # Imprime na tela, porém através do `stderr`
```

Redireções também funcionam com estruturas como `for` e `while`. Quando você chega nesse nível, as coisas podem ficar extremamente confusas. O exemplo abaixo, lê as linhas de um arquivo `lower.txt`, colocando cada uma delas na variável `$linha`, que é "`echo`ada" para `tr`, que transforma tudo em maiúsculas. Porém o `stdout` de `tr` vai para um segundo `tr` que apaga as vogais do texto. E em seguida, o resultado é escrito em `UPPER.txt`. Loucura.

```sh
#!/bin/sh

while read linha; do
	echo $linha | tr '[:lower:]' '[:upper:]'
done < lower.txt | tr -d 'aeiou' > UPPER.txt
```

Era possível escrever o script acima de forma mais simples. Mas eu quis fazer assim, para você exercitar seu poderoso cérebro.


## Funções

Funções funcionam como mini-scripts contidos no seu script. Elas são declaradas como `foo() { ... }` e são invocadas como qualquer outro comando: `foo arg1 arg2 arg3 ...`.

```sh
#!/bin/sh

somar() {
	# Soma os dois argumentos recebidos *pela função*.
	expr $1 '+' $2
}

# Processa todos os argumentos recebidos pelo script
# somando todos eles.
resultado=0
while [ -n "$1" ]; do
	# Soma o argumento com o resultado atual.
	resultado=`somar $resultado $1`
	# Coloca o $2 no lugar do $1; o $3 no lugar do $2; etc.
	shift
done

echo "Soma total: $resultado"
```

Perceba que dentro da função `$1` e `$2` são argumentos recebidos **pela função**, e não pelo script. Do lado de fora da função, nós estamos usando o `$1` que é o primeiro argumento do nosso script. Veja que as duas coisas não se misturam.

**Cuidado:** as funções podem alterar variáveis do escopo global:

```sh
troll() {
	x=2
}
x=1
echo $x  # Imprime '1'
troll  # Muda o valor de 'x'
echo $x  # Imprime '2'
```

Eu acho que isso é o que tem de mais importante para falar sobre as funções em shell script. Acho que você deve saber o que fazer a partir daqui. Mas me sinto culpado de não colocar um exemplo um pouco mais complexo. Então abaixo está uma função que calcula o fatorial de um número...

```sh
fatorial() {
	if [ "$1" -gt "1" ]; then
		i=`expr $1 - 1`
		j=`fatorial $i`
		k=`expr $1 \* $j`
		echo $k
	else
		echo 1
	fi
}

# Calcula os 5 primeiros fatoriais.
for n in `seq 5`; do
	fatorial $n
done
```


## Matemática

De vez em quando precisamos fazer uma conta ou outra em shell script. A forma como fazemos isso é usando qualquer comando que faça contas. Alguns dos mais úteis são `expr` e `bc`.

O mais básico é o `expr`. Ele serve para fazer contas simples, mas deixa a desejar para contas mais complexas e de número flutuante. E ele é um pouco chato quanto aos espaços. Você precisa separar cada um dos números e operadores, pois eles devem ser recebidos como diferentes argumentos. E você precisa ter cuidado com o `*` de multiplicação, para que ele não seja interpretado como um wildcard antes mesmo de ser recebido pelo `expr`, então use `\*` ou `'*'`. O mesmo vale para os parênteses, use `\( ... \)`, ou `'(' ... ')'`

```sh
expr 2 + 2  # "4"
expr 2+2  # "2+2" -- lol
expr 2+2 + 2  # "expr: non-integer argument"
expr 2 '*' 3  # "6"
expr 8 \* 0.5 # "expr: non-integer argument"
expr 8 / 4 # "2"
expr 8 / 5 # "1" -- A divisão é inteira
expr \( 3 + 7 \) / \( 1 + 1 \)  # "5"
```

Como você pode ver, `expr` apenas gosta de números inteiros. Além disso, expressões mais complexas ficam extremamente longas, já que você tem que colocar espaços ao redor de tudo.

Para contas um pouco mais complexas, ou quando você quiser usar números decimais, recomendo usar o `bc`. Porém este programa possui outro inconveniente: você tem que passar as contas para ele por redirecionamento, pois ele não processa contas pelos argumentos. E para contas com muitas casas decimais, use `bc -l`.

```sh
echo "2 + 2" | bc  # "4"
echo "2+2 + 2" | bc  # "6"
echo "2*3" | bc  # "6"
echo "8 * 0.5" | bc  # "4.0"
echo "8 / 4" | bc  # "2"
echo "8 / 5" | bc  # "1"
echo "8 / 5" | bc -l  # "1.60000000000000000000"
echo "(3 + 7) /2" | bc  # "5"
echo "2.22 / 1.22 * 0.75" | bc  # ".75" -- What???
echo "2.22 / 1.22 * 0.75" | bc -l  # "1.36475409836065573770" -- Hmmmm
```

Eu nunca usei muito o `bc`, mas parece que ele é capaz de fazer [bem mais][wiki-bc]. Se você precisar de expressões matemáticas complexas, dê uma olhada na sua documentação com carinho.

[wiki-bc]: https://en.wikipedia.org/wiki/Bc_(programming_language)#GNU_bc


## Manipulação de texto

Umas das coisas mais comuns que você vai fazer em shell script é manipular texto. Por isso é bom que você saiba fazer isso bem. Minha sugestão é que você aprenda bem, um dos seguintes programas: `sed`, `awk` ou `perl`. Eu costumo usar o `sed`. Porém, explicar como ele funciona é um tutorial à parte. Mas veja algumas coisas básica que você pode fazer com o `sed`.

```sh
# Imprime todo o texto recebido, porém substituindo "banana" por "maçã"
echo "banana-banana" | sed 's/banana/maçã/g'  # "maçã-maçã"
# Imprime apenas as linhas que começam com "Erro" ou "erro"
echo -e "Uva\nErro 1\nPêra\nerro2-critico" | sed -nr '/^[Ee]rro/p'  # "Erro 1\nerro2-critico"
# Formata um número de telefone
echo '27988882222' | sed -r 's/(.{2})(.{1})(.{4})(.{4})/(\1) \2 \3-\4/'  # "(27) 9 8888-2222"
# Pega apenas o nome do arquivo
echo "fotos/viagem/familia.jpg" | sed -r 's;.*/([a-Z]+)\..+;\1;'  # "familia"
```

Infelizmente não tem como eu explicar aqui com detalhes como funciona o `sed`. Mas, pelo menos, o primeiro exemplo você deve ter entendido. Eu, pessoalmente, aprendi o que sei de `sed` (30% do total, talvez) usando uma [página na internet que parecia ter sido feita no período jurássico][sed]. Sinta-se livre para buscar qualquer fonte que possa te ajudar.

[sed]: http://www.grymoire.com/Unix/Sed.html


## Conclusão

Essa é a despedida. Depois de tudo isso, eu agora espero que você consiga usar shell script para resolver seus problemas. Se ficou faltando alguma coisa, ou não deu para entender alguma parte, pode postar o seu feedback aqui nos comentários.

É isso aí. Abraço.
