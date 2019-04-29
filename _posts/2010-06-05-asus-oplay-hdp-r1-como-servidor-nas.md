---
id: 68
title: Asus o!play hdp-r1 como servidor NAS
date: 2010-06-05T10:22:34+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=68
permalink: /2010/06/asus-oplay-hdp-r1-como-servidor-nas/
categories:
  - blog
tags:
  - Linux
  - o!play
---
Alguien de asus comentó en los [foros de soporte de asus](http://vip.asus.com/forum/topic.aspx?board_id=19&model=O!Play%20HDP-R1&SLanguage=en-us) que en la siguiente actualización de firmware, la ahora existente 1.27, tendría capacidades de NAS.

Tras publicar un firmware de esta versión, la 1.27 en su ftp de betas, y quitarlo a las horas debido a problemas en los subtítulos, volvieron a poner una nueva 1.27. Sí, yo opino también que deberían llamarse diferente y no solo indicarlo en el número de build o en el fichero interno de versión de SVN.

Pues bien, el caso es que algunos participantes del foro que he mencionado antes, investigaron y descubrieron que mediante la ejecución de un par de comandos existentes en el firmware y al que se puede acceder vía telnet, se activaba el servidor samba compartiendo lo que haya conectado vía USB/eSATA. Pirlas, publicó en su [blog](https://durao.net/?p=1710) los comandos a ejecutar pero no indicó una forma para que se iniciase este sistema durante el arranque.

Este mismo usuario, también ha descubierto, que en esta versión del firmware, viene un [cliente de bittorrent](https://durao.net/?p=1716) con interfaz web, que aún no parece funcionar del todo bien.

Pues bien, para modificar el arranque del sistema no tenemos más que acceder vía telnet y modificar el fichero que podemos encontrar en la ruta /usr/local/etc/rcS. Este path de etc se encuentra montado para lectura y escritura porque en él, se guardan entre otras cosas la información de configuración, los favoritos y la información de pausa; para continuar por donde se dejaron los contenidos al volver a reproducirlos.

Para que arranque el servicio de samba, nos tenemos que asegurar que está levantada la interfaz de red y se encuentran montados las unidades USB. En mi aparato, conectando un pendrive, puede tardar entre 12 y 18 segundos en montar correctamente la unidad, así que el arranque del servicio samba, lo he retrasado 20 segundos. La red también la levantamos nostros manualmente haciendo uso del binario udhcpc.

Este es el bloque que he añadido al final del fichero /usr/local/etc/rcS
```
export SMBLOG=/usr/local/etc/smb.log
init_logger () {
date > $SMBLOG
}
logger () {
$@ >> $SMBLOG
}
enable_samba () {
sleep 20
cd /tmp/package/script
logger ./configsamba
logger ./samba start
logger date
}
init_logger
logger /sbin/udhcpc -i eth0 -n -s /etc/udhcpc.script -r 3
enable_samba &
```

Este script, deja el rastro de la salida de los comandos en el fichero /usr/local/etc/smb.log . De esta forma, sino funciona, podemos ver que ha ocurrido.

Dejo como adjunto mi fichero rcS con los cambios realizados.

Para copiar este fichero al hdp-r1 tendríamos que hacer lo siguiente:

  1. Descomprimir el fichero rcS.tar.gz y copiar el fichero rcS en un pendrive
  2. Enchufar el pendrive al hdp-r1 y asegurarnos de que no hay ninguna otra unidad conectada. Necesitamos la conexión de red.
  3. Encender el aparato desde el mando, ir a Peliculas » Carpeta » Red . Con esto nos aseguramos que se conecte a la red.
  4. Mirar la ip del aparato en la configuración, para ello, pulsamos en **setup**, Nos desplazamos hacia la izquierda hasta que llegemos a **RED** y podemos ver la **ip**
  5. Ejecutamos telnet ip en nuestro pc con acceso a la red
  6. Para acceder indicamos que somos root, no se nos pedirá contraseña
  7. Una vez dentro comprobamos que se ha montado correctamente el pendrive ejecutando
     ```
      ls /mnt/usbmounts/sda1
     ```
  8. Entonces, ejecutamos los siguientes comandos, uno a uno.
     ```
     cp /usr/local/etc/rcS /usr/local/etc/rcS.old
     cp /mnt/usbmounts/sda1/rcS /usr/local/etc/rcS
     reboot
     ```
  9. Tras reiniciar, sobre unos 30 segundos después, el servicio de samba debe haber arrancado. Si exploramos la red, ya sea con windows o con linux con los paquetes de samba instalados, veremos la máquina Venus en el grupo de trabajo Workgroup.

Se ha comprobado que si no hay unidad usb, el servicio arranca, pero no comparte nada. Además, si no hay conexión de red, el servicio no arranca pero no entorpece el arranque del sistema de Asus.

La velocidad de transferencia, tanto copiando al asus como desde el asus me han parecido más que aceptables para una red a 100M, en torno a los 6-9MB/s y si copiamos algo vía wifi, entorno a los 2MB/s. Eso sí, si intentamos reproducir contenidos vía lan en el asus mientras estamos realizamos alguna transferencia, el rendimiento de la reproducción decae bastante.

_NOTAS:_

  * He cambiado pirlo por pirlas tal y como me ha sugerido dicho usuario en el primer post.

[rcs.tar.gz](/wp-content/uploads/rcs.tar.gz)
