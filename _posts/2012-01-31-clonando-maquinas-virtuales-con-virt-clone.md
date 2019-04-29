---
id: 189
title: Clonando máquinas virtuales con virt-clone
date: 2012-01-31T19:47:20+00:00
author: ant30
layout: post
guid: http://www.ant30.es/?p=189
permalink: /2012/01/clonando-maquinas-virtuales-con-virt-clone/
categories:
  - Sysadmin
  - Virtualización
tags:
  - kvm
  - libvirt
  - qcow2
  - qemu
  - virt-manager
---
En el post anterior, <a href="http://www.ant30.es/2011/12/creando-una-imagen-plantilla-de-debian-y-derivados-para-kvm/" title="Creando una imagen plantilla de Debian y derivados para kvm" target="_blank">Creando una imagen plantilla de Debian y derivados para kvm</a>, ya vimos lo fácil que era crear imágenes de sistemas virtualizados a través de imágenes skels o plantillas y concluíamos creando la máquina mediante virt-install. También comenté que era posible realizarlo con virt-clone.

Virt-clone aún no soporta el tratamiento de snapshots o backing image directamente. De momento, lo que sí que podemos hacer si ya tenemos la imagen creada y el dominio virtualizado disponible en libvirt es invocar al clonado, indicando que queremos preservar los datos de la imagen destino. En nuestro caso, la imagen destino la indicaremos nosotros y se correspondeerá con la imagen del sistema nuevo.

Como ventajas tenemos que copiaremos los datos de ram, cpu, tarjetas de red, contexto selinux/apparmor y el resto de configuraciones del dominio xml asociado a la máquina virtual. Por tanto, resulta interesante principalmente para máquinas con muchos añadidos de configuración.

<!--more-->

Recordamos el proceso de clonación de usando plantillas:

  * Dirigiéndonos a nuestro almacen de imágenes. Congelamos la imagen plantilla quitando permisos de escritura.

    ```bash
    chmod -r ubuntu1110-master.img
    ```

  * Indicamos que queremos crear una nueva imagen de disco basándonos en la plantilla

    ```bash
    qemu-img create -f qcow2 -b ubuntu1110-master.img ubuntu1110-clon.img
    ```

  * Usamos virt-clone para crear la nueva réplica. Con características similares a la máquina original, en este caso, cambian la mac de la máquina y el UUID del dominio.

    ```bash
    virt-clone -n ubuntu1110-clon -o ubuntu1110 -f ubuntu1110-clon.img --preserve-data
    ```

En algunos casos puede ser que tengamos que forzar a que se conecte con el libvirt local, en este caso, lo ejecutamos de esta forma:

```bash
virt-clone --connect qemu:///system -n ubuntu1110-clon -o ubuntu1110 -f ubuntu1110-clon.img --preserve-data
```

En nuestro caso, como queremos lanzar un clon de forma rápida, ha sido necesario el comando `--preserve-data`. Sin embargo, si queremos que la imagen sea independiente, se puede hacer sin la opción `--preserve-data`. La operación durará varios minutos porque revisará todos los mapas de bloques vacíos para resumir el espacio de la nueva imagen de disco.

```bash
virt-clone --connect qemu:///system -o ubuntu1110 -n ubuntu1110-clon2 -f ubuntu1110-clon2.img
```
El comando nos muestra el progreso de la ejecución en la salida incluyendo un tiempo estimado de finalización y el porcentaje.

Virt-clone pertenece al grupo de utilidades de gestión de virtualización <a title="virt-manager" target="_blank" href="http://virt-manager.org/">virt-manager</a> que funcionan haciendo uso de la api <a title="libvirt" target="_blank"  href="http://libvirt.org">libvirt</a>
