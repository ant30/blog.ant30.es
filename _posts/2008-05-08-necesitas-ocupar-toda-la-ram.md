---
id: 37
title: ¿Necesitas ocupar toda la ram?
date: 2008-05-08T14:25:39+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=37
permalink: /2008/05/necesitas-ocupar-toda-la-ram/
categories:
  - blog
tags:
  - Linux
  - misscripts
  - software
  - yaco
---
Alguna vez me ha pasado que he necesitado ocupar toda la ram para ver como se comportaba un proceso que usara la swap. En los sistemas antiguos, con 64, 128 o incluso 521MB de ram era medianamente fácil abriendo firefox y poco más. Pero ahora, con el _pepino_ este que tengo, un quad core con 4gigas de ram, no hay quien llene esa RAM.

Hay un montón de recetas para hacerlo, pero existe una muy efectiva. En mi caso, en 10 segundos tenía ocupados 3.5 gigas de ram. No necesitaba una cantidad en concreto, solo aproximado, algo que hiciera usar swap y más swap y enviase a unos programas de testeo a la muerte del rendimiento de la swap. El comando es el siguiente:

```
a=$(yes;read)
```

Ejemplo de procedimiento:

  1. Abrir una terminal y ejecutar top, dejar parada la ordenación por ram ocupada y la tecla k para matar el pid que le digamos, con esto controlamos la ram ocupada.
  2. En otra terminal, poner el comando antes citado: `a=$(yes;read)`
  3. Mirar la ventana top: Si matamos el proceso yes, la ram dejará de crecer, si matamos la bash, la que ocupa ram, la ram se liberará

¿Te atreves a probarlo?
