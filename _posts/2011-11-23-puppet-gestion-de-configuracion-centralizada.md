---
id: 72
title: Puppet, gestión de configuración centralizada.
date: 2011-11-23T14:48:39+00:00
author: ant30
layout: post
guid: http://www.wordpress.ant30.es/?p=72
permalink: /2011/11/puppet-gestion-de-configuracion-centralizada/
categories:
  - blog
tags:
  - Linux
  - puppet
  - software
---

[Puppet](http://puppetlabs.com/) es una herramienta gestión de configuraciones
centralizada que nos ayudará en la laboriosa gestión de servidores cuando el
número de estos empieza a crecer.

Recientemente ha cambiado de licencia, de GPL a Apache a partir de la versión
2.7.0. Está basado en Ruby, pero las descripciones de configuraciones se
escriben en modo declarativo. Existen paquetes construidos que ya vienen en el
sistema de paquetería para las principales distribuciones Linux, en nuestro
caso usaremos Debian Squeeze y los paquetes a los que tenemos acceso en su
repositorio. También tienen soporte para otros sistemas UNIX como Solaris,
\*BSD o Mac OS X. Y en desarrollo tienen un soporte básico de sistemas Windows.

El desarrollo es público desde 2006 y destaca por su estabilidad y facilidad de
manejo, aunque como todo este tipo de herramientas, la curva de aprendizaje, no
es muy liviana. Entre los principales clientes de la solución enterprise para
empresas destacan empresas como Nokia, RackSpace, Zynga, Twitter, Digg, ...

En este post, veremos como configurar Puppet en Debian Squeeze y podremos ver
dos ejemplos de aplicación de configuraciones.
<!--more-->

## ¿Porqué escribir sobre puppet?

Nunca he profundizado en el uso puppet, nunca más allá que algunas de pruebas
de concepto o funcionalidad y es por esta razón quiero escribir esta serie de
posts, obligarme a conocer mucho mejor esta herramienta. En nuestro caso
queremos usar puppet para controlar un entorno virtualizado con libvirt.
Nuestro objetivo es controlar solo a Guests, es decir, máquinas virtuales,
debido a que este es el entorno más heterogéneo y difícil de mantener, entre
nuestros componentes Hosts/Guests. Se supone, que en un sistema de
virtualización relativamente grande, todo está controlado mediante una
herramienta de orquestado, ya seas herramientas basadas en VMWare como vCenter
o vSphere o del tipo RedHat RHEV / Ovirt, Proxmox, Eucalyptus, OpenStack ...
Por tanto, los nodos de virtualización deberían estar bastante controlados.

Para la siguiente prueba usaremos tres máquinas procedentes de una misma
plantilla de kvm con una instalación de debian6 básica. En otro artículo
hablaré sobre como crear templates de kvm, pero para nuestro caso, imagínate
que son 3 máquinas virtuales recién instaladas usando virt-manager. En una de
las máquinas instalaremos lo que se denomina Master, que será el servidor
encargado de gestionar la configuraciones del resto.

El objetivo de la prueba enviar un par de ficheros a los clientes, cliente1 y
cliente2 a partir de ahora. Uno de estos ficheros tendrá un contenido diferente
según sea el receptor del fichero cliente1 o cliente2.

En la segunda prueba, instalaremos un par de paquetes y al igual que en la
prueba anterior, nos aseguraremos de que el paquete 1 se instale en las dos
máquinas, y que además, en el cliente1 se instale el paquete 2 y el paquete 3.

Tras bucear un poco en la red, no he encontrado ninguna guía actualizada para
debian 6 (squeeze), pero sí que he podido localizar muchas guías de
introducción, con las que he ido recomponiendo este post.

En mi caso, me ha ayudado bastante tanto la [documentación de
puppet](http://docs.puppetlabs.com/) como esta [Introducción a Puppet de la Universidad de
Florida](http://www.ctrip.ufl.edu/centralized-system-administration-debian-using-puppet).
Aunque en este último caso, los ejemplos hablan de debian lenny, es decir, una
versión de debian anterior. Realmente, como veremos en los ejemplos, no hay
mucha diferencia entre la configuración de Lenny y Squeeze, lo que nos permite
confiar en la estabilidad del código.

## Antes de empezar

Antes de empezar, nos debemos asegurar de que los nombres de las máquinas
resuelven correctamente entre ellas. Esto nos permitirá olvidarnos de problemas
con certificados y similares, que son difíciles de depurar después.

En mi caso, las dos máquinas clientes, tienen en el /etc/hosts una línea que
hace referencia al master, de forma que tiene un nombre del tipo
master.cluster. Además, en la máquina master.cluster tengo registrado los
nombres de los clientes como client1 y client2.

Por tanto, para verificar que todo está correctamente en cuanto a DNS debemos
asegurarnos que tenemos valores de este tipo para el comando hostname

```bash
root@master:~# hostname
master
root@master:~# hostname -f
master.cluster
```
Es un error muy habitual, que por ejemplo hostname -f nos de localhost o localhost.localdomain. Esto es debido a que la línea donde debemos especificar el alias hacia nuestra máquina, debe ser la primera del fichero /etc/hosts

## Preparando el master

Instalamos el paquete puppetmaster.

```bash
apt-get install puppetmaster
```

Una vez la instalación haya finalizado, hay que configurar el directorio donde se compartiran los ficheros, así que rango de IPs tendrá acceso al directorio de ficheros y plugins.

Abrimos el fichero fileserver.conf

```bash
editor /etc/puppet/fileserver.conf
```

Si estás usando libvirt para gestionar la máquina y usas la configuración de red que viene por defecto, tus máquinas virtuales tendrán una IP dentro del rango 192.168.122.0/24 . Por tanto, para nuestro ejemplo, usaremos este rango. Con esto, el fichero fileserver.conf quedaría de esta forma:

```bash
[files]
path /etc/puppet/files
allow 192.168.122.0/24
[plugins]
allow 192.168.122.0/24
```

También puedes usar nombres DNS, o directamente, permitir a todo el mundo con
una regla del tipo allow `*.` Puedes ver más información en la doc de
[puppetlab](http://docs.puppetlabs.com/guides/file_serving.html) sobre este
fichero de configuración.

Ahora vamos a crear los ficheros básicos de manifest casi vacíos.

Primero creamos el fichero /etc/puppet/manifests/nodes.pp

```puppet
node pruebanode {}
node 'client1.cluster' inherits pruebanode {}
node 'client2.cluster' inherits pruebanode {}
```

Para que este fichero sea tomado en cuenta, hay que crear el fichero
/etc/puppet/manifests/site.pp e incluir el fichero nodes.pp que hemos comentado
antes. En las reglas de estilo de puppet indican que además habría que crear en
este directorio un fichero modules.pp e importarlo desde site.pp, pero en
nuestro caso no va a ser necesario porque los modulos que crearemos se
importarán automáticamente.

Este es el contenido del fichero /etc/puppet/manifests/site.pp

```
import "nodes"
```

También es conveniente saber que el nombre del servicio en el servidor es
puppetmaster. Por tanto, para reiniciarlo, ejecutaríamos algo como:

```bash
service puppetmaster restart
```

## Preparando los clientes

Este proceso lo repetiremos en cada uno de las máquinas que queremos controlar.

Instalamos el paquete puppet

```bash
apt-get install puppet
```

Revisamos el fichero de configuración del cliente, que debería lucir así:

```bash
root@client1:~# cat /etc/puppet/puppet.conf
[main]
logdir=/var/log/puppet
vardir=/var/lib/puppet
ssldir=/var/lib/puppet/ssl
rundir=/var/run/puppet
factpath=$vardir/lib/facter
templatedir=$confdir/templates
[agent]
server=master.cluster
```

Para que pueda funcionar puppetd, el demonio que funciona en los clientes, es
necesario habilitarlo. Hay que editar el fichero `/etc/default/puppet`

```bash
editor /etc/default/puppet
```

Y debemos asegurarnos que START está confirado como yes

Arrancamos el servicio puppet con service

```bash
service puppet start
```

Una vez realizado esto, nos debemos fijar en el syslog del cliente, si vemos
líneas con información como esta:

```
Could not request certificate: Retrieved certificate does not match private key; please remove certificate from server and regenerate it with the current key
```

Está claro que algo no está funcionando. Verifica la configuración de DNS antes
de instalar como comenté en la sección de Antes de empezar

Ahora procedemos a verificar desde el master que se han registrado
correctamente los identificadores de las firmas de los clientes:

```bash
root@master:~# puppet cert --list --all
client1.cluster (00:00:ssl_fingerprint:00:00)
client2.cluster (00:00:ssl_fingerprint:00:00)
+ master.cluster (00:00:ssl_fingerprint:00:00)
```

Tenemos que firmar los certificados de los clientes

```bash
root@master:~# puppet cert --list
client2.cluster
client1.cluster
root@master:~# puppet cert --sign client1.cluster
notice: Signed certificate request for client1.cluster
notice: Removing file Puppet::SSL::CertificateRequest client1.cluster at '/var/lib/puppet/ssl/ca/requests/client1.cluster.pem'
root@master:~# puppet cert --sign client2.cluster
notice: Signed certificate request for client2.cluster
notice: Removing file Puppet::SSL::CertificateRequest client2.cluster at '/var/lib/puppet/ssl/ca/requests/client2.cluster.pem'
```

## Creación de un módulo básico y asignación de clientes al módulo

Podemos tener varios módulos. En un módulo es donde definiremos todas las
configuraciones y ficheros de un perfil de máquinas. También se puede tener una
configuración determinada. Como ejemplo, podríamos tener un módulo para
configurar un LAMP y luego otro para poder desplegar Drupal en el LAMP. Puedes
ver más información en [La doc de puppet](http://projects.puppetlabs.com/projects/puppet/wiki/Puppet_Modules)

Creamos la estructura básica:

```bash
mkdir -p /etc/puppet/modules/prueba/{manifests,files,templates}
```

Creamos un fichero vacío donde pondremos luego la receta de puppet

```bash
touch /etc/puppet/modules/prueba/manifests/init.pp
```

Ahora editamos el fichero de configuración de nodos. Este fichero lo tendremos
en la ruta `/etc/puppet/manifests/nodes.pp` y deberá tener este aspecto:

```puppet
node pruebanode {
    include prueba
}
node 'client1.cluster' inherits pruebanode {}
node 'client2.cluster' inherits pruebanode {}
```

Con esto, tendremos el modulo prueba aplicado a client1 y client2.

## Prueba 1: Distribución de ficheros

La primera prueba que realizaremos será colocar un fichero en una ruta en concreto de las dos máquinas.

En el directorio `/etc/puppet/prueba/files` creamos un fichero llamado
ficherodeprueba que contiene solo la siguiente frase:

```
esto es un fichero de prueba
```

Ahora nos dirigimos al init.pp del módulo prueba en
`/etc/puppet/modules/prueba/manifests/init.pp` y ponemos el siguiente contenido

```puppet
class prueba {
    file { "/root/ficherodeprueba":
        owner   => root,
        group   => root,
        mode    => 400,
        source  => "puppet:///modules/prueba/ficherodeprueba",
    }
}
```

Normalmente, la configuración puede tardar bastante tiempo en aplicarse, para
forzarla vamos a ejecutar el siguiente puppetrun en el master.

Además, si queremos verificar si funciona, puede ser de utilidad ejecutar el
proceso de actualización de forma manual y en modo de depuración, para esto
ejecutaríamos en el cliente:

```bash
root@client1:~# puppetd -vt && ls
info: Caching catalog for client1.cluster
info: Applying configuration version '1322082679'
notice: Finished catalog run in 0.05 seconds
ficherodeprueba
```

## Prueba 2: Instalación de software procedente de paquetería

En esta prueba, vamos a instalar un paquete diferente en cada máquina. En
client1 instalaremos nginx y en client2 instalaremos el paquete memcached.

Comenzamos con la creación del módulo para instalar nginx. Lo vamos a llamar
httpd y empezamos creando la estructura de directorios en el master:

```bash
mkdir -p /etc/puppet/modules/httpd/{manifests,files,templates}
```

Ahora en files crearemos un fichero estático para ser servido por nginx.
Creamos el fichero en `/etc/puppet/modules/httpd/wp-content/uploads/texto` y
añadimos unas cuantas líneas de texto plano.

Hay que crear el manifiest en `/etc/puppet/modules/httpd/manifests/init.pp` con
el siguiente contenido:

```puppet
class httpd {
    package { "nginx":
         ensure => latest,
    }
    file { "/var/www/texto":
        owner => "www-data",
        group => "www-data",
        mode => 640,
        ensure => present,
        source => "puppet:///httpd/texto"
    }
    service { "nginx::server":
        name => nginx,
        ensure => running,
        enable => true
    }
}
```

En este manifest, tenemos tres pasos. Primero instalamos el paquete nginx,
después incorporamos un fichero en un ruta de servicio y después arrancamos el
servicio nginx y nos aseguramos de que esté arrancado.

Para que se aplique esta ejecución en el servidor client1, tenemos que
incorporar la asignación en el fichero `/etc/puppet/manifests/nodes.pp` con lo
que el fichero quedaría como sigue:

```puppet
node pruebanode {
    include prueba
}

node 'client1.cluster' inherits pruebanode {
    include httpd
}

node 'client2.cluster' inherits pruebanode {}
```

Esta vez solo lo hemos incorporado en client1.cluster para que solo se instale en este servidor.

Una vez salvemos este fichero, cuando toque actualizar, o si forzamos
simplemente reiniciando el agente en el cliente con service puppet restart,
podremos ver si se ha instalado el paquete nginx, si está funcionando y si
además, se puede acceder al fichero de texto que hemos enviado.

Ahora vamos con la instalación memcached, creamos los directorios necesarios:

```bash
mkdir -p /etc/puppet/modules/memcached/{manifests,files,templates}
```

Hay que crear el manifiest en `/etc/puppet/modules/memcached/manifests/init.pp`
con el siguiente contenido:

```puppet
class memcached {
    package { "memcached":
        ensure => latest
    }

    service { "memcached::server":
        name => memcached,
        ensure => running,
        enable => true,
    }
}
```

Tras guardar, añadimos tal y como hicimos con nginx (httpd) en site.pp, este
módulo en la parte de client2, con una línea que contenga include memcached .
Una vez hecho esto, es cuestión de esperar a que se apliquen los cambios, o
forzar a que se apliquen reiniciando el agente en el cliente.

Pues sí que quedó largo al final.
