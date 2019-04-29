---
id: 56
title: Plugin de Gedit, panel de ficheros abiertos con extras
date: 2009-01-13T12:52:02+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=56
permalink: /2009/01/plugin-de-gedit-panel-de-ficheros-abiertos-con-extras/
categories:
  - blog
tags:
  - django
  - gedit
  - gedit-plugins
  - gnome
  - gtk
  - proyectos
  - python
---
Últimamente en el trabajo suelo trabajar bastante con django, un framework de
desarrollo web en python. Este tiene la particularidad de tener todas las
aplicaciones, o subcomponentes con una jerarquía de ficheros muy definidas. De
esta forma, si tienes muchas aplicaciones pequeñas, y estás trabajando en
varias a la vez, es bastante común que mostrar un nombre de fichero no sea
suficiente como para distinguirlo del resto. Suele pasar con los ficheros
views.py, models.py, urls.py, forms.py. Si estás tocando 3 ficheros
views.py no eres capaz de distinguirlos y acabas probando a cambiar a alguna de
las tres pestañas; hasta que consigues abrir el que quieres.

Para solucionar esto, se me ocurrió la idea crear un panel lateral de gedit,
usando el código de otros plugins python para este editor, de forma que te
mostrase, además del nombre del fichero, en caso de que exista más de dos en el
listado, que se muestren también el nombre del directorio padre, y sino, el
abuelo, así hasta encontrar diferencias.

No vale solo con directorio padre porque también puede ser que tengas abierto
el mismo fichero en dos versiones de un proyecto diferentes y entonces,
estaríamos en las mismas, seguiríamos sin distinguir a simple vista que fichero
queremos seleccionar.

He subido el código a un proyecto creado en google code,
[listopenfilespanel-gedit](http://code.google.com/p/listopenfilespanel-gedit).
Con licencia GPLv3.

En un principio la funcionalidad es bastante escueta además de permitir cambiar
a una u otra pestaña cuando se pulsa en un item del listado. También se
mantiene en el mismo orden en el que se encuentran las pestañas, y la pestaña
activa se encuentra remarcada entre corchetes.

Entre algunas mejoras, me gustaría incorporar algunos botones con funciones
específicas como ordenar los ficheros abiertos por path, un campo de texto para
filtrar ficheros, en vez de mostrarlos todos- ¿Se te ocurre alguna
sugerencia?
