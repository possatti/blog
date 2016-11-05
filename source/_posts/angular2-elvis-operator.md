---
title: "Elvis Operator - Angular 2"
description: "No Angular 2, o Elvis Operator, é um operador usado para acessar uma propriedade de um objeto, apenas quando ele não é nulo."
tags:
  - angular2
comments: true
share: true
thumbnail: /images/angular2.png
date: 2016-11-04 22:18:00
---

Outro dia no trabalho, eu precisei colocar atributos de um objeto em um formulário usando o Angular 2. Porém eu não inicializava o objeto de imediato. O objeto só seria inicializado quando um serviço retornasse a reposta do servidor. E em função disso a *view* não renderizava, devido a um erro quando o Angular tentava acessar o atributo de um valor nulo. Buscando uma solução para o problema, eu acabei me deparando com o "*Elvis Operator*" (`?.`). Que, devo admitir, me chamou bastante a atenção pelo nome curioso.

O [*Elvis Operator* não é um conceito novo][wiki-elvis-operator]. Ele já é usado em algumas linguagens de programação, como C++, Groovy, PHP e outras. E antes que você me pergunte, eu já te aviso que ele não é um operador nativo do Javascript. Ele realmente só funciona dentro dos templates HTML do Angular 2. Vale a pena ler a [documentação oficial][docs-safe-navigation].

[wiki-elvis-operator]: https://en.wikipedia.org/wiki/Elvis_operator

E note que há algum tempo atrás o *Elvis Operator* do Angular foi [renomeado][rename-elvis-operator] para "*Safe Navigation Operator*" (Operador de Navegação Segura), que é um nome muito mais chato, convenhamos... *Elvis Operator* é um nome muito mais legal. Só dizendo.

<!--
**Obs:** Se eu entendi certo, talvez *Elvis Operator* seja sequer o termo correto para essa operação. Possivelmente o nome mais apropriado para essa operação seja *null-safe dereferencing*. Aparentemente existe uma [confusão entre os dois termos][the-true-elvis].

[the-true-elvis]: http://mail.openjdk.java.net/pipermail/coin-dev/2009-July/002089.html
-->

No Angular 2, esse operador é usado para acessar um atributo de um objeto, apenas quando o objeto não é nulo. Enquanto ele for nulo, nada é acessado. Assim nós não nos deparamos com um erro, e a aplicação continua funcionando normalmente. Imediatamente quando o valor deixa de ser nulo, o atributo será acessado e renderizado na tela.

Para obter esse efeito poderíamos usar uma expressão como `carro && carro.cor`, dessa forma `carro.cor` seria acessado apenas se `carro` não fosse nulo. Essa expressão funciona, porém utilizando o *Elvis Operator* podemos simplificar a expressão para apenas `carro?.cor`.

Vamos a um exemplo mais completo. Suponhamos um componente como o abaixo:

```ts
@Component({
  moduleId: module.id,
  selector: 'example',
  templateUrl: 'example.component.html'
})
export class ExampleComponent implements OnInit {

  private carro: Carro;

  constructor(private exampleService: ExampleService) {}

  ngOnInit() {
    // Busca a instância de carro no servidor.
    this.exampleService.getCarro(1)
      .subscribe(carro => this.carro = carro);
  }
}
```

E que usa o seguinte template HTML:

```html
<h3>Detalhes do Carro</h3>

<p>ID: {{ carro.id }}</p>
<p>Marca: {{ carro.marca }}</p>
<p>Modelo: {{ carro.modelo }}</p>
<p>Cor: {{ carro.cor }}</p>
```

Ao iniciar a aplicação vamos ter um erro, pois o Angular tentará ler a propriedade `id` de `carro`, sendo que inicialmente `carro` é nulo, pois quando o componente é renderizado na tela, não deu tempo do *browser* terminar a requisição HTTP e buscar o objeto. Teremos uma mensagem semelhante ao seguinte: `cannot read property 'id' of undefined`.

Podemos resolver isso, ao utilizar o *Elvis Operator* (`?.`). No exemplo abaixo, ele verifica se `carro` é nulo: se for nulo, ele *deixa quieto*; porém se carro realmente for um objeto, só então, ele tentará acessar os respectivas atributos de `carro`.

