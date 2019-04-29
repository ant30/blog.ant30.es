---
id: 25
title: 'Vim: desactivando el auto-identado al vuelo'
date: 2008-02-03T15:37:59+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=25
permalink: /2008/02/vim-desactivando-el-auto-identado-al-vuelo/
categories:
  - blog
tags:
  - software
  - vim
---
Suelo usar vim como editor a menudo. Es muy fácil controlar las funciones básicas, y siempre se encuentra instalado vim o vi en cualquier servidor Linux y en la mayoría de Unixes varios.
Uno de los principales engorros hasta ahora los tenía al pegar texto. Vim tiene implementadas funciones para detectar el autoidentado según el la extensión del fichero, de esta forma, sabe donde si tiene que introducir 4 espacios más de separación al terminar una línea que empieza por &#8220;def&#8221; en python, por citar un ejemplo.
Sin embargo, esta ventaja no es siempre tal ventaja, sobre todo cuando uno se dispone a pegar texto. Al pegarlo, suele incorporar un tabulador de más por cada línea, de forma que el texto queda en diagonal, algo completamente horrible.
¿Una solución?


este comando:
```
:set pastetoggle=<F11>
```
De esa forma, al pulsar en F11 desactivamos la autoindentación y se activa el modo:
```
INSERTAR (pegar)
```

Es decir, insertar sin autoindentación, ideal para pegar. Lógicamente, puedes
cambiar F11 por la tecla que quieras, e incluso añadirlo en tu fichero .vimrc
para que quede salvada de forma permanente.

Tenemos que pulsar una tecla más, pero al menos no tenemos que reordenar todo
el texto.

**NOTA:** información que proviende de [el foro de trucos(tips) de
vim](http://www.vim.org/tips/tip.php?tip_id=330)
