---
id: 49
title: Dedicando algo de tiempo al blog
date: 2008-10-12T11:24:08+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=49
permalink: /2008/10/dedicando-algo-de-tiempo-al-blog/
categories:
  - blog
tags:
  - blog
---
Gracias a la lluvia de hoy, me he quedado en casa. Sí, ya lo sé, es domingo,
pero después de ver la fantástica carrera de Alonso, tenía una gran tarea
pendiente, actualizar cosas en el blog.

Algunas cosas eran estrictamente necesarias, como actualizaciones de seguridad
de mucho de los módulos que tenía instalado, también tenía que incorporar el
shadowbox 2.0 ahora que está terminado. Y además, un resaltador de código.

Y como siempre, he encontrado algún que otro problemilla. Por ejemplo, tengo en
mi casa un chroot con una configuración casi idéntica al del servidor donde se
aloja este blog, pero por lo que se ve, se queda en eso, en &#8220;casi&#8221;.
En los themes de drupal tenemos un fichero que se suele llamar nombretema.info
y en el aparece información sobre la compatibilidad, módulos requeridos,
ficheros javascripts, css &#8230; pero al final, que pasa ?. Pues tenía mis
.info bien puesto para que de esta forma el js se comprimiese y todo eso, pero
no, en el server no funciona, así que están puesto con script + src a pelo. Es
simplemente un mal menor, pero tendré que verlo algún otro día con más
detenimiento.

Probamos el coloreador de código?

## Prueba python

```
def hola_mundo(mensaje="hola"):
print mensaje
hola_mundo()
hola_mundo("hola mundo")
```

## Prueba bash

```
#!/bin/bash
set -e
imprime (){
echo "mensaje: $1";
}
for N in `seq 1 10`;
do
imprime "hola mundo";
done
```
Le faltan cosas como la pestaña para copiar código o por lo menos que se pueda
ver en plano para que al copiar y pegar no estén los números de línea