```html
<h3>Detalhes do Carro</h3>

<p>ID: {{ carro?.id }}</p>
<p>Marca: {{ carro?.marca }}</p>
<p>Modelo: {{ carro?.modelo }}</p>
<p>Cor: {{ carro?.cor }}</p>
```

Porém é necessário ter cuidado ao usar o *Elvis Operator* juntamente com o *two-way data binding*, o que não é suportado. Isso pode acontecer ao usarmos a diretiva `ngModel` em um formulário, por exemplo.

```html
<h3>Cadastrar novo Carro</h3>

<form>
  <label>Marca</label>
  <input type="text" [(ngModel)]="carro?.marca">
  <label>Modelo</label>
  <input type="text" [(ngModel)]="carro?.modelo">
  <label>Cor</label>
  <input type="text" [(ngModel)]="carro?.cor">
</form>
```

No exemplo acima teremos um erro: `The '?.' operator cannot be used in the assignment at column 5 in [carro?.marca=$event] in ExampleComponent`. Isso acontece pois, ao fazer o *two-way binding*, o Angular precisa colocar valores dentro de `carro?.marca`, e isso não é suportado usando o *Elvis Operator* (ainda). É possível contornar o problema quebrando o `[(ngModel)]` em duas partes, como no exemplo a seguir.

[issue-two-way-binding]: https://github.com/angular/angular/issues/7697


```html
<form>
  <label>Marca</label>
  <input type="text" [ngModel]="carro?.marca" (ngModelChange)="carro.marca=$event">
  <label>Modelo</label>
  <input type="text" [ngModel]="carro?.modelo" (ngModelChange)="carro.modelo=$event">
  <label>Cor</label>
  <input type="text" [ngModel]="carro?.cor" (ngModelChange)="carro.cor=$event">
</form>
```

Isso funciona, mas na *minha opinião* fica verboso demais. E no final das contas, acaba parecendo uma gambiarra. Repare também que existe uma *issue* aberta para que o [*Elvis Operator* seja suportado pelo *two-way data binding*][issue-two-way-binding]. (Quem sabe no futuro ele não funciona com o `[(ngModel)]`?)

De qualquer forma, no meu trabalho, ao invés de usar o *Elvis Operator*, eu achei mais sucinto e elegante criar um objeto vazio para ser temporariamente renderizado, enquanto o objeto com os dados verdadeiros ainda não é carregado. Para isso eu alterei o meu componente para o seguinte:

```ts
@Component({
  moduleId: module.id,
  selector: 'example',
  templateUrl: 'example.component.html'
})
export class ExampleComponent implements OnInit {

  // Um objeto vazio para servir de "placeholder".
  private carro: Carro = new Carro();

  constructor(private exampleService: ExampleService) {}

  ngOnInit() {
    this.exampleService.getCarro(1)
      .subscribe(carro => this.carro = carro);
  }
}
```

Assim, a tela é carregada usando os atributos do objeto vazio. Porém, logo que o objeto real chega, a tela é alterada para refletir os valores do objeto verdadeiro. E isso **sem** usar o *Elvis Operator*.

Concluindo, o *Elvis Operator* pode ser muito útil em algumas situações. Mas, pelo menos na **minha opinião**, acaba com cara de gambiarra. Apesar de que, se ele está presente na documentação oficial com certeza tem usos legítimos. Mas eu sinto que uma solução mais simples e elegante é renderizar um objeto temporário na tela que será substituído pelo objeto verdadeiro depois.

[docs-safe-navigation]: https://angular.io/docs/ts/latest/guide/template-syntax.html#!#safe-navigation-operator

[rename-elvis-operator]: https://reviews.angular.io/R6:4cfff4148523b67eb204225f89bff3485e44ce4f

<!--
Links não utilizados:

[cheatsheet]: https://angular.io/docs/ts/latest/guide/cheatsheet.html
[stackoverflow-question]: http://stackoverflow.com/questions/35161789/correct-use-of-elvis-operator-in-angular2-for-dart-components-view
[elvis-on-syntaxsuccess]: http://www.syntaxsuccess.com/viewarticle/elvis-operator-in-angular-2.0
-->
