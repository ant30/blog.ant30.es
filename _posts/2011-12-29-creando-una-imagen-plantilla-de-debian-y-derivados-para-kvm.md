---
id: 176
title: Creando una imagen plantilla de Debian y derivados para kvm
date: 2011-12-29T21:48:40+00:00
author: ant30
layout: post
guid: http://www.ant30.es/?p=176
permalink: /2011/12/creando-una-imagen-plantilla-de-debian-y-derivados-para-kvm/
categories:
  - blog
  - Sysadmin
  - Virtualización
tags:
  - debian
  - kvm
  - libvirt
  - qcow2
  - qemu
  - Virtualización
---
## ¿Qué es una imagen plantilla?

Imagen Skel, plantilla, Imagen Gold, template, Appliance, base, .. todo estos términos se acaban refiriendo a lo mismo. En los sistemas de virtualización más habituales, tenemos la posibilidad de guardar el disco duro del sistema virtualizado en nuestro propio disco duro en una imagen que habitualmente contiene unos metadatos de información como por ejemplo el tamaño del disco y además contiene lo que sería equivalente al disco duro físico de la máquina virtual. Existen múltiples formatos de imagen de disco como vmdk, vdi, raw, qcow2 o qed. En nuestro caso, vamos a usar qcow2 y porque es el formato más estable para el sistema de virtualización que vamos a usar, libvirt con kvm.

Si solemos trabajar con sistemas virtualizados, es bastante habitual tener múltiples máquinas con una misma versión de debian, aunque luego tengan software diferente. También puede resultar que tengamos todo el sistema y una aplicación desplegada en muchas máquinas, de forma que luego solo hacemos algunas personalizaciones sobre la aplicación. Es decir, que realmente estas máquinas comparten mucha información como sistema operativo y aplicaciones. Intentar evitar la repetición de información compartida es el objetivo de las imágenes gold o plantillas. Pero no solo por evitar la duplicidad de información, sino para conseguir ahorrar tiempo en la creación de nuevas máquinas virtuales de características similares.

A continuación veremos una forma de reducir drásticamente el tiempo de creación de nuevas máquinas virtuales a través de imágenes plantilla.
<!--more-->

## Un ejemplo

Imagina que te necesitas 10 máquinas virtuales con debian squeeze, y por tanto, tuvieras que instalar debian 10 veces. Qué pérdida de tiempo ¿no?

Una posible solución, y suele ser la primera solución que hemos hecho alguna vez todos, es tras acabar de instalar debian en esa imagen, copiarla 9 veces. Con un disco duro medianamente rápido conseguimos una tasa de copia en un mismo disco de 40MB/s, te puedes ir a tomar un café mientras se termina la copia y quizás cuando vuelvas aún no haya terminado.

Tras arrancar cada una de las nuevas 9 máquinas es posible que nos encontremos sin red, porque se nos habrá creado una nueva interfaz de red que no está contemplada en la configuración (/etc/network/interfaces) para que se active durante el arranque. Esto ocurre porque la MAC de la tarjeta de red de la nueva máquina virtual es diferente a la que se generó primero. Para solucionar esto, basta con borrar el fichero /etc/udev/rules.d/70-persistent-net.rules y reiniciar.

Bien, ya tenemos las 10 máquinas arrancadas y con red, si hacemos un ssh y nos fijamos tenemos un nuevo problema. Todos los servidores tienen la misma key. Por tanto toca regenerarlas para cada servidor y reiniciar servicio ssh.

También puede resultar problemático que todas las máquinas tengan el mismo hostname y por tanto, al hacer ssh, todas tengan el mismo prompt, lo cual puede resultar confuso.

Por tanto, seguimos teniendo varios problemas. Larga espera para la copia de las imágenes y modificaciones manuales máquina a máquina. Necesitamos algo más práctico.

## Mejora 1: Reduciendo modificaciones manuales del primer arranque

Una vez tenemos nuestra imagen preparada, recién instalada, antes de empezar a usarla como imagen plantilla, deberíamos intentar simplificar al máximo las tareas manuales a realizar durante el primer arranque. Además de limpiar de datos que no sean útiles para los futuros clones.

### Eliminar persistencia de nombres de dispositivos de red de udev

Si estamos usando una imagen con una distribución debian o basada en debian como ubuntu, tendremos el problema de udev, así que lo ideal es siempre borrar el fichero /etc/udev/rules.d/70-persistent-net.rules para que cuando se arranque una copia de la máquina, se cree un nuevo fichero de nuevo. En VMware y XEN este problema no ocurre, pero sí con KVM. El problema radica en que el identificador del dispositivo en KVM es como el de un dispositivo real, ya sea Realtek o Intel, y con esto, udev, no puede distinguir cuando se está usando una interfaz virtual o no.

Actualmente, las reglas para inhibir la creación del fichero de persistencia para redes de KVM están en el fichero /lib/udev/rules.d/75-persistent-net-generator.rules pero también se puede apreciar como existen reglas para Realtek que entran en colisión. Por tanto, también puedes anular la regla de Realtek para se haga efectiva, pero entonces estarás modificando ficheros del paquete udev, práctica nada recomendable para futuras actualizaciones.

### Eliminar historial

Dependiendo de nuestras necesidades o de quien pueda usar después la imagen que estamos preparando, lo ideal es borrar rastros como por ejemplo el fichero .bash_history de todos los usuarios.

Hay quien incluso limpiaría logs y otros restos.

