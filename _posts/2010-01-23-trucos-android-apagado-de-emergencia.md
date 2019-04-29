---
id: 64
title: 'Trucos Android: Apagado de emergencia'
date: 2010-01-23T13:29:16+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=64
permalink: /2010/01/trucos-android-apagado-de-emergencia/
categories:
  - blog
tags:
  - android
---
No es la primera vez que tras instalar un nuevo firmware, mi maltrecho HTC Dream no termina de arrancar. Al principio de mis tiempos con android, lo primero que se me ocurría era quitar la batería. Desde luego es la solución más drástica y que seguro que funciona. Pero claro, si desmontas unas diez o veinte veces la parte trasera del móvil simplemente para sacar la batería, la tapa cogerá holgura.
En el arranque, tras haber pasado ya la fase de inicio en la que vemos el logo de la compañía, en mi caso Movistar, el terminal ya es capaz de recibir comandos vía adb. Esto nos permite hacer ciertas operaciones mientras el móvil arranca, y en el caso que ahora nos interesa podemos realizar dos importantes acciones que son apagar o reiniciar el móvil.
Para ejecutar estas acciones desde el pc conectado por usb al móvil y sabiendo de antemano que tenemos adb funcionando tendríamos que lanzar los siguientes comandos:

Reiniciar:
:   `$ adb shell reboot`

Apagar:
:   `$ adb shell reboot -p`

Un ejemplo para el que necesitamos el comando de apagado es cuando una rom no termina de arrancar para poder llegar a la pantalla de restauración o recovery. A esa que llegamos si arrancamos el androide dejando pulsado el botón de la casita y a su vez el de encendido.

<!--more-->
