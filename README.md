# erreme.github.io ou erre.me

[![Build Status](https://travis-ci.org/erreme/erreme.github.io.svg?branch=source)](https://travis-ci.org/erreme/erreme.github.io)

Este é o repositório com o código fonte de todos os arquivos do site,
incluindo as páginas com o conteúdo do curso.

O site é construído com o [Octopress][], que por sua vez é
baseado no [Jekyll][], e hospedado no [GitHub][].

## Instruções de instalação e modificação

Para instalação e utilização básica do Octopress, siga os passos de
configuração inicial descritos [aqui][1].

Para criar uma nova página com conteúdo, faça

```shell
rake new_page[nome]
```

Isto irá criar um novo diretório dentro de `source`, por exemplo
`source/nome` com um arquivo `index.markdown`. Como vamos usar o
[knitr][] para escrever em R Markdown e converter para Markdown,
deveríamos simplesmente renomear este arquivo para `index.Rmd` e editar
normalmente. **No entanto**, na hora de construir o site, o Jekyll vai
tentar interpretar este arquivo (com extensão `.Rmd`) como se fosse um
`.md` e a página não será convertida para HTML. (Mesmo depois de existir
um arquivo `.md` gerado corretamente pelo knitr).

Para contornar este problema, renomeie o arquivo `index.markdown` para
`index.Rmk` (ao invés de `index.Rmd`), ou seja,

```shell
mv source/nome/index.markdown source/nome/index.Rmk
```

Dessa forma, o Jekyll não vai se preocupar coom esse arquivo `.Rmk` e
irá processar o arquivo `.md` gerado pelo knitr. O inconveniente disso é
que a extensão usual para arquivos RMarkdown é `Rmd` e alguns editores
não vão reconhecer esse extensão apropriadamente. No Emacs, usando o
[polymode][] isto é fácil de contornar, veja por exemplo esse
[.emacs][2].

Finalmente, para converter esse arquivo `.Rmk` para `.md` utilizando o
knitr, fazemos (no R):

```r
library(knitr)
knit("index.Rmk", "index.md")
```

Dessa forma, o site agora pode ser gerado normalmente.

> NOTA: este problema com a extensão do arquivo Rmd tem outras soluções,
> como podem ser vistas no blog de [Jason Bryer][3].

### Padrões do knitr para arquivos `.Rmk` (`.Rmd`)

Depois de renomear o arquivo para `index.Rmk`, mantenha o header padrão
do YAML gerado automaticamente (que já está lá), e logo abaixo inclua
estas configurações globais para o knitr:

```r
```{r setup, include=FALSE, cache=FALSE}
opts_chunk$set(fig.path = "../images",
               fig.align = "center",
               fig.width = 8,
               fig.height = 6,
               out.width=".8\\textwidth",
               prompt = FALSE,
               comment = NA,
               tidy = FALSE,
               cache = TRUE)
```
```

De todas estas opções, a principal (e obrigatória!) é a `fig.path =
"../images"` que fará com que todas as figuras geradas sejam salvas
automaticamente no diretório `source/images`, que é aonde o Jekyll vai
procurar as imagens para incluir no site.

Se não houver essa opção, as figuras são salvas no diretório `figures`,
no local padrão (nesse caso em `source/nome/figures`) e o Jekyll não
encontrará a figura.

> NOTA: Ver se existe a possibilidade de alterar o padrão do Jekyll.



## Licença

[Octopress][] possui licença [MIT][].

O **conteúdo** do site é

Copyright © 2014 Fernando de Pol Mayer e Rodrigo Sant'Ana

e licenciado sob uma licença
[Creative Commons Atribuição-Compartilha Igual 3.0 Não Adaptada][CC].

<a rel="license"
href="http://creativecommons.org/licenses/by-sa/3.0/deed.pt_BR"><img
alt="Licença Creative Commons" style="border-width:0"
src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" /></a>

[Octopress]: https://github.com/imathis/octopress
[Jekyll]: http://jekyllrb.com
[GitHub]: http://www.github.com
[knitr]: http://yihui.name/knitr/
[polymode]: https://github.com/vitoshka/polymode
[MIT]: http://opensource.org/licenses/MIT
[CC]: http://creativecommons.org/licenses/by-sa/3.0/deed.pt_BR
[1]: http://fernandomayer.github.io/blog/2013/03/02/configuring-octopress/
[2]: https://github.com/fernandomayer/emacs-files/blob/master/emacs.el
[3]: http://jason.bryer.org/posts/2012-12-10/Markdown_Jekyll_R_for_Blogging.html
