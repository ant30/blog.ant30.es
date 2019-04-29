---
id: 162
title: Este blog ya está migrado de Drupal 6 a WordPress 3
date: 2011-12-28T14:43:27+00:00
author: ant30
layout: post
guid: http://www.ant30.es/?p=162
permalink: /2011/12/este-blog-ya-esta-migrado-de-drupal-6-a-wordpress-3/
categories:
  - blog
tags:
  - Drupal
  - web
  - Wordpress
---
Tras 3 años con Drupal, ahora he migrado a WordPress. Teniendo en cuenta que solo se trata de un blog, necesito algo que sea más fácil de gestionar, actualizar, utilizar y creo que en eso WordPress gana de sobra a Drupal. Me encantan las Views y los CCK de Drupal, pero es que realmente, para este tipo de blog no los estaba usando.

Existen unas cuantas formas de migrar de Drupal a WordPress, pero no todas contemplan todas las opciones. Lógicamente, migrar funcionalidades de los módulos principales es una tarea que acaba siendo manual.

En mi caso, tenía algunos requisitos mínimos sin los cuales, no podría considerar hacer la migración:

  * Conservar todos los nodos de Drupal. En Drupal, las páginas, libros y entradas de blogs son nodos.
  * Conservar las taxonomías, pero no en categorías, sino transformándolas a tags.
  * Resaltado de sintáxis de código. Tengo varios posts con bloques de código, así que es imprescindible que sigan apareciendo con resaltado de sintaxis.
  * Migración de adjuntos, y en el caso de las imágenes, que por lo menos, sean visibles. No siempre están como html en el contenido, porque mediante el módulo img_filter, se visualizaban de forma automática en los posts.
  * Conservar los comentarios, aunque cambien un poco el formato. En drupal hay asunto del comentario y en WordPress no.
  * Solo hay un usuario, así que esto no debería suponer un problema.
  * Debe haber pocos 404 tras la migración.

