---
id: 40
title: Synergy, un kvm de ratón y teclado por RED
date: 2008-06-04T14:16:48+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=40
permalink: /2008/06/synergy-un-kvm-de-raton-y-teclado-por-red/
categories:
  - blog
tags:
  - Aplicación
  - software
  - yaco
---
En la oficina donde trabajo, en el equipo en el que participo, tenemos una
máquina que la usamos para acceder a una VPN de un cliente. En la nueva
oficina, este PC ha caído justo al lado mía, pero para acceder tengo que o
bien, mover la silla y coger usar una mesa que en realidad no es una mesa, no
hay donde meter las piernas. O bien, usar un kvm para intercambiar teclados y
ratón, entre mi PC y ese. ¿Buscar un kvm para 4 cosas y encima solo por no
mover la silla?. Hay una solución sin gastarse un solo euro.

Googleando, llegué a una solución bastante útil, que además, me ha resuelto
directamente otros 2 casos iguales que tengo en mi propia casa.
[Synergy2](http://synergy2.sourceforge.net/) hace lo que haría un PC con dos
monitores y dos sesiones de X diferentes. Es decir, cambiamos el foco de
monitor al mover el ratón de una sesión a otra. Pero el caso, y es lo que más
me ha sorprendido, la máquina de la VPN usa windows y mi máquina Linux. Sin
problemas, en la misma web también podemos encontrar el cliente/servidor para
windows. También hay cliente para OSX.

Además para los que usamos ubuntu, tenemos la documentación en la propia wiki
de ubuntu en un bonito [How to](https://help.ubuntu.com/community/SynergyHowto)
en Inglés. Tenemos el paquete en los repositorios universe.

Es impresionante, ahora giro la pantalla de la otra mesa, sin levantarme de la
mesa, muevo el ratón hacia la derecha y ya estoy en ese escritorio, sin usar
VNC ni similares que sobrecarguen la red.

Como problema puede tener, y lo tiene es el de la seguridad. Por defecto, el
programa no cifra ninguna comunicación, sin embargo, en la misma web tienen un
guía de como hacerlo pasar a través de ssh evitar estos riesgos.

El programita en cuestión tiene ya bastante tiempo, está posteado en
genbeta.com con fecha de 2005. ¿Te atreves a probarlo?
