---
title: "Como implementar un remote theme en Jekyll"
date: 2020-09-20T16:00:00-05:00
categories:
  - Jekyll
tags:
  - GitHub
  - Remote Themes
toc: true
toc_label: "En esta página"
related: true
header:
  teaser: /assets/images/Jekyll.png
  
---


Hace un par de semanas me propuse hacer porfín una pagina web, Lo hice a través de *Jekyll* y estas son algunas cosas que me hubiese gustado saber acerca de como implementar un remote theme en Jekyll.
{: .notice}

<!--more-->

## Que es una static site?

Una Static site es una pagina web que se muestra al usuario exactamente como la tiene almacenada el creador de esta, sin una web aplication de intermediario.

### Ventajas de las static sistes

A diferencia de las páginas web dinámicas, requieren muchas menos actualizaciones, herramientas, y mantenimiento en general.
[Jack Dougherty], profesor de Trinity College explíca a detalle qué lo motivó a cambiar su página web de **Wordpress** a **Github Pages** en este [post][Migrating Jack].

Es más sencillo hostearlas y por eso mismo es que se pueden encontrar algunas empresas que brindan este servicio de forma gratuita, como lo hace **Github** con la popular [Github Pages]. Ahora es cuando entra [jekyll] en escena, porque es una herramienta que ayuda a crear páginas web estáticas y está optimizada para usarse con Github.


## jekyll

### Instalación

En la página de Jekyll están las instrucciones para la correcta instalación de jekyll.

-[Jekyll Instalation](https://jekyllrb.com/docs/installation/)

### Templates

Github ofrece unos templates súper básicos para hacer una pagina web con Jekyll en Github Pages.

<figure>
	<a href="https://pages.github.com/themes/"><img src="/myweb/assets/images/Jekyll-Themes.jpg"></a>
	<figcaption><a href="https://pages.github.com/themes/" title="Temas default de Github Pages">
  </a>.
  </figcaption>
</figure>

Éstos tienen la ventaja que se pueden instalar en el documento `_config.yml` directamente desde la variable `Theme`.

Pero existen Templates mucho más interesantes creados y publicados por usuarios de Github, que se pueden usar, pero ahora mediante la variable de `_config.yml` llamada `remote_theme`.

Puedes encontrar muchas de estas en esta página:

-[Jekyll Themes en rugygems](https://rubygems.org/search?utf8=%E2%9C%93&query=jekyll+theme)

La que yo estoy usando en este blog se llama [Minimal Mistakes], es muy utilizada, y esta excelentemente bien documentada y se puede implementar de bastantes formas, la más sencilla de todas, a mi parecer, es creando un repositorio en Github a través de [este enlace][remote theme starter].

[Mike-Dane]: https://www.youtube.com/watch?v=T1itpPvFWHI&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB
[Bill-Raymond]: https://www.youtube.com/watch?v=EvYs1idcGnM&list=PLWzwUIYZpnJuT0sH4BN56P5oWTdHJiTNq
[Jack Dougherty]: https://jackdougherty.org/
[Migrating Jack]: https://jackdougherty.org/2019/02/17/wordpress-to-jekyll-github/
[Github Pages]: https://pages.github.com/
[Jekyll]: https://jekyllrb.com/
[Minimal Mistakes]: https://mmistakes.github.io/minimal-mistakes/
[remote theme starter]: https://github.com/mmistakes/mm-github-pages-starter/generate
