---
layout: post
title: "Jekyll"
description: "Breve introdução ao funcionamento do Jekyll, como ele pode ser usado e para quê."
modified: 2014-08-22 07:27:29 -0300
tags: [jekyll]
comments: true
share: true
---

![](/images/jekyll-logo.png)

Nesse primeiro post eu resolvi escrever sobre a ferramenta (Jekyll) que eu usei para criar o meu primeiro blog, e como ela pode ser útil para vários propósitos.

### O que é o Jekyll?

O [Jekyll][jekyll] é um gerador de websites estáticos. Em poucas palavras, o que ele faz é converter um monte de html, css, javascript, markdown, etc em um website estático. Para usa-lo, você define templates para exibir diferentes tipos de publicações, e define o conteúdo separadamente.

Já que você irá definir a parte bruta através de HTML, CSS e JavaScript, isso te permite ter controle total sobre o estilo do website. Ao mesmo tempo que te permite publicar todo o tipo de conteúdo que você definir.

Você ganha praticidade quando resolve esquematizar o projeto, dividindo-o em templates e conteúdo. Por exemplo, você pode definir um template para posts de um blog, e define o conteúdo dos posts usando uma linguagem mais simples como [markdown][markdown] ou [textile][textile] (markdown é o mais usado). Assim, o Jekyll irá se responsabilizar por processar esse conteúdo e enquadra-lo na definição do seu template de posts. Com isso, o resultado final é uma página html completa com o seu conteúdo. Você pode inclusive ter mais de uma definição de template para posts, e em cada post especificar qual template você deseja usar.

O foco principal do Jekyll está na criação de blogs, mas ele também pode ser usado para projetos mais ambiciosos como por exemplo [o projeto que o grupo Development Seed fez][health-care-developmentseed-post] para criar o portal [HealthCare.gov][health-care] para o governo norte americano.

Ferramentas como essa parecem estar ganhado uma popularidade cada vez maior, como alternativas a CMSs (Content Management System), como o [Wordpress][wordpress] e o [Drupal][drupal]. Pois elimina elementos custosos do desenvolvimento e da manutenção, ao mesmo tempo que te permite um maior controle sobre a estrutura do website. Como exemplo, em um website estático você não precisa se preocupar com um banco de dados. Assim você elimina o custo de ter um, e também de ter pessoas que saibam usá-lo.

Porém, com isso você pode acabar perdendo alguma funcionalidade dinâmica de um CMS, que a princípio necessite de uma aplicação do lado do servidor. Mas você pode facilmente compensar isso usando APIs de terceiros para compor o seu website. Uma funcionalidade comum de um CMS é a gestão de comentários nas páginas. Mas como você poderia alcançar o mesmo efeito em um site totalmente estático? Você pode usar uma API como a do [Disqus][disqus], que vem ganhando bastante popularidade recentemente, ou a do [Facebook][facebook-comments-plugin]. Assim, no final das contas, o que você tem é um pequeno código HTML-JavaScript que você joga em seu template, e assim todas as páginas ganham uma seção de comentários. Uhuul!

Mas fazer a escolha entre utilizar um CMS ou um gerador de sites estáticos é uma decisão delicada, e que deve ser pensada com cuidado, tendo em vista o objetivo final do projeto.

### Personalização do projeto

É possível ainda, acrescentar outras ferramentas no ciclo de vida do seu projeto. Deixando o desenvolvimento mais a seu próprio gosto. Por exemplo, você pode combinar pre-processadores (como o [Less][less] ou [Sass][sass]) para a sua build.

E se você não quiser se preocupar com o desenvolvimento bruto do site, você pode simplesmente escolher um tema já pronto, e se preocupar apenas com o conteúdo. Mas isso te limita a usar o que outras pessoas já criaram.

Existem vários temas para o Jekyll já prontos, a grande maioria deles focada em blogs. Basta escolher um deles, e começar a publicar... como eu fiz ;) . Mas se você tiver os conhecimentos necessários, você pode se aventurar e criar o seu próprio tema, e assim construir tudo completamente personalizado.

E como eu disse, há vários temas espalhados pela internet (você pode dar uma olhada [nesse link][jekyll-themes] para conferir vários deles) feitos por diversas pessoas que resolveram compartilhar seus trabalhos. Há alguns bem simples e outros mais elaborados e de muito bom gosto. Cabe a você escolher.

Mas já adianto que se você pretende criar algo muito diferente de um blog, você provavelmente terá que criar tudo manualmente. Isso não é problema do Jekyll em si, pois ele suporta a geração de diversos tipos de conteúdo, e é possível customiza-lo para o que for necessário. Mas se o que você quer for muito específico é provável que você não encontrará nada pronto pela internet.

Se o Jekyll não te atender em algum aspecto você pode experimentar outros geradores de sites estáticos como: [Octopress][octopress] (usa o Jekyll), [Hexo][hexo.io], [nanoc][nanoc], entre [vários outros][static-site-generators].

### Como usar

