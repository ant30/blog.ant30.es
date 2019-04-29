---
id: 44
title: Mi dormitorio o el centro de mando
date: 2008-07-07T13:16:44+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=44
permalink: /2008/07/mi-dormitorio-o-el-centro-de-mando/
categories:
  - blog
tags:
  - así vivo
  - blog
  - yaco
---
Durante estas semanas, las obras en mi casa han llegado a la sala de máquinas,
así que ahora tengo a todas en mi dormitorio. Eso sí, me quitado de un pentium
200, diversas placas bases, el scaner por puerto paralelo y la impresora que
nunca tiene tinta, ya sustituiré estas dos cosas por un combo baratito con
pictbridge y otras cosillas

Hay dos diferentes ángulos y en ninguno de ellos se ve el lío de cables que hay
detrás de las dos torres del suelo. Pero si podemos ver muchas cosas. Empezamos
por la izquierda de la foto:

<!--more-->

  * Detrás del portátil, tenemos el router WRT54GL (con linux, claro, dd-wrt)
    que se encuentra encima del subwoofer del 4.1 de Creative que tengo como
altavoces.
  * El portátil (**sputnik**), 3 años y medio ya, kilómetros y kilómetros de
    paseos universitarios durante los dos primeros años. Acogedor pruebas y más
pruebas, disco duro de 80GB que ha llegado a estar a 0 bytes disponibles en
casi todas sus particiones. En fin, un sufridor nato que se porta aún
perfectamente. Incluso la batería para la lata y el tiempo que tiene aún dura
hora y media.
  * Eso negro entre el teclado y el portátil es la funda de la cámara.
  * Encima de los altavoces, tenemos la centrar meteorológica. Temperatura
    exterior e interior
  * Si te fijas, detrás del monitor tenemos una caja negra, otra más, en este
    caso el disco duro externo de medio Tera, que ahora mismo tiene 50 gigas
libres, y el 50% son copias de seguridad y diferentes perfiles de
funcionamiento.
  * Monitor de 22&#8243; de Dell, la calidad la justa para su precio. Lo bueno,
    es que tiene entrada DVI y VGA y permite cambiar entre una entrada y otra
pulsando un botón, muy útil cuando tienes tantas torres.
  * Un ratón de 7 botones de NGS, no muy bueno, pero da el pego. Tiene un
    botoncito para cambiar de precisión que es muy útil cuando cambias de una
pantalla de 1680&#215;1050 a otra de 1024&#215;768
  * El primer switch del par que tengo, acompañado del segundo ratón, otros dos
    altavoces. El lapicero de Guadalinex, además de su utilidad, me indica cual
es el altavoz frontal, así puedo coger el otro y tirar del cable cuando veo o
juego a algo con más de 2 canales
  * La torre negra con frontal blanco (**courier**) con pantalla para encender,
    controlar ventiladores temperatura &#8230; La potencia de la casa, 4 cores
a 2.4 nominales, 4 gigas de RAM Corsair, capacidad para otros 4 más. Un disco
de 500, con capacidad para otros 5 más &#8230; tarjeta gráfica nvidia gf9600
serie media de nvidia . Overclokeando en invierno ha aguantado de forma estable
a cerca de 3.200GHz por core y la memoria a 1066, aunque con el rendimiento
nominal de fábrica tengo más que de sobra
  * Llegó la hora a lo que hay debajo de la mesa, dos reliquias. La de la
    izquierda es un P4 a 1.5GHz (**ariane**) con 1giga de ram y un dos discos
duros, 80 y 160, habitualmente suele tener el externo de 500 también y está
todo el día encendido como buen servidor para casa que es Aunque parezca
mentira, viene a consumir entre 55(idle) y 80W(pleno rendimiento), lo cual no
está nada mal para tratarse de un P4. Normalmente hace de firewall y router
gigabit con una de sus 3 interfaces (dos gigabit y una fast ethernet). Otra
curiosidad es que actualmente está ofreciendo todos los servicios que estaban
en aeros desde que se quedó sin pila de BIOS y disco duro secundario de 20GB,
todo dentro un chroot. Actualmente durante el arranque carga un total de 20
servicios, de los que 18 están cada uno en su chroot
  * A su derecha tenemos a un Duron 850MHz (**aeros**), otro que sirve para
    pruebas. Normalmente, cuando funciona, a pesar de que ya solo le quedan 128
megas de RAM funcionando suele hacer de servidor, sobre todo porque gasta un
poco menos, entre 45 y 60.
  * Encima de las torre tenemos otra regleta, otro switch gigabit y el kvm
    manual, de los antiguos.
  * Y para terminar, el monitor, que tiene unos 7 años, casi lo mismo que el
    ariane de 17&#8243; y un teclado conectado al kvm. Normalmente, si está
conectado a ariane y este tiene entorno gráfico levantado, lo uso con synergy,
del que ya hablamos en el blog, en ese caso. tengo las dos salidas conectadas
con lo que suelo darle al botón del monitor para cambiar a ver ariane
directamente

En fin, aparte de los cacharros que hay en los diversos cajones, y la pared con
duelas de color madera, tampoco tengo muchas más cosas raras.

Usando mi medidor de consumo puesto en el enchufe principal, todo encendido,
viene a gastar unos 600-700W de media tirando alto. Con un consumo así da para
pensar el tenerlo todo el día encendido y ver la factura de la luz. Así que
habitualmente, solo está encendido algo cuando es necesario. Puedo incluso
encender los otros ordenadores desde aeros o ariane que son visibles desde
fuera gracias wol, llamadas acpi-pci y otras configuraciones. En el futuro,
espero que se puedan activar desde un estado suspendido con una
simple llamada al servicio, y que el router lo levante. De momento, esto ya
funciona con courier (el quadcore) por que en la placa base, la interfaz wifi y
la ethernet en conjunto hacen de punto de acceso y está configurado para que al
detectar actividad en la mac del ethernet, se levante el solito incluso estando
apagado.

![img_0180.jpg](/wp-content/uploads/img_0180.jpg)

![img_0182.jpg](/wp-content/uploads/img_0182.jpg)
