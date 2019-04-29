---
id: 70
title: Hay que seguir de cerca al proyecto ownCloud
date: 2011-10-11T17:15:42+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=70
permalink: /2011/10/hay-que-seguir-de-cerca-al-proyecto-owncloud/
categories:
  - blog
tags:
  - software
---
Monta un [owncloud](http://owncloud.org/>owncloud) y tendrás un sistema web para compartir ficheros. Bueno, eso era antes, ya se ha publicado la versión 2.0 y ahora es algo más que compartir ficheros. Tienes la posibilidad de escuchar tu música almacenada en el servidor, tener un listado de marcadores web o bookmarks, un calendario y una agenda. Lo tienes en tu servidor y funciona incluso en sharing hostings baratos tipo DreamHost. Con licencia AGPL
![](/wp-content/uploads/vista-navegacion.png)

Lo que me gusta es su simpleza, si tienes disponible un servidor web con php5, conseguir que funcione es muy fácil. Descomprimir y acceder a la url correcta. Al acceder la primera vez nos pide el nombre del usuario administrador y crea la base de datos en sqlite. Y ya queda listo para usar.

Soporta webdav para poder exportar los contenidos. De esta forma, si el servidor donde lo tenemos desplegando lo permite, podremos disponer de acceso desde un navegador de ficheros, ya sea usando davfs2 para montarlo o accediendo desde gnome o kde. No he conseguido que me funcione correctamente el uso de webdav y nginx, pero es que realmente, el soporte de webdav en nginx está aún muy verde.

En esta base de datos se controlan los usuarios y las quotas de disco de cada uno. También se pueden tener grupos y compartir ficheros entre usuarios, entre grupos o publicarlos mediante un enlace con un hash.
![](/wp-content/uploads/perfil-usuario.png)

Una vez hemos subido algunos ficheros, nos podemos centrar en la parte del almacenado de ficheros. Se crea un directorio por usuario creado y dentro de este otro directorio files que es donde residen los ficheros subidos. La ventaja es que si subes un fichero sin pasar por la web, este sí que se verá en la web. De este modo, sería fácil incorporarlo a un sistema con homes UNIX desplegados mapeando los usuarios correctamente. Se le puede indicar donde reside la raíz de los datos.

Se pueden tomar usuarios desde un ldap, y también ofrece la posibilidad de servir de fuente de autenticación OpenID, aunque no he visto como hacer el camino inverso, es decir, autenticare en ownCloud mediante un id OpenID externo.

En la versión de desarrollo tienen incorporadas una app de galería de imágenes y otra para editar ficheros de texto plano en el servidor. Por otro lado, Rekonq, el navegador de KDE4, permite sincronizar los bookmarks con ownCloud. Sería interesante que se pudieran sincronizar con Firefox y Chrome.

La creación de aplicaciones no parece muy complicada y además tienen ya planteado un repositorio de aplicaciones en la web del proyecto.

¿Qué cosas faltan para que pueda ser un reemplazo de dropbox y similares? Lógicamente, aún queda mucho trabajo por hacer, entre otras cosas, aplicaciones locales que faciliten la integración con el escritorio, aún así, accediendo a los contenidos mediante webdav/https, nos quitaríamos el problema de muchas redes corporativas donde resulta complicado abrir nuevos puertos y en otros entornos menos hostiles, se podría incorporar acceso ftp/smb/sshfs a las carpetas de usuario, dando así más posibilidades de integración sin llegar a tener aplicaciones.
