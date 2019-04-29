---
id: 12
title: Apagar el monitor (modo stand-by)
date: 2007-11-06T13:47:38+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=12
permalink: /2007/11/apagar-el-monitor-modo-stand-by/
categories:
  - blog
tags:
  - bah
  - Linux
---
Para apagar el monitor o lanzar el salvapantallas tenemos la posibilidad de
hacerlo mediante comandos. Entonces espero que me preguntes ¿Para qué?

Imagínate que quieres asociar una tecla para apagar el monitor, y otra para
bloquear la pantalla (fondo negro o salvapantallas + petición de password de
usuario). En este caso, en gnome, tenemos la posibilidad de decirle a metacity
mediante gconf-editor o a compiz mediante su configurador, que cuando pulsemos
tal tecla nos lance un comando como este:

```
gnome-screensaver-command --lock
```

Además, si quieres apagar el monitor, tendrías que ejecutar el comando:

```
xset dpms force standby
```

Con xset puedes jugar con muchas cosas. Puedes usar los leds del teclado, esos
de bloqueo numérico, mayúsculas o scroll lock para mensajes de información. En
mi caso, suelo usar cuando puedo el scroll lock para notificarme de correo
usando el modo parpadeante.
