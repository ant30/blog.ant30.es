---
id: 228
title: Mi primera aplicación en Google Play, eldiario.es para android
date: 2013-02-24T09:03:58+00:00
author: ant30
layout: post
guid: http://www.ant30.es/?p=228
permalink: /2013/02/mi-primera-aplicacion-en-google-play-eldiario-es-para-android/
categories:
  - android
  - aplicaciones
  - eldiario.es
---
[<img class="alignleft size-full wp-image-232" alt="eldiario.es enlace" src="http://www.ant30.es/wp-content/uploads/2013/02/brand-128.png" width="128" height="128" />](https://play.google.com/store/apps/details?id=com.thegeekbunch.eldiario)

Es una aplicación para leer los contenidos de eldiario.es en tu móvil o tablet.

Se trata básicamente de un lector de RSS, pero en vez de leer la fuente RSS directamente del medio en cuestión, pasa a través de un backend que es capaz de ir almacenando las noticias de cada sección y redimensionar las imágenes para que estén optimizadas en tamaño para móviles y tabletas. Dependiendo del tamaño de la pantalla, ofrece vista a una o dos columnas, así como imágenes de diferentes tamaños.

El backend está compuesto por <a title="Pyramid" href="http://www.pylonsproject.org/" target="_blank">Pyramid</a>, [MongoDB](http://www.mongodb.org/) e [ImageMagick](http://www.imagemagick.org).

En el cliente móvil tenemos [phonegap](http://phonegap.com/), [jQuery](http://jquery.com/), [backbonejs](http://backbonejs.org/), [underscore](http://underscorejs.org/), [iScroll](http://cubiq.org/iscroll-4), &#8230; Toda una fauna de librerías típicas que acompañan a una aplicación web.

Desde hace unos meses, andaba jugueteando con conceptos de aplicaciones móviles a modo de entretenimiento con algunos compañeros del trabajo, así que un día de estas navidades, comencé otra prueba de concepto enfocado a realizar una aplicación móvil de un periódico. Entonces seleccioné a <eldiario.es> por publicar sus contenidos con licencia [Creative Commons](http://www.eldiario.es/licencia/) y empecé a jugar con sus fuentes RSS.

Para ir a la página de la aplicación en el market de android puedes pulsar en este enlace:

[Enlace a la aplicación eldiario.es para android](https://play.google.com/store/apps/details?id=com.thegeekbunch.eldiario)