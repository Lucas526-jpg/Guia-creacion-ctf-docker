<h1 align="center"> Guia de creacion de ctf's para docker </h1>

### Empezamos creando una carpeta para guardar nuestro archivo docker con los siguientes comandos

mkdir ctf-docker
cd ctf-docker

### Creamos el archivo dockerfile(necesario para todas las configuraciones que tendra nuestro ctf) con el siguiente comando

nano dockerfile

### 1. Imagen Base

FROM debian:latest

<em> Define la imagen base a partir de la cual se construirá el contenedor. En este caso, utiliza la última versión estable del sistema operativo Debian. </em>

