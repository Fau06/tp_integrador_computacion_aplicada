# Trabajo Práctico Integrador: Configuración de Servidor Debian en Entorno Virtualizado

## Introducción

Este repositorio contiene los entregables y la documentación de un Trabajo Práctico Integrador cuyo objetivo principal es la configuración **end-to-end de un servidor GNU/Linux Debian 11 (Bullseye)** en un entorno virtualizado. El proyecto demuestra habilidades avanzadas en administración de sistemas, seguridad, despliegue de servicios y automatización, abarcando desde la configuración de la red y el almacenamiento hasta la implementación de una estrategia de backup y el control de versiones.

## Objetivos del Proyecto

El proyecto se desarrolló siguiendo una consigna estructurada para lograr los siguientes objetivos clave:

* [cite_start]**Configuración del Entorno Base**: Inicialización de una máquina virtual Debian pre-configurada, establecimiento de credenciales de `root`, configuración del `hostname` como `TPServer` y definición de una dirección IP estática[cite: 25, 27, 28, 29].
* [cite_start]**Despliegue de Servicios Esenciales**: Instalación y configuración de un servidor SSH para acceso remoto seguro con clave público-privada, un servidor web Apache con soporte PHP para servir contenido dinámico y una base de datos MariaDB con carga inicial de datos[cite: 30, 31, 32].
* [cite_start]**Arquitectura de Almacenamiento Avanzada**: Incorporación y particionamiento de un disco virtual adicional de 10 GB para alojar `/www_dir` (3 GB) y `/backup_dir` (6 GB), formateo con `ext4` y configuración de montaje persistente mediante `/etc/fstab`[cite: 33, 34, 35, 148, 149, 150, 151, 152, 153, 154, 155, 156, 157, 158, 159, 160]. [cite_start]Se incluye la reconfiguración de Apache para usar el nuevo directorio web[cite: 36, 748].
* **Estrategia de Backup y Automatización**: Desarrollo de un script Bash (`backup_full.sh`) capaz de realizar backups comprimidos de directorios específicos, validando rutas y generando nombres de archivo con formato de fecha. [cite_start]Se implementa la programación de tareas automáticas diarias y semanales utilizando `cron`[cite: 37, 38, 224, 225, 226, 227, 228, 229, 230, 231, 232, 233, 234, 235, 236, 237, 238, 239].
* [cite_start]**Persistencia de Datos del Sistema**: Configuración de un servicio `systemd` para guardar la información de las particiones (`/proc/partitions`) en un archivo persistente en cada arranque[cite: 282, 283, 284, 285].
* [cite_start]**Entregables y Control de Versiones**: Organización de los archivos del proyecto para su subida a un repositorio de GitHub, incluyendo los directorios clave del sistema comprimidos y un diagrama de topología de la infraestructura[cite: 39, 40, 41].

## Servicios y Configuraciones Implementadas

* **Máquina Virtual**: Debian 11 (Bullseye) en VirtualBox.
* **Gestión de Red**:
    * [cite_start]Configuración de IP estática (`/etc/network/interfaces`)[cite: 87, 88, 89, 90].
    * [cite_start]Servidores DNS persistentes (8.8.8.8, 8.8.4.4)[cite: 455, 456].
* **Acceso Remoto Seguro**:
    * [cite_start]OpenSSH Server instalado y configurado[cite: 92].
    * [cite_start]Acceso del usuario `root` habilitado (`PermitRootLogin yes`)[cite: 94, 546].
    * [cite_start]Autenticación basada en clave pública/privada (con permisos restrictivos en `/root/.ssh` y `authorized_keys`)[cite: 95, 96, 97, 548, 549].
* **Servidor Web**:
    * [cite_start]Apache2 instalado[cite: 31, 263].
    * [cite_start]Soporte para PHP (v7.3+) y módulo `php-mysql` para conexión con la base de datos[cite: 263, 660].
    * [cite_start]`DocumentRoot` reconfigurado a `/www_dir`[cite: 36, 751].
* **Base de Datos**:
    * [cite_start]MariaDB Server instalado[cite: 32, 265].
    * [cite_start]Instalación asegurada (`mysql_secure_installation`)[cite: 266, 551].
    * [cite_start]Carga de base de datos inicial desde `db.sql`[cite: 32, 267, 554].
* **Gestión de Almacenamiento**:
    * [cite_start]Adición de un segundo disco de 10 GB[cite: 33, 693].
    * [cite_start]Particionamiento con `fdisk` (`/dev/sdc1` de 3GB para `/www_dir`, `/dev/sdc2` de 6GB para `/backup_dir`)[cite: 34, 715, 716].
    * [cite_start]Formateo de particiones con `ext4`[cite: 155, 734].
    * [cite_start]Montaje persistente vía `/etc/fstab`[cite: 35, 157, 737, 738].
* **Automatización**:
    * [cite_start]Script `backup_full.sh` en `/opt/scripts` para backups comprimidos (`.tar.gz`) con validaciones y timestamps[cite: 37, 288, 793].
    * [cite_start]Tareas `cron` programadas para `/var/log` (diario a las 00:00) y `/www_dir` (Lun, Mié, Vie a las 23:00)[cite: 38, 238, 239, 822].
* **Monitorización y Diagnóstico**: Uso de herramientas como `lsblk`, `ip a`, `systemctl status`, `tail -f` para verificar y diagnosticar el estado del sistema y los servicios.

## Contenido del Repositorio (Entregables)

Este repositorio incluye los siguientes archivos comprimidos, generados directamente desde la máquina virtual configurada:

* [cite_start]`root.tar.gz` [cite: 301, 831]
* [cite_start]`etc.tar.gz` [cite: 301, 831]
* [cite_start]`opt.tar.gz` [cite: 301, 831]
* [cite_start]`proc.tar.gz` [cite: 301, 834, 850] (excluyendo `/proc/kcore`)
* [cite_start]`www_dir.tar.gz` [cite: 301, 831]
* [cite_start]`backup_dir.tar.gz` [cite: 301, 831]
* [cite_start]`var.tar.gz.part-aa` (y sus partes consecutivas, ej. `.part-ab`, `.part-ac`, etc.) [cite: 301, 832, 872]

[cite_start]**Nota**: El archivo `var.tar.gz` completo **NO** se sube, solo sus partes divididas para cumplir con los límites de tamaño de GitHub[cite: 863, 873].

## Diagrama de Topología

Se adjunta un diagrama de topología de la infraestructura, que ilustra la configuración de red y los componentes principales del sistema:

* [cite_start]**Máquina Física (Host)** [cite: 302, 840]
* [cite_start]**VirtualBox** (Software de virtualización) [cite: 303, 841]
* [cite_start]**VM TPServer** (Máquina Virtual Debian) con su IP estática (192.168.1.100) [cite: 304, 841]
* [cite_start]Conexión al **Router/Gateway** local [cite: 304, 841]
* [cite_start]Conexión a **Internet** [cite: 305, 842]

* Fausto Villalba
