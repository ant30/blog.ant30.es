---
id: 58
title: 'Plugin jQuery: jquery.flickrutils.js'
date: 2009-03-01T13:54:00+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=58
permalink: /2009/03/plugin-jquery-jquery-flickrutils-js/
categories:
  - blog
tags:
  - api
  - flickerutils
  - jquery
  - json
  - proyectos
---
Hace unos meses comencé a mirar la api pública de flickr, entre otras cosas por
la necesidad de facilitarme el enlace desde este mismo blog a fotos de flickr.

En primer lugar, quería hacer una especie de selector en que el se me
permitiese realizar búsquedas por tags y nombres, quizás por fechas, incluso
fotos de otros usuarios. En un principio, pensando solo en que fuese compatible
con Drupal.

Al final, ayer sábado por la mañana, decidí retomar lo que tenía hecho de aquel
día y además de intentar aprender un poco de jQuery, conseguir algo que sea
fácilmente portable a cualquier sistema web donde se pueda usar jQuery.

Entre alguna de las funciones que ya podemos encontrar son:

  * **Buscador de fotos** permite devolver lo obtenido a una función
    `callback`; donde se pasa una lista de id de fotos. El callback
    que viene por defecto mete estos ids en un input. Aún le queda mucho por
    maquetar y mucho por revisar en la funcionalidad, pero parece funcionar.

  * **Carga de fotos** Ya tenemos una lista de ids, ahora queremos poder
    cargarla en algún div. Para esto teníamos varias opciones. y al final he
    ido implementando casi una por una.
      * Imágenes en un link a la vista pública de la imagen en flickr.
      * Imágenes en un link a la misma imagen pero en otro tamaño.
      * Buscar un formato en todo el documento y reemplazarlo por imágenes

El enlace al proyecto en Google Code:

[jquery-flickr-utils](http://code.google.com/p/jquery-flickr-utils/)

Enlace a una demo con lo que hay a fecha de publicación de este post:

[Demo](http://demos.ant30.es/jquery-flickr-utils/)

Para obtener la información necesaria uso la API rest de la que tenemos
disponible una esplendida documentación a través de
[flickr](http://www.flickr.com/services/api/). Y recalco lo de espléndida
documentación porqué sin ella, no sirve de nada tener una API.

A ver si termino de ponerlo un poco bonito y lo empaqueto como plugin para
drupal.