Esta no iba a ser mi primera migración entre diferentes softwares de servicios web, he realizado diferentes migraciones tanto a nivel profesional, en [Yaco Sistemas](http://www.yaco.es), como para diferentes proyectos personales u otros fines sin ánimos de lucro. En este caso, quiero un resultado no necesariamente profesional, pero sí que esos requisitos mínimos especificados arriba queden satisfechos. Así que como indica el sentido común para realizar migraciones en un sistema de producción, lo ideal es prepararse un entorno de desarrollo con una copia de los datos de producción. En este caso, preparé un chroot con un sistema operativo a donde está hosteado el blog y luego desplegué un clon de Drupal con un dump de la base de datos, y una base de WordPress para comenzar a realizar pruebas.

<!--more-->

Las primeras pruebas que hice, eran pruebas de migración realizando modificaciones sobre el SQL siguiendo algunas guías que existen:

  * [Post de Lincoln Hawks](http://socialcmsbuzz.com/convert-import-a-drupal-6-based-website-to-wordpress-v27-20052009/) en Socialcmsbuzz.com
  * [Migration from Drupal 6.x to WordPress 2.9x](http://scurker.com/blog/2010/02/migration-from-drupal-6-x-to-wordpress-2-9x/)

El problema con estos procesos de migración, son los adjuntos. En estos posts, se propone reemplazar las rutas de los tags img en el body, pero es que en mi caso hay tags para img_filters de tipo [img:01 yy:left] o sin ni siquiera tag, indicando en el adjunto como se debe publicar.

Al final, entre diversas pruebas, he optado por pasar por un formato intermedio, [MovableTypes](http://es.wikipedia.org/wiki/MovableType) Viendo [este post](http://andrew.sterling.hanenkamp.com/2008/03/drupal-to-movable-type.html) de Andrew Sterling parece bastante fácil manipular el body, los tags y el resto de información creando un nodo con contenido php en Drupal.

finalmente, mi código de exportación de Drupal, basándome en el publicado por Andrew Sterling, ha quedado como en el siguiente bloque. No es que sea bonito, pero es funcional:

```
header('Content-type: text/plain');
$nids = db_query("select distinct n.nid from {node} n inner join {term_node} t on t.nid = n.nid");
while ($node = node_load(db_fetch_object($nids)->nid, NULL, TRUE)) {
  print "AUTHOR: ".$node->name."\n";
  print "TITLE: ".$node->title."\n";
  print "STATUS: ".($node->status?"Publish":"Draft")."\n";
  print "ALLOW COMMENTS: ".($node->comment>0)."\n";
  print "CONVERT BREAKS: 1\n";
  print "ALLOW PINGS: ".($node->comment>0)."\n";
  print "DATE: ".strftime("%m/%d/%Y %I:%M:%S %p", $node->created)."\n";
  print "CATEGORY: blog \n";
  print "KEYWORDS: \n";
  foreach ($node->taxonomy as $vid => $term) {
    print $term->name.",";
  }
  foreach ($node->category as $category) {
    print $category->title.",";
  }
  print "\n";
  print "-----\n";
  print "BODY:\n";

  $body = str_replace("<python>", "[py]", $node->body);
  $body = str_replace("</python>", "[/py]", $body);

  $body = str_replace("<bash>", "[sh]", $body);
  $body = str_replace("</bash>", "[/sh]", $body);

  $body = str_replace("<code>", "<pre class="crayon-plain-tag">", $body);
  $body = str_replace("</code>", "</pre>", $body);

  $body = str_replace("/files/", "/wp-content/uploads/", $body);

  $media_base = "/wp-content/uploads/";

  $patterns = array("/\[img:01 align:left\]/s",
                    "/\[img:vista-navegacion.png yy:float_left\]/s",
                    "/<\!-- break -->/s",
                    "/<\!--break-->/s",
                    "/<h3>/s",
                    "/<\/h3>/s"
                     );

  $replace = array('<img src="'.$media_base.'01_antes_de_comenzar.jpg"/>',
                   '<img src="'.$media_base.'vista-navegacion.png"/>',
                   "<!--more-->",
                   "<!--more-->",
                    "<h2>",
                    "</h2>"
                   );

  $body = preg_replace($patterns, $replace, $body);

  print $body."\n";

  $uploads = db_query("select * from {upload} u where u.nid= %d and list = 1", $node->nid);
  while ($upload = db_fetch_object($uploads)) {
      $file_query = db_query("select * from {files} f where f.fid = %d", $upload->fid);
      $file = db_fetch_object($file_query);
      $file_url = str_replace("files/", "/wp-content/uploads/", $file->filepath);
      $pos = stripos($body, $file_url);
      if ($pos === false){
        $pos_mime = stripos($file->filemime, "image/");
        if ($pos_mime === false){
            print '<a href="'.$file_url.'">'.$file->name.'</a>';
        }
        else {
            print '<img alt="'.$file->filename.'" src="'.$file_url.'"/>';
        }
        print "\n";
      }
  }

  print "-----\n";
  $comments = db_query("select * from {comments} c where c.nid = %d", $node->nid);
  while ($comment = db_fetch_object($comments)) {
    print "-----\n";
    print "COMMENT:\n";
    print "AUTHOR: ".$comment->name."\n";
    print "EMAIL: ".$comment->mail."\n";
    print "IP: ".$comment->hostname."\n";
    print "URL: ".$comment->homepage."\n";
    print "DATE: ".strftime("%m/%d/%Y %I:%M:%S %p", $comment->timestamp)."\n";
    print $comment->subject."\n";
    print $comment->comment."\n";
  }
  print "--------\n";
}
exit;
?>
```
Con este código, las imágenes adjuntas que no estaban insertadas con tags img ya sea de image_filter o html img, se añaden al final. En el caso de que el adjunto no sea una imagen, se incorpora un enlace. Es importante mover las imágenes del directorio drupal files, al de wordpress, /wp-content/uploads/

En los comentarios, el asunto de muestra en la primera línea, y luego se incorpora el body. También hay ciertas personalizaciones como la conversión del _break_ de drupal al _more_ de wordpress y otras conversiones _caseras_.

El único problema a resolver ahora está en las URL. En el script de Andrew, se incorporaban como categorías, de forma, que luego mediante un redirect a una búsqueda de wordpress, fuera fácil localizar el post. Sin embargo, esto acaba creando muchas categorías que no son útiles, y aunque se puedan usar módulos para ocultarlas, a la hora de publicar contenidos, estas categorías siguen apareciendo. En mi caso tengo 70 posts, así que si tenemos 2 tipos de urls, el permalink y la pretty url, tendríamos 140 categorías inválidas, demasiadas.

Como opción, finalmente, he escrito un pequeño script en php que haga un redirect a la url de búsqueda de wordpress si se detecta en apache una url con el formato de drupal. Este script sí que no lo publico, son 5 líneas que dan vergüenza.

También he tenido en cuenta en la migración la antigua url del feed de Drupal, que se cambia al nuevo formato de WordPress mediante un Redirect en Apache.

He cambiado el color de la cabecera a blanco y negro, en escala de grises, añadido los botones de twitter y google+ y Akismet y poco más. Esta es la historia de una migración de Drupal a WordPress.
