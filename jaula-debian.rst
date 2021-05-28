
Como crear una Jaula en Debian (chroot)
====================================

Preparamos el estación
++++++++++++++++++++

apt-get install debootstrap coreutils -y

$ j1=/home/laboratorio/jaula

$ echo $j1
/home/laboratorio/jaula

$ CODENAME=$(cat /etc/os-release | grep VERSION_CODENAME | awk -F"=" '{print $2}')

$ echo $CODENAME
buster

Ejecutar el debootstrap
++++++++++++++++++++++++

Ejecutamos el debootstrap para que descargue desde un repositorio oficial de Debian un sistema básico de Debian, esto tendrá un aproximado de 294M.

$ sudo debootstrap --arch=amd64 $CODENAME $j1 http://ftp.debian.org/debian

Hacer chroot (Conocido como jaula)
+++++++++++++++++++++++++++++++

chroot Ejecuta un comando o un shell interactivo en un directorio especial que se utilizara como raíz. Para más ayuda man chroot. Re-montamos los directorios (proc, dev y sys) de nuestro HOST en el directorio donde descargamos el Debian básico. Para saber más de estos directorios https://wiki.debian.org/es/FilesystemHierarchyStandard. Los montamos para tener acceso a la red y los disco.::

	$ sudo mount -o bind /proc $j1/proc 
	$ sudo mount -o bind /sys $j1/sys
	$ sudo mount -o bind /dev $j1/dev

++++++++++++++++++++++
::

	$ sudo chroot $j1
	root@debian:/# 
