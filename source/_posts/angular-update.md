---
title: "Atualização para o Angular 2 Final"
description: "Na empresa onde eu trabalho, tivemos que atualizar para a versão final do Angular 2. Neste post eu resumo o que eu aprendi."
tags:
  - angular2
comments: true
share: true
thumbnail: /images/angular2.png
date: 2016-10-15 13:27:00
---


Na empresa em que estou trabalhando, nós estamos construindo uma aplicação usando o Angular 2 no front-end. Nós iniciamos o projeto na versão RC3 do Angular 2 (`2.0.0-rc3`), e agora mais recentemente nós atualizamos para a versão Final do Angular 2 (`2.0.0`). Fazer isso (pelo menos para mim) foi uma tarefa mais difícil do que eu esperava. Então eu resolvi escrever um passo a passo, enquanto eu aprendia a atualizar o projeto.

Como eu disse, fazer a atualização pode dar um pouco de trabalho. Algumas coisas no Router mudaram e isso foi o que mais me trouxe dificuldade. Além de que agora Módulos são usados para cada "pacote de funcionalidade". Você pode conferir [esse Plnkr][exemplo] e o ["Quickstart"][quickstart] para ter uma ideia do que deve ser feito. No passo-a-passo abaixo, eu vou me referir a esses dois o tempo inteiro, então eu espero que você pelo menos dê uma olhada neles antes.

[exemplo]: https://angular.io/resources/live-examples/router/ts/plnkr.html
[quickstart]: https://angular.io/docs/ts/latest/quickstart.html

## Processo de atualização do angular

  1. Colocar os arquivos na nova convenção de nomes: `*.component.{ts,html}`, `*.service.ts`, `*.routing.ts`, `*.guard.ts`, etc. (Opcional)
  2. Colocar o `package.json` do ["Quickstart"][quickstart] no lugar do `package.json` atual e executar `npm install`.
  3. Possivelmente, você deve deletar a pasta "typings" e rodar `npm run postinstall`, se erros não aparecerem nos `*.ts` do projeto. Porque erros vão aparecer!
  4. Seguir o [exemplo][exemplo] para configurar as rotas.
    - Muitos `*.component.ts` terão erro, pois `ROUTER_DIRECTIVES` não existe mais e deve ser retirado de todo o código.
    - `Router`, `ActivatedRoute` e `Params` continuam sendo importados normalmente de `@angular/router`.
    - Diretivas como o `routerLink` são inseridas pelo RouterModule aparentemente, mas não tenho certeza.
  5. Acertar também os arquivos de rotas (`*.routing.ts`):
    - Não existe mais o `RouterConfig`, nem o `provideRouter`.
    - `Routes` substitui `RouterConfig`.
    - No lugar de `terminal: true` (junto com `redirectTo: ...`), agora é usado `pathMatch: 'full'`.
    - É necessário exportar o objeto `whateverRouting`.
    - E o Router de Login exporta `authProviders` contendo os "Guards" e o serviço de login. (acho que isso é opcional.)
  6. É recomendado criar módulos for funcionalidade (ex: `app/alunos`, `app/profs`, etc). O módulo raíz (`app.module.ts`) importa os outros módulos, na seção `imports`. Fazer isso não é necessário. É possível ter um único módulo, mas ter múltiplos módulos está sendo muito usado. Ao optar por não usar esse modelo, o `app.module.ts` pode rapidamente ficar lotado de *Components* e *Services*. Independente da escolha, pelo menos um módulo deve ser criado para a aplicação, chamado `app.module.ts`.
    - Cada Módulo importa os módulos que irá precisar, como: `HttpModule`, `FormsModule`, `CommomMondule`.
    - Cada Módulo importa (`imports`) as rotas pertencentes àquele módulo. Ex: `alunos.module.ts` importa `alunos.routing.ts`.
    - Cada Módulo registra seus componentes na seção `declarations`.
    - Cada Módulo registra os serviços que fornece na seção `providers`.
    - `app.module.ts` deve importar `BrowserModule`, e deve ter uma seção `bootstrap: [ AppComponent ]`.
  7. Criar um `main.ts`. Tudo é igual ao [exemplo][exemplo].
  8. Atualizar o `systemjs.config.js`. (Também igual ao [exemplo][exemplo].)
  9. Nos templates HTML, não é mais possível usar `#id=index`, ou qualquer `#` dentro do `*ngFor`. `let` deve ser usado.

## Exemplo de Módulo

```ts
// whatever.module.ts

import { CommonModule } from '@angular/forms';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http'; // '@angular/http' ?
import { whateverRouting } from './whatever.routing';

@NgModule({
  imports: [ // `imports`: módulos que dependemos.
    CommonModule, // Diretivas como `ngFor` e `ngIf`.
    FormsModule, // Template Driven Forms.
    HttpModule, // Para requisições http.
    whateverRouting, // Rotas desse módulo.
    ...
  ],
  declarations: [ // `declarations`: components, directives e pipes.
    WhateverComponent,
    WhateverPipe,
    ...
  ],
  providers: [ // `providers`: services.
    WhateverService,
    ...
  ],
})
export class WhateverModule {}
```

## Exemplo de rotas

```ts
// whatever.routing.ts

import { ModuleWithProviders }   from '@angular/core';
import { Routes, RouterModule }  from '@angular/router';

import { WhateverComponent } from './whatever.component';
import { WhateverGuard } from '../../whatever.guard';

const whateverRoutes: Routes = [
	{ path: '', redirectTo: '/whatever', pathMatch: 'full' },
	{ path: 'whatever', component: WhateverComponent, canActivate: [ WhateverGuard ] },
];

export const whateverRouting: ModuleWithProviders = RouterModule.forChild(whateverRoutes);
```

O router de login é um pouco diferente:

```ts
// ...
export const authProviders = [
  AuthGuard,
  AuthService
];
```

## Exemplo de uso de *Router* dentro de um *Component*

Os *imports* que eu observei do *router*:

```js
// whatever.component.ts

import { Router, ActivatedRoute, Params } from '@angular/router';

@Component({...})
export class WhateverComponent {
  // Injestão de dependências.
  constructor(
      private service: WhateverService,
      private route: ActivatedRoute,
      private router: Router
    ) {}
}
```
