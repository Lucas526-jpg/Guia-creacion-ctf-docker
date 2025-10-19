<h1 align="center"> Guia de creacion de ctf's para docker </h1>

### Empezamos creando una carpeta para guardar nuestro archivo docker con los siguientes comandos

mkdir ctf-docker
cd ctf-docker

### Creamos el archivo dockerfile(necesario para todas las configuraciones que tendra nuestro ctf) con el siguiente comando

nano dockerfile

### 1. Imagen Base

FROM debian:latest

<em> Define la imagen base a partir de la cual se construirá el contenedor. En este caso, utiliza la última versión estable del sistema operativo Debian. </em>

### 2. Actualización Inicial (Fase de Construcción)

RUN apt update && apt upgrade -y

<strong>RUN</strong>: <em> Ejecuta comandos durante la fase de construcción de la imagen.</em>

<strong>apt update</strong>:	<em>Actualiza la lista de paquetes disponibles en los repositorios de Debian.</em>

<strong>apt upgrade</strong> 	<em>Este comando actualizaría todos los paquetes instalados a sus versiones más recientes.</em>

<strong>-y<strong>: <em>-y es para aceptar automáticamente la instalación.</em>

### 3. Creación de Usuario Inseguro (Vector de Acceso Inicial)

RUN useradd -m -s /bin/bash bob\ && echo "bob:password1

useradd -m -s /bin/bash bob	

<em>Crea un nuevo usuario llamado bob (useradd bob), crea su directorio personal (-m), y establece su shell predeterminada como Bash (-s /bin/bash).</em>

### 4. Escalada de Privilegios

RUN chmod u+s /usr/bin/env

Modifica los permisos de un binario común, creando otro vector de escalada.

<strong>chmod u+s</strong>:	<em>Establece el bit SUID (Set User ID) en el archivo.</em>

<strong>/usr/bin/env<strong>:	<em>Es un programa común que ejecuta otro programa en un entorno modificado.</em>

### 5.Instalación de Servicios Adicionales

RUN apt install apache2 vsftpd -y

Instala dos servicios de red comunes que pueden contener vulnerabilidades de configuración o de versión.

<strong>apache2</strong>:	Instala el popular servidor web Apache. Esto abre el puerto 80 (HTTP) para posibles ataques web (inyecciones SQL, XSS, etc.).

<strong>vsftpd</strong>:	Instala el servicio de servidor FTP. Esto abre el puerto 21 (FTP) para ataques de fuerza bruta, archivos de configuración inseguros o acceso anónimo.

### Arranque de Servicios (Fase de Ejecución)

CMD service apache2 start && service vsftpd start && tail -f /dev/null	

<strong>CMD<strong>: Define el comando que se ejecuta cuando el contenedor inicia.
<strong>service apache2 start</strong>: Inicia el servidor web Apache.
<strong>service vsftpd start</strong>:	Inicia el servidor FTP.
<strong>tail -f /dev/null</strong>: Un contenedor se detiene si su proceso principal termina. Este comando ejecuta tail indefinidamente, manteniendo abierto el archivo /dev/null. Esto asegura que el contenedor permanezca activo para que los servicios Apache y VSFTPD sigan ejecutándose en segundo plano.