Não adianta chorar, você provavelmente terá que sujar as mãos em algum momento e usar a linha de comando. Mas não é difícil como algumas pessoas pensam. Logo, pare de reclamar, se for o seu caso, e siga em frente. ¬¬

Para exemplificar o uso do Jekyll, vou descrever aqui como criar e gerenciar um blog simples, sem nada demais. Assim você terá conhecimento de como ele funciona, e poderá, então, escolher se irá personalizar tudo na mão ou pegar um tema já pronto e pular o trabalho duro. De qualquer forma, é útil ter um conhecimento de como ele funciona.

O primeiro passo de todos é instala-lo na sua máquina. Para isso consulte as [intruções de instalação][install-jekyll] no site oficial. Lá você encontra instruções detalhadas para a instalação na sua plataforma específica.

Porém, um método fácil de instala-lo é usando o RubyGem (gerenciador de pacotes da linguagem Ruby), através de um único comando:

``` bash
# Talvez você tenha que usar sudo
$ gem install jekyll
```

Após isso, basta um comando para criar um novo projeto:

``` bash
$ jekyll new meu-novo-blog
```


Assim o jekyll irá criar um exemplo de um blog bem básico com a estrutura exemplificada abaixo (para servir as páginas em localhost basta executar `jekyll serve`).

``` bash
$ cd meu-novo-blog
$ tree
.
├── about.md
├── _config.yml
├── css
│   └── main.css
├── feed.xml
├── _includes
│   ├── footer.html
│   ├── header.html
│   └── head.html
├── index.html
├── _layouts
│   ├── default.html
│   ├── page.html
│   └── post.html
└── _posts
    └── 2014-08-30-welcome-to-jekyll.markdown
```

Então, permita-me explicar um pouco sobre essa estrutura. `_config.yml` é o arquivo de configuração do seu site, que contém informações como: nome do site, seu e-mail, plugins usados, etc. Esse arquivo utiliza a sintaxe [YAML][yaml]. O diretório `_includes` contém "pedaços de HTML" que você usará para definir seus layouts (templates). Em `_layouts` você deve colocar os seus templates. (Os layouts são páginas quase completas, esperando apenas para receberem conteúdo.) E, finalmente, em `_posts` você define seus posts em uma linguagem simples como o markdown, por exemplo.

Se você investigar os arquivos html que foram gerados (em `_includes/` e `_layouts/`), verá que neles foi usado uma notação diferente no meio do html (um monte de chaves `{}` espalhados). Isso que está sendo usado, são tags [Liquid][liquid]. Elas são [usadas pelo Jekyll para a definição dos templates][jekyll-liquid]. Você precisará entender como elas funcionam para quando você quiser criar os seus próprios templates. Mas não são necessárias quando você estiver escrevendo seus posts.

Você pode notar também que quando executamos `jekyll new`, ele criou uma página inicial (`index.html`) e um `about.md` (que quando processado se tornará `about.html`). Além disso, ele gerou um exemplo de um post para você em `_posts/`.

A essa altura você deve querer visualizar o resultado de tudo isso. Então vamos em frente, e depois voltamos para esclarecer mais algumas coisas. Para gerar o website estático é necessário apenas executar o comando `jekyll build` e o jekyll irá processar todos os seus arquivos e gerar o resultado no diretório `_site`. Isso só já basta para você conseguir o que realmente importa (um website estático), mas vamos fazer um pouco mais do que isso, e vamos executar `jekyll serve`. Assim ele irá servir em localhost os arquivos gerados, para que você possa visualizar o site. Então, depois desse último comando, abra o navegador e entre em `localhost:4000`, por padrão, e veja o seu mais novo blog! Tcharaaam!

Repare que é possível executar `jekyll serve --watch` para que o Jekyll fique "assistindo" o diretório do projeto. E quando qualquer mudança acontecer (quando você editar um post, por exemplo), ele irá gerar novamente os arquivos necessários. O único arquivo que ele não irá assistir é o `_config.yml`. E também repare que não é necessário executar `jekyll build` antes de `jekyll serve`, pois esse último já executa a build automaticamente.

Entenda o que o Jekyll fez. Ele construiu o site, em basicamente duas etapas. Primeiramente, ele tomou todos os arquivos que necessitavam de processamento (os arquivos em markdown e textile) e os processou combinando com os layouts e includes necessários,  gerando arquivos de html para cada um deles. Repare que os diretórios de destino dos posts é diferente do comum, o Jekyll usa o nome do post para criar diretórios de destino para eles. Isso da seguinte forma, se você tem um post chamado `2014-10-19-exemplo.md` ele será processado e irá parar em `_site/2014/10/19/exemplo.html`. Mas fora isso, todos os arquivos vão parar em lugares segundo suas posições relativas ao diretório raiz do projeto (exemplo: `/about/index.md` é processado e vai parar em `_site/about/index.html`) da forma como você esperaria.

