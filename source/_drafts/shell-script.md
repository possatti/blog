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

## Wildcards (Globs)

`?` representa qualquer caractere. E `*`, qualquer quantidade de qualquer caractere.

```sh
ls $HOME/fotos/viagem/*.jpg  # Imprime o nome de todas as fotos da viagem
ls $HOME/fotos/viagem/*-2015-01-??.jpg  # Apenas as fotos de janeiro
```

## Substituição de comandos

As vezes é útil guardarmos a saída de algum programa. Ao invés de imprimir na tela, gostaríamos de pegar esse valor e guardar em uma variável, por exemplo. Para isso, usamos [substituição de comandos][wiki-command-substitution]: `$(prog)`, ou ``prog``. Até onde eu sei, não há diferença entre as duas formas. Eu, particularmente, prefiro o segundo.

```sh
echo `pwd`  # Imprime o diretório atual
arquivos_de_texto=$(ls *.txt)
echo $arquivos_de_texto  # Imprime todos os "txt" do diretório atual
echo "2 + 2 = $(expr 2 + 2)"  # Imprime '2 + 2 = 4'
```

[wiki-command-substitution]: https://en.wikipedia.org/wiki/Command_substitution

## Loops

Temos duas opções de loop aqui: `while` e `for`. Cada um é útil em uma situação específica.

### For loop

```sh
#!/bin/sh

# Pega todos os "txt" do diretório local. Mais sobre essa sintáxe depois.
arquivos_txt=`ls *.txt`

for arquivo in $arquivos_txt; do
	if [ $arquivo == critical.txt ]; then
		continue  # Pula 'critical.txt' para que ele não seja apagado
	fi
	rm $arquivo
done
```



Quando você desejar iterar sob uma sequência de números que você determinou, você pode usar o comando `seq`. Exemplo: `seq 3` irá imprimir `1 2 3` (separados por `\n`, na verdade) e `seq 0 3` irá imprimir `0 1 2 3`. No exemplo abaixo, nós criamos uma sequência de 0 à 10, e elevamos cada um deles ao quadrado.

