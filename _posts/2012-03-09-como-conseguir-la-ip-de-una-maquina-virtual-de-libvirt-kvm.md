---
id: 209
title: Cómo conseguir la IP de una máquina virtual de libvirt / kvm
date: 2012-03-09T20:58:05+00:00
author: ant30
layout: post
guid: http://www.ant30.es/?p=209
permalink: /2012/03/como-conseguir-la-ip-de-una-maquina-virtual-de-libvirt-kvm/
categories:
  - Sysadmin
  - Virtualización
tags:
  - kvm
  - libvirt
  - redes
  - virt-manager
---
Una de esas pequeñas cosas que molestan de libvirt en su combinación por virsh es que no tiene un método para obtener la IP de una máquina virtual. Y sin la IP ¿cómo conectamos por ssh a la máquina?

En los últimos cambios incorporados en libvirt, se permite controlar parte del gestor dhcp/dns que viene incorporado, dnsmasq y que seguro que en un futuro nos permitirá obtener la IP sin hacer trucos. Se puede ver en la documentación de [Addressing de libvirt](http://libvirt.org/formatnetwork.html#elementsAddress "Addressing de libvirt")

La forma trivial de obtener la IP es bastante fácil. A través de virt-manager, accedes a la pantalla de la máquina mediante VNC. Haces login con tu usuario y contraseña, y bastaría con ejecutar _ifconfig_ o _ip a_ para ver la IP entre toda la información de la red.

Este método es un poco tedioso, y además, si el servidor se encuentra en una red lejana con mucho _lag_ en la terminal, el efecto sobre la consola VNC puede ser temible. Por tanto, hay que buscar alternativas más cómodas. Cómo mediante la información arp.

<!--more-->


Hasta ahora, mi método solía ser mirar la mac de máquina obtenida del XML de la máquina con:

```bash
$ virsh dumpxml nombrevm
```

Y luego buscarla en la tabla arp, fácilmente accesible mediante:

```bash
$ arp -n
```

Para refrescar la memoria de algún rezagado, con el comando arp se obtiene la información de la tabla de caché del sistema que vincula "macs" vecinas con sus respectivas IPs, de forma que se evite tener que estar continuamente haciendo peticiones de descubrimiento de asociaciones mac-ip en la red en cada envío de paquete tcp/ip. Si quieres más información sobre el protocolo ARP siempre puedes consultar su <a href="http://es.wikipedia.org/wiki/Address_Resolution_Protocol" title="ARP en Wikipedia" target="_blank">página en la Wikipedia</a>. Como inconveniente tenemos que solo es válida su ejecución en el host de virtualización donde esté corriendo la máquina virtual. De otra forma, al no llegarnos la petición de IP de DHCP mediante broadcast, no podremos obtener esta IP.

Este método es muy manual, pero es bastante fácil meterlo todo en un comando de una sola línea combinando esos comandos que tanto nos gusta usar en la shell como cut y grep:

```bash
env NOMBREVM=nombrevm arp -n | grep $(virsh dumpxml $NOMBREVM | grep mac\ address | cut -d \' -f 2 ) | cut -d \  -f 1
```

Y si ya tenemos todo en una línea ¿porqué no incorporarlo como comando del sistema?

Como adjunto, he incorporado el script que uso para obtener la IP de una máquina virtual indicándole el nombre de la máquina. El comando lo llamo virt-ip y lo suelo colocar bajo la ruta _/usr/local/bin/virt-ip_

[virt-ip](http://www.ant30.es/wp-content/uploads/2012/03/virt-ip.gz)
