# Trabajo Práctico Integrador: Configuración de Servidor Debian en Entorno Virtualizado

## Introducción

Este repositorio contiene los entregables y la documentación de un Trabajo Práctico Integrador cuyo objetivo principal es la configuración **end-to-end de un servidor GNU/Linux Debian 11 (Bullseye)** en un entorno virtualizado. El proyecto demuestra habilidades avanzadas en administración de sistemas, seguridad, despliegue de servicios y automatización, abarcando desde la configuración de la red y el almacenamiento hasta la implementación de una estrategia de backup y el control de versiones.

## Objetivos del Proyecto

El proyecto se desarrolló siguiendo una consigna estructurada para lograr los siguientes objetivos clave:

* **Configuración del Entorno Base**: Inicialización de una máquina virtual Debian pre-configurada, establecimiento de credenciales de `root`, configuración del `hostname` como `TPServer` y definición de una dirección IP estática.
* **Despliegue de Servicios Esenciales**: Instalación y configuración de un servidor SSH para acceso remoto seguro con clave público-privada, un servidor web Apache con soporte PHP para servir contenido dinámico y una base de datos MariaDB con carga inicial de datos.
* **Arquitectura de Almacenamiento Avanzada**: Incorporación y particionamiento de un disco virtual adicional de 10 GB para alojar `/www_dir` (3 GB) y `/backup_dir` (6 GB), formateo con `ext4` y configuración de montaje persistente mediante `/etc/fstab`. Se incluye la reconfiguración de Apache para usar el nuevo directorio web.
* **Estrategia de Backup y Automatización**: Desarrollo de un script Bash (`backup_full.sh`) capaz de realizar backups comprimidos de directorios específicos, validando rutas y generando nombres de archivo con formato de fecha. Se implementa la programación de tareas automáticas diarias y semanales utilizando `cron`.
* **Persistencia de Datos del Sistema**: Configuración de un servicio `systemd` para guardar la información de las particiones (`/proc/partitions`) en un archivo persistente en cada arranque.
* **Entregables y Control de Versiones**: Organización de los archivos del proyecto para su subida a un repositorio de GitHub, incluyendo los directorios clave del sistema comprimidos y un diagrama de topología de la infraestructura.

## Servicios y Configuraciones Implementadas

* **Máquina Virtual**: Debian 11 (Bullseye) en VirtualBox.
* **Gestión de Red**:
    * Configuración de IP estática (`/etc/network/interfaces`).
    * Servidores DNS persistentes (8.8.8.8, 8.8.4.4).
* **Acceso Remoto Seguro**:
    * OpenSSH Server instalado y configurado.
    * Acceso del usuario `root` habilitado (`PermitRootLogin yes`).
    * Autenticación basada en clave pública/privada (con permisos restrictivos en `/root/.ssh` y `authorized_keys`).
* **Servidor Web**:
    * Apache2 instalado.
    * Soporte para PHP (v7.3+) y módulo `php-mysql` para conexión con la base de datos.
    * `DocumentRoot` reconfigurado a `/www_dir`.
* **Base de Datos**:
    * MariaDB Server instalado.
    * Instalación asegurada (`mysql_secure_installation`).
    * Carga de base de datos inicial desde `db.sql`.
* **Gestión de Almacenamiento**:
    * Adición de un segundo disco de 10 GB.
    * Particionamiento con `fdisk` (`/dev/sdc1` de 3GB para `/www_dir`, `/dev/sdc2` de 6GB para `/backup_dir`).
    * Formateo de particiones con `ext4`.
    * Montaje persistente vía `/etc/fstab`.
* **Automatización**:
    * Script `backup_full.sh` en `/opt/scripts` para backups comprimidos (`.tar.gz`) con validaciones y timestamps.
    * Tareas `cron` programadas para `/var/log` (diario a las 00:00) y `/www_dir` (Lun, Mié, Vie a las 23:00).
* **Monitorización y Diagnóstico**: Uso de herramientas como `lsblk`, `ip a`, `systemctl status`, `tail -f` para verificar y diagnosticar el estado del sistema y los servicios.

## Contenido del Repositorio (Entregables)

Este repositorio incluye los siguientes archivos comprimidos, generados directamente desde la máquina virtual configurada:

* `root.tar.gz`
* `etc.tar.gz`
* `opt.tar.gz`
* `proc.tar.gz` (excluyendo `/proc/kcore`)
* `www_dir.tar.gz`
* `backup_dir.tar.gz`
* `var.tar.gz.part-aa` (y sus partes consecutivas, ej. `.part-ab`, `.part-ac`, etc.)

**Nota**: El archivo `var.tar.gz` completo **NO** se sube, solo sus partes divididas para cumplir con los límites de tamaño de GitHub.

## Diagrama de Topología

Se adjunta un diagrama de topología de la infraestructura, que ilustra la configuración de red y los componentes principales del sistema:

* **Máquina Física (Host)**
* **VirtualBox** (Software de virtualización)
* **VM TPServer** (Máquina Virtual Debian) con su IP estática (192.168.1.100)
* Conexión al **Router/Gateway** local
* Conexión a **Internet**


* Fausto Villalba