```sh
for i in $(seq 0 10); do
	i_quadrado=$(expr $i '*' $i)
	echo "$i ao quadrado é igual a: $i_quadrado"
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

Essa é provavelmente a coisa mais interessante que você pode fazer em shell script. É usando Pipes (literalmente, canos, ou tubos) e redireção que você vai conseguir libertar os verdadeiros poderes do shell script.

Antes de falar sobre isso, eu tenho que explicar uma coisa mais básica: Fil. No mundo do Unix e Linux existe o que nós chamamos de "file descriptor". Qualquer programa têm três *file descriptors*: Standard Input, Standard Output, e Standard Error. Comumente abreviados: `stdin`, `stdout`, `stderr`. O programa irá ler dados do `stdin`, irá escrever em `stdout` . Muitas vezes, o texto vindo de `stdin` será o texto digitado pelo teclado, pelo usuário. Mas muitas outras vezes, esse texto será recebido de forma programática.

Quando usamos pipe `|`, nós estamos conectando o `stdout` do comando à esquerda, com o `stdin` do comando à direita. É possível fazer vários pipes em sequência também. Você deve imaginar que o texto está fluindo da esquerda para a direita, e que cada comando está modificando o texto, ou agindo de alguma forma sobre ele. Vamos à um exemplo simples:

```sh
echo "LARANJA_123" | tr '[:upper:]' '[:lower:]' # Imprime "laranja_123" (minúsculo)
```

O `tr` é um programa que troca alguns caracteres em outros (`man tr`). Perceba que o `echo` escreveu o texto em `stdout` (normalmente seria impresso, mas não foi devido ao pipe), e seu `stdout` foi redirecionado para o `stdin` de `tr`.  E o `tr`, após manipular o texto, escreveu o resultado em seu `stdout`, que foi impresso na tela.

```sh
FRUTA='LARANJA_123'
echo $FRUTA | tr '[:upper:]' '[:lower:]' | tr -d '[_0-9]'  # Imprime "laranja"
```

Mais um pipe agora. Dessa vez, o `stdout` do primeiro `tr` não é impresso, mas é redirecionado para o `stdin` do segundo `tr`. O segundo `tr` irá ler o texto de seu `stdin`, modificá-lo, e escrever em `stdout`. Como não há mais nenhuma redireção, seu `stdout` será impresso.

Não sei se já conseguiu perceber. Mas isso é incrivelmente útil!


Também podemos fazer redireções usando arquivos. `>` é usado para redirecionar o `stdout` para um arquivo, porém apaga o conteúdo do arquivo, se ele já existir. `>>` faz o mesmo que `>`, porém não apaga o conteúdo original do arquivo. Ao final do script abaixo, teremos um arquivo com três frutas: Cajú, Mamão e Pêra.

```sh
# Escreve "Banana" no arquivo "frutas.txt"
echo "Banana" > frutas.txt
# Apaga o conteúdo do arquivo inteiro, e depois escreve "Cajú" nele
echo "Cajú" > frutas.txt
# Escreve "Mamão" no final do arquivo, sem apagar seu conteúdo
echo "Mamão" >> fruta.txt
echo "Pêra" >> fruta.txt
```

`<`
Agora outro exemplo

Pêra! (huehue.) Se existe `>` e `>>`, deve existir também `<<`, já que existe `<`. Sim, senhor. E o nome disso é "[Here Document][wiki-here-document]". Ao invés de ler de um arquivo (como `<`) o texto será lido do próprio script.

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

Exercício mental:

```sh
#!/bin/sh
sh << MIND
sh << BLOWING
echo "Mind blowing."
BLOWING
MIND
```

Cada file descriptor tem um número associado: `stdin`, `0`; `stdout`, `1`; e `stderr`, `2`. É comum redirecionarmos o `stderr` de um programa para o `stdout` do mesmo programa. Fazemos isso usando `2>&1`. Isso é muito útil quando temos um programa que escreve coisas importante para `stderr`, porém nós queremos gravar em um arquivo, por exemplo. Para isso fazemos `prog 2>&1 > meu.log`. Ou ainda, gravarmos `stdout` e `stderr` em diferentes arquivos: `prog 1> meu.log 2> erros.log`.

E se quisermos direcionar o `stdout` para `stderr`, usamos `1>&2`. Você pode usar isso para escrever em `stderr` no seu script através de `echo 1>&2`.

```sh
echo "Hello"  # Imprime através do `stdout`
echo "World" 1>&2  # Imprime na tela, porém através do `stderr`
```

Redireções também funcionam com estrutras como `for` e `while`. Quando você chega nesse nível as coisas podem ficar extremamente confusas. O exemplo abaixo, lê as linhas de um arquivo `lower.txt`, colocando cada uma delas na variável `$linha`, que é `echo`ada para `tr`, que transforma tudo em maíusculas. Porém o `stdout` de `tr` vai para um segundo `tr` que apaga as vogais do texto. E em seguida, o resultado é escrito em `UPPER.txt`. Loucura.

```sh
#!/bin/sh

while read linha; do
	echo $linha | tr '[:lower:]' '[:upper:]'
done < lower.txt | tr -d 'aeiou' > UPPER.txt
```


## Funções

Funções funcionam como mini-scripts contidas no seu script. Elas são declaradas como `foo() { ... }` e são invocadas como qualquer comando: `foo arg1 arg2 arg3 ...`.

```sh
somar() {
	expr $1 '+' $2
}

resultado=`somar 12 21`
echo $resultado  # Imprime '33'
```

**Cuidado:** as funções podem alterar variáveis do escobo global:

```sh
troll() {
	x=2
}
x=1
echo $x  # Imprime '1'
troll
echo $x  # Imprime '2'
```

## Matemática

De vez em quando precisamos fazer uma conta ou outra em Shell Script. A forma como fazemos isso é usando qualquer comando que faça contas. Yeah! Alguns dos mais úteis são: `expr`, `bc`

## Manipulação de texto

Umas das coisas mais comuns que você vai fazer em Shell Script é manipular texto. Por isso é bom que você saiba fazer isso bem. Minha sugestão é que você aprenda bem, um dos seguintes: `sed`, `awk` ou `perl`. Eu costumo usar o `sed`, e explicar como ele funciona é um tutorial em si. Mas veja algumas coisas básica que você pode fazer com `sed`.

```sh
# Imprime todo o texto recebido, porém substituindo "banana" por "maçã"
sed 's/banana/maça/'
# Imprime apenas as linhas que começam com "Erro" ou "erro"
sed -nr '/^[Ee]rror/p'
# Formata números de telefone: 27988882222 para (27) 9 8888-2222
# '.' poderia ser '[[:digit:]]' para ficar mais específico
sed -r 's/.{2}.{1}.{4}.{4})/(\1) \2 \3-\4/'
```

Infelizmente não tem como eu explicar aqui com detalhes como funciona o `sed`. Mas, pelo menos, o primeiro exemplo você deve ter entendido.
