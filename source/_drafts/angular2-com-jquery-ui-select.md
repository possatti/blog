---
title: "Usando o select do jQuery UI com Angular 2"
description: "Fazer o selectmenu do jQuery UI funcionar em um app do Angular 2 pode ser bem complicado. Mas eu explicarei como eu fiz os dois funcionarem juntos."
tags:
  - angular2
  - jquery
comments: true
share: true
thumbnail: /images/angular2.png
date: 2017-01-14 15:23:00
---

Em um sistema que estamos implementando no trabalho, nós tivemos que utilizar o [`selectmenu` do jQuery UI][selectmenu-doc] dentro de uma aplicação Angular (versão 2). E para que a utilização ficasse fácil dentro do Angular, tentamos criar um Component para isso. Acabou que essa foi uma tarefa muito mais árdua do que havíamos imaginado.

[selectmenu-doc]: https://jqueryui.com/selectmenu/

Eu fui o responsável por criar tal componente. Inicialmente, eu queria usar o mínimo de jQuery possível. Queria apenas chamar a função `selectmenu({...})` e fazer o resto com Angular. Mas depois de inúmeras tentativas fracassadas, meu colega de trabalho me deu o conselho de usar **mais** jQuery, ao invés de menos. E realmente ao implementar o componente inteiro usando quase que apenas jQuery, tudo funcionou. Por isso eu escrevo esse post. Caso você caia na mesma situação que eu, agora você já sabe o que fazer.

Vou explorar passo-à-passo aqui, como criar o componente. Primeiro o armação básica:

```ts
import { Component } from '@angular/core';

@Component({
	moduleId: module.id,
	selector: ui-select,
	template: `
		<select>
			<option></option>
		</select>
	`
})
export class UiSelectComponent { }
```

Essa é a sintaxe básica para criar um componente. Se quisermos usá-lo juntamente com a diretiva `ngModel`, precisamos adicionar mais algumas coisas:

```ts
@Component({
	moduleId: module.id,
	selector: 'ui-select',
	template: `
		<select>
			<option></option>
		</select>
					`,
	providers: [
		{
			provide: NG_VALUE_ACCESSOR,
			useExisting: forwardRef(() => UiSelectComponent),
			multi: true
		}
	]
})
export class UiSelectComponent implements ControlValueAccessor {

	/* Esse é o valor que está selecionado atualmente. */
	value: any;

	writeValue(value: any) {
		this.value = value;
	}

	private propagateChange = (value: any) => {};
	registerOnChange(fn: (value: any) => {}) {
		this.propagateChange = fn;
	}

	registerOnTouched() {}
}
```

```ts

```

```ts

```