### Preparar tareas para el primer arranque

No existe o no conozco una forma elegante de programar una tarea que se ejecute solo en el próximo arranque. Así que lo que vamos a preparar es una forma poco intrusiva para llevarlo a cabo.

Creamos en /root un script llamado firstboot.sh con el siguiente contenido:

```
#!/bin/bash
rm /etc/ssh/ssh\_host\_*
/usr/sbin/dpkg-reconfigure openssh-server
```


En nuestro caso, solo vamos a regenerar las claves del servicio ssh.

Ahora, creamos un nuevo fichero en cron.d para que invoque a nuestro script al completar el arranque, por tanto creamos el fichero /etc/cron.d/firstboot con el siguiente contenido:

```
@reboot root bash /root/firstboot.sh && rm -f /etc/cron.d/firstboot /root/firstboot.sh
```

Primero se ejecutará firstboot.sh y luego se borrará la entrada del cron y el script del primer arranque para que no se vuelvan a ejecutar.

Ya avisé que no era una forma muy elegante de hacerlo.

El script firstboot.sh descrito es lo mínimo, pero sería interesante recoger el hostname desde el DNS y aplicar el cambio, realizar las tareas para inicializar un sistema de gestión de configuración centralizada o cualquier otra tarea.

### Prepara tu entorno

Una vez finalizada la instalación de debian, tenemos vi, pero no vim, basta con instalar el paquete, pero aprovechando que estamos haciendo nuestra template, deberíamos instalar esas cosas que siempre acabamos usando, y aquí va una lista de las cosas que suelo tener en mis imágenes plantillas independientemente de para qué se usen:

  * El metapaquete vim-nox, o vim
  * Mi configuración de vim, .vimrc
  * Mis claves públicas ssh en /root/.ssh/authorized_keys para no tener que estar metiendo contraseñas en cada ssh
  * Un .bashrc con los alias que suelo usar
  * Screen y su configuración
  * ¿Está instalado ssh server y aptitude? ¿Necesito sudo?
  * htop, un top mucho más cómodo
  * Algunos alias en el fichero /etc/hosts

## Mejora 2: técnica alternativa al copiado de imágenes, backing images

Estamos usando libvirt con kvm, que en definitiva se trata de qemu y por tanto, para la creación de imágenes, usamos qemu-img. Para crear una imagen nueva basada en nuestra plantilla es tan fácil como ir al directorio de imágenes y suponiendo que la imagen creada se llame debian6.img creamos un clon, debian6-clon.img, con el comando qemu-img como sigue:

```
qemu-img create -f qcow2 -b debian6.img debian6-clon.img
```

Tendremos la nueva imagen en segundos y el nuevo fichero debian6-clon.img ocupará solo unos kbytes. Esta nueva imagen irá almacenando bloques que se creen que no estén o hayan sido modificados respecto a la imagen base.

Como inconveniente, tenemos una bajada en el rendimiento de IO por los múltiples accesos a disco por cada petición. Este problema se puede solventar en gran medida si las plantillas las alojamos en discos SSD. Aún así, sino vamos a realizar pruebas de rendimiento, no debería suponer mucho problema.

Si vas a usar la propia imagen como backing image para el resto, recuerda quitar los permisos de escritura para que nunca se vuelva a arrancar, y si puede ser posible, deberías desvincularla de cualquier dominio libvirt. En caso de arranque, el resto de imágenes clonadas que se basen en tu backing image, perderán consistencia y dejarán de funcionar. Casi mejor, por tanto, tener cuidado con la imagen plantilla.

Por otro lado, si queremos convertir la imagen de este clon en imagen plantilla, deberíamos aplanarla, es decir, separarla de la imagen debian6.img fusionando sus datos en una nueva imagen que sea independiente. Esta nueva imagen independiente la podremos mover a otra ruta o incluso a otro servidor. Esto se consigue mediante el comando:

```
qemu-img convert -f qcow2 -O qcow2 debian6-clone.img debian6-plana.img
```

Que además, evitará poner en la nueva imagen bloques vacíos y por tanto puede ocupar menos de lo que uno espera.

## Creando una máquina virtual con virt-install usando la plantilla

Entre las utilidades que solemos encontrar con libvirt, tenemos virt-install, que nos permite crear una máquina virtual desde bash, también podríamos crearla desde virt-manager de forma gráfica, pero en nuestro caso la vamos a crear mediante virt-install.

Suponiendo que tengamos la imagein debian6-clon.img creada en el punto anterior, en el directorio /var/lib/libvirt/images/ y teniendo en cuenta que este es el almacenamiento por defecto para las imágenes, para crear un sistema con 512MB de RAM y especificando que está basado en Debian Squeeze (debian 6), ejecutaríamos lo siguiente:

```
sudo virt-install --name debian-clone --ram 512 --os-variant=debiansqueeze --disk /var/lib/libvirt/images/debian6-clone.img --import
```

Se nos abrirá un visor vnc con virt-viewer para que podamos acceder, por ejemplo para ver si el script de primer arranque se ha ejecutado correctamente, o para conocer la IP de la máquina y poder acceder por ssh de un modo más cómodo.

Por tanto las nuevas réplicas, tanto por la clonación del disco, como del esquema XML de la máquina en libvirt se obtienen en segundos y las tendremos disponibles en nuestro Virt-Manager.

También podríamos usar virt-clone para la generación del XML, pero eso ya lo dejamos para otro día.
