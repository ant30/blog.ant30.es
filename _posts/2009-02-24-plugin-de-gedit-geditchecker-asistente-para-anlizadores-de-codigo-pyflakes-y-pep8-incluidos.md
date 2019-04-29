---
id: 57
title: 'Plugin de Gedit: GeditChecker, Asistente para anlizadores de código, pyflakes y pep8 incluidos.'
date: 2009-02-24T13:05:12+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=57
permalink: /2009/02/plugin-de-gedit-geditchecker-asistente-para-anlizadores-de-codigo-pyflakes-y-pep8-incluidos/
categories:
  - blog
tags:
  - gedit
  - geditchecker
  - proyectos
  - python
---
Si el último post fue sobre un plugin de gedit, aquí va otro plugin. Un
ayudante para que nuestro gedit pueda comprobar nuestro código, cuando salvamos
el fichero. Lo que hay realizado, está hecho durante el tiempo sobrante después
del almuerzo, y con esto, lo que quiero remarcar es la velocidad de desarrollo
que aporta python y tener una api de plugins en este lenguaje como la que tiene
Gedit, mi editor habitual cuando ando por escritorios.

En este caso se trata de un plugin para revisar la sintaxis del fichero que
estamos editando al salvar. Para ello, en el caso de python, se ayuda para
utilidades externas como pyflakes y pep8.py . Vuelva la salida de estos en un
panel inferior, pero además, la funcionalidad extra que añade es que analiza
las líneas de salida de los analizadores externos para que al clickearlas nos
sitúe en la línea que se ha producido el error o warning.

Un compañero de trabajo también está realizando una utilidad para comprobar
código css, haciendo uso de w3c, y también está incorporado. De esta forma, ha
surgido la idea de hacer esto configurable mediante un panel de configuración
en el que poder añadir nuevas herramientas externas, como las de
gedit, o usar esas mismas, y poder asociarlas a extensiones de ficheros para
que se asocien al evento de salvado de documentos. También estaría bien poder
asignar parámetros extras a estos otros analizadores.

El enlace al proyecto en google code:

[GeditChecker](http://code.google.com/p/geditchecker/)

El código del panel GeditChecker se publica bajo GPL, como se indica en el
directorio. Las utilidades de testeo, tienen cada una su respectiva licencia.

De momento solo se ha probado en Intrepid Ibex que usa Gnome 2.24

Esto me ha recordado algo que se suele decir mucho en Software Libre, y era
algo así como que si _hay algo que falta en un programa, por qué no lo haces_
