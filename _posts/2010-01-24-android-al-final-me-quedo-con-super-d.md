---
id: 65
title: 'Android: Al final me quedo con Super D'
date: 2010-01-24T13:16:32+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=65
permalink: /2010/01/android-al-final-me-quedo-con-super-d/
categories:
  - blog
tags:
  - android
---
Este fin de semana ha sido un caos de tantas pruebas de roms que he hecho gracias a los amigos de los foros de [xda-developers](http://forum.xda-developers.com/forumdisplay.php?f=448).

¿Porqué cambia uno de rom? La respuesta es fácil, hay ciertas necesidades que no se me cumplen con la rom que usaba mi dispositivo. Llevo usando los firms de cyanogen desde que dude y haykuro dejaron de aparecer. Sin embargo, en los últimos tiempos, sentía como la ROM iba a pedales. No es normal que al empezar a escribir con el teclado, tengas un lag de varios segundos y que la carga del sistema esté rondando entre 2 y 3 casi todo el día. Así como que la swap estuviese casi siempre saturada. Lógicamente, todo esto hace que la experiencia Android se convierta en un poco desagradable. Así que tocaba probar con los siguientes requisitos:

  * El bluetooth debe de funcionar de forma completa, y la app del market, bluetooth file transfer funcione tanto para los ficheros como para los contactos. Y lo más importante de bluetooth en mi caso es que sea capaz de conectarse al manos libres del coche, un Parrot CK3100 Evolution con firm 5.11c
  * Velocidad de respuesta y estabilidad. No me importa que no siempre valla a más de 25fps todas las animaciones, ni que estén activadas todas los efectos. Pero sí que quiero tener la sensación de que el terminal responde a mis dedos y no que lo hace unos cuantos segundos después.
  * Conectividad wifi. La mayoría de firms tienen el mismo soporte de wifi, con que funcione es más que suficiente.
  * Por supuesto, deben funcionar la localización y My Maps Editor, así HTC_IME como teclado virtual aunque sea instalándolo mediante adb. Este último debe funcionar con soltura.
  * La home debe responder al acelerómetro para cambiar de vista horizontal a vertical y viceversa.

He probado varios firms Eclair tanto 2.0 como 2.1 y la mayoría de ellos me han dejado buen sabor de boca. Muchas de ellas ya sabía que no tenían algunos de los requisitos. Como el funcionamiento de bluetooth, el funcionamiento de la cámara, la no grabación de vídeo existente aún en las 2.x para el Dream y la falta de drivers 2D/3D para estas versiones. Sin embargo, la velocidad del termina era bastante aceptable. Incluso el paso de horizontal a vertical y viceversa tenían buena velocidad. Sin embargo, para la mayoría de firms de estas versiones, el soporte de bluetooth no parece estar completo.

Tras realizar pruebas con las 2.x estaba claro que tenía que pasar a firms 1.6 si quería estabilidad y entonces fue cuando usé la recomendación de [@hugopvigo](http://twitter.com/hugopvigo) de usar Super D. Esta ROM tiene el hack de los 10M más de RAM, soporte de swap y en general es como una cyanogen pero los binarios han pasado por un [zipalign](http://developer.android.com/guide/developing/tools/zipalign.html). Lo que mejora sustancialmente el rendimiento del sistema, al estar los apk y compañía más optimizados para su ejecución.

Con esta firm desde cero, wipe + ext3 en sd formateada, me ha funcionado el bluetooth sin hacer nada, simplemente instalando la aplicación que he citado antes. He podido enviar unos cuantos contactos al Parrot y encima el móvil, se desenvuelve con normalidad aún ejecutando unos 12-14 servicios.

En concreto estoy usando la versión estable 1.6 de las publicadas en este [hilo de xda-developers](http://forum.xda-developers.com/showthread.php?t=613809)
