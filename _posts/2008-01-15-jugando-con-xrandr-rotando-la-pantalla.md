---
id: 22
title: Jugando con xrandr, rotando la pantalla
date: 2008-01-15T12:52:36+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=22
permalink: /2008/01/jugando-con-xrandr-rotando-la-pantalla/
categories:
  - blog
tags:
  - pantallas
  - Scripts
  - software
---
El comando **xrandr** nos permite cambiar resoluciones y disposiciones de
monitores desde la línea de comandos. De hecho, el configurador que gnome tiene
en Ubuntu 7.10 en realidad es un frontend de xrandr.

Lo realmente interesante de xrandr es que nos permite cambiar estas
configuraciones al vuelo. Acciones como cambiar la resolución, cambiar la
frecuencia de muestreo, cambiar la rotación de la pantalla, Incorporar un nuevo
Monitor y la posición de este son ahora posible desde una línea de comandos,
sin reiniciar Xorg.

<!--more-->
Lo primero que hay que hacer es saber que acciones nos permite nuestra interfaz
gráfica. Para ello ejecutamos alguno de estos comandos:

```
xrandr -q
xrandr
```

En mi portatil, este comando tiene la siguiente salida:

```
$ xrandr
Screen 0: minimum 320 x 200, current 1024 x 768, maximum 1024 x 1024
VGA disconnected (normal left inverted right x axis y axis)
LVDS connected 1024x768+0+0 (normal left inverted right x axis y axis) 304mm x 228mm
1024x768       60.0*+
800x600        60.3
640x480        59.9
TMDS disconnected (normal left inverted right x axis y axis)
```

Es decir, no tengo monitor VGA externo conectado, y la pantalla del portátil (LVDS) permite rotar, invertir, reflejar y las resoluciones y frecuencias que se nos indican.
Hay muchas cosas que podemos hacer, para ello nada mejor que ver la ayuda:
```
xrandr --help

```
Y ahora una propuesta curiosa. No, no es algo original, vi algo parecido en la
ubucon 2007, que se celebró en Sevilla.

Si nuestro monitor permite rotaciones cambiar LVDS por nuestro tipo de monitor,
LVDS para portátiles y VGA para pcs de escritorio, puedes mirar las interfaces
que tienes conectadas con el primer comando.

Es una sola línea muy larga, atención:

**NOTA:** _Antes de ejecutarlo, guarda todos tus documentos por si fallan las X o cualquier cosa, no habrá más de 10 cambios, es decir, esto no durará más de 30 segundos._
```
MONITOR=LVDS && M=(left right normal inverted) && for i in 1 2 3 4 5 6 ; do xrandr --output $MONITOR --rotate ${M[$(($RANDOM % 4))]} ; sleep 5; done ; xrandr -output $MONITOR --rotate normal
```

Ahora pongo el mini script un poco más legible y aún volcable en la consola:
```
MONITOR=LVDS \
M=(left right normal inverted) \
for i in 1 2 3 4 5 6 \
do xrandr --output $MONITOR --rotate ${M[$(($RANDOM % 4))]} \
sleep 5\
done\
xrandr -output $MONITOR --rotate normal
```

Con la última línea pones el monitor en modo normal, te recomiendo que la
copies por si tienes que usarla por que no te funcione el script.
