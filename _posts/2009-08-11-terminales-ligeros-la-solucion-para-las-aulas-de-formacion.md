---
id: 60
title: Terminales ligeros, la solución para las aulas de formación
date: 2009-08-11T14:13:23+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=60
permalink: /2009/08/terminales-ligeros-la-solucion-para-las-aulas-de-formacion/
categories:
  - blog
tags:
  - Linux
  - software
  - tcos
  - terminales ligeros
  - thinclients
---
Tras llevar bastante tiempo ligado a los servicios web, tanto en el desarrollo
como en el despliegue y mantenimiento de esta clase de sistemas, ahora tengo la
oportunidad de trabajar en un proyecto mucho más relacionado con Sistemas
directamente y el comúnmente llamado _cacharreo_.

Se trata de incorporar a una serie de aulas de formación, un sistema basado en
clientes ligeros y servidor, usando para ello [Tcos](http://www.tcosproject.org/).

El hecho de incorporar este tipo de arquitectura a las aulas es beneficioso por varios motivos:

  * Fácil administración de todos los equipos del aula. Ahora todos los puestos
    usarán el mismo sistema, que será servidor y procesado por el servidor
  * Ahorro en máquinas. Los clientes ligeros tienen un tiempo de vida muy
    superior al de un equipo normal. En un aula con equipos normales, hay que
    renovar equipos cada 3 o 5 años, porque se han quedado obsoletos. En un aula
    con clientes ligeros. Pasado este tiempo, solo habría que cambiar el servidor
    si se necesita más rendimiento.
  * Consumo energético. En el caso de usar clientes ligeros, tenemos terminales
    con consumos desde 5w, y máximo de 20w. El PC de un puesto normal, con
    estar encendido ya viene a consumir 60-80w como mínimo.
  * Reutilización de equipos viejos. Se pueden usar equipos antiguos con poca
    ram (a partir de 64MB, o un incluso menos), sin disco duro, ni disquettes.
    Solo necesitan una tarjeta de red que soporte arranque con PXE o una disquetera
    para emular el arranque PXE con etherboot
  * Además, si decidimos usar Linux y la alternativa fuese windows, nos
    ahorramos el precio de licencia por puesto.

Dejo pendiente para otros días analizar alguno de los principales sistemas que tenemos para ofrecer este tipo de servicio, como son LTSP, Tcos, PXEs
