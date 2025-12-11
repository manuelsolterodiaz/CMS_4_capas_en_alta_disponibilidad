# Instalación de CMS en arquitectura de 4 capas en alta disponibilidad.
Contiene los ficheros VagrantFile y de aprovisionamiento necesarios para esta tarea.
## Capas
- Capa 1: Capa pública. Balanceador de carga con Nginx.
- Capa 2: Backend (Dos servidores web y un servidor NFS y motor PHP_FPM).
- Capa 3: Balanceador de Base de Datos con HAProxy.
- Capa 4: Base de Datos (Dos servidores de base de datos con MariaDB)
## Índice

1. [Introduccción](#id1)
2. [Instalaciones](#id2)
3. [Scripts](#id3)

# Introducción

En esta práctica se va a realizar el despliegue de una aplicacion web que esta alojada en un repositorio público de GitHub.

La infraestructura es un balanceador con Nginx, dos servidores de web , un servidor NFS con la aplicacion alojada en una carpeta a la que acceder los servidores para mostrarla en el navegador, después en la misma maquina hay instalado el motor de PHP-FPM (separa el proceso del PHP del servidor web mediante el protocolo FastCGI), seguidamente hay un servidor que actua de balanceador en los servidores de base de datos, se ha usado HAProxy, y por último dos servidores de base de datos donde se aloja la base de datos.

### Direccionamiento

Para esta practica se han usado las diferentes redes:
- Red publica balanceador: 192.168.10.0/24
- Red privada servidores web: 192.168.20.0/24
- Red privada nfs: 192.168.30.0/24
- Red privada base de datos: 192.168.40.0/24
##### Por cada máquina
- Balanceador: Utiliza dos redes.
    - Red pública (redpublica): 192.168.10.0/24 - utiliza - 192.168.10.10
    - Red privada (redweb): 192.168.20.0/24 - utiliza - 192.168.20.14
- Server Web 1: Utiliza dos redes.
    - Red privada (redweb): 192.168.20.0/24 - utiliza - 192.168.20.10
    - Red privada (rednfs): 192.168.30.0/24 - utiliza - 192.168.30.11
- Server Web 2: Utiliza dos redes.
    - Red privada (redweb): 192.168.20.0/24 - utiliza - 192.168.20.11
    - Red privada (rednfs): 192.168.30.0/24 - utiliza - 192.168.30.12
- Server NFS: Utiliza dos redes.
    - Red privada (redweb): 192.168.20.0/24 - utiliza - 192.168.20.13
    - Red privada (rednfs): 192.168.30.0/24 - utiliza - 192.168.30.13
- Server HAProxy: Utiliza dos redes.
    - Red privada (rednfs): 192.168.30.0/24 - utiliza - 192.168.30.10
    - Red privada (reddatabase): 192.168.40.0/24 - utiliza - 192.168.40.10
- Server DB1: Utiliza una unica red.
    - Red privada (reddatabase): 192.168.40.0/24 - utiliza - 192.168.40.11
- Server DB2: Utiliza una unica red.
    - Red privada (reddatabase): 192.168.40.0/24 - utiliza - 192.168.40.12