A segunda etapa da build é mais simples. Ele copia todos os arquivos que não necessitam de processamento, como imagens, css, javascript, etc, para o diretório `_site/`. Então, por exemplo, um arquivo `/imagens/foto.jpg` vai parar em `_site/imagens/foto.jpg`).

### YAML Front Matter

Mas que tipo de arquivos são processados pelo Jekyll exatamente? E quais são simplesmente copiados sem processamento? Por definição do Jekyll, somente são processados os arquivos com extenções do markdown ou textile, e os arquivos que tenham um cabeçalho especial, chamado de ["YAML Front Matter"][front-matter]. Quando o Jekyll detectar esse tipo de cabeçalho ele irá processar o arquivo. Um "front matter" deve ser colocada logo no começo do arquivo e se parece com o seguinte:

```
---
layout: post
title: "Jekylll"
modified: 2014-08-22 07:27:29 -0300
tags: [jekyll]
comments: true
---
```

A sintaxe utilizada para a codificação do "Front Matter" é a [YAML][yaml] (assim como o arquivo `_config.yml`). E nessa seção você pode definir variaveis que serão utilizadas pelo Jekyll (como `layout` e `title`). E, até mesmo, variáveis personalizadas, como é o caso do exemplo acima em que a variável `comments` é utilizada pelo meu tema atual, para saber quando ele deve incluir uma seção de comentários, ou não, em um post.

E se você quiser que o arquivo seja processado, mas ele não precisa de nenhuma variável, você ainda assim precisa de incluir o front matter. Mas pode deixa-lo vazio, sem nenhuma variável. Isso pode ser útil quando você quer usar Liquid Tags em arquivos css e javascript, por exemplo.

### Hospedagem no Github Pages

Ok... mas e depois que você criar o seu website e quiser publicá-lo na internet? Você pode optar por várias possibilidades, inclusive a de usar o seu próprio servidor pessoal. Porém, o que eu considero mais prático, no momento, é utilizar a hospedagem gratuita do [Github Pages][github-pages]. Para mim, a forma mais simples para publicar seu site, é você criar um repositório na sua conta do GitHub chamado `<seu-nome-de-usuario>.github.io` e subir o seu projeto do Jekyll para o novo repositório. O GitHub irá processar o seu projeto do Jekyll e gerar o seu website estático no endereço `<seu-nome-de-usuario>.github.io`. Simples assim. Apenas preste atenção na versão do Jekyll que você está usando e a versão que está sendo usada pelo GitHub (confira [aqui][github-pages-versions]). Mas isso não deve ser um problema.

Se preferir, você pode construir o site na sua máquina e subir o resultado final para o GitHub Pages. Isso é uma outra possibilidade. Assim, você tem controle de qual versão do Jekyll utilizar, e também usar plugins em sua build (coisa que o github bloqueia para evitar que plugins maliciosos executem). Você pode até usar um domínio personalizado se quiser. Para entender melhor como o GitHub Pages funciona, leia a [página oficial do Github Pages][github-pages].

### O que fazer depois?

Se você não sabe para onde ir agora, e o que fazer para aprender mais, eu te aconselho a brincar com o blog que criamos nesse tutorial. Crie um novo post; dê uma olhada em como funcionam os layouts e includes; futuque as Liquid Tags e veja como elas funcionam; etc.

Se você ainda tiver dúvidas ou quiser entender melhor as coisas, eu recomendo que você dê uma olhada na [documentação do Jekyll][jekyll-docs] (em inglês). A documentação é boa, e fala sobre vários aspectos da ferramenta. E tudo está escrito de forma clara e concisa.

É isso aí! Até mais. :)

[static-site-generators]: http://staticsitegenerators.net/
[jekyll]: http://jekyllrb.com/
[octopress]: http://octopress.org/
[hexo.io]: http://hexo.io/
[nanoc]: http://nanoc.ws/

[health-care]: https://www.healthcare.gov/
[health-care-developmentseed-post]: http://www.developmentseed.org/blog/new-healthcare-gov-is-open-and-cms-free/

[less]: http://lesscss.org/
[sass]: http://sass-lang.com/

[jekyll-themes]: http://jekyllthemes.org/

[drupal]: https://www.drupal.org/
[wordpress]: http://wordpress.org/

[disqus]: https://disqus.com/
[facebook-comments-plugin]: https://developers.facebook.com/docs/plugins/comments

[markdown]: http://en.wikipedia.org/wiki/Markdown
[textile]: http://en.wikipedia.org/wiki/Textile_%28markup_language%29

[install-jekyll]: http://jekyllrb.com/docs/installation/

[yaml]: http://pt.wikipedia.org/wiki/YAML
[liquid]: http://liquidmarkup.org/
[jekyll-liquid]: http://jekyllrb.com/docs/templates/
[front-matter]: http://jekyllrb.com/docs/frontmatter/

[github-pages]: https://pages.github.com/
[github-pages-versions]: https://pages.github.com/versions/
[jekyll-docs]: http://jekyllrb.com/docs/home/
