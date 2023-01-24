# 1. Contenedores con Docker

> Enlaces de interés
> * [Curso “Introducción a Docker”](https://sergarb1.github.io/CursoIntroduccionADocker/)
> * [Docker for beginners](http://prakhar.me/docker-curriculum/)
> * [getting-started-with-docker](http://www.linux.com/news/enterprise/systems-management/873287-getting-started-with-docker)
> * [Docker for Beginners](https://testdriven.io/blog/docker-for-beginners/)

Es muy común que nos encontremos desarrollando una aplicación, y llegue el momento que decidamos tomar todos sus archivos y migrarlos, ya sea al ambiente de producción, de prueba, o simplemente probar su comportamiento en diferentes plataformas y servicios.

Para este tipo de situaciones existen herramientas que nos facilitan el embalaje y despliegue de la aplicación, es aquí donde entra en juego los contenedores (Por ejemplo Docker o Podman).

Estas herramientas nos permite crear "contenedores", que son aplicaciones empaquetadas auto-suficientes, muy livianas, capaces de funcionar en prácticamente cualquier ambiente, ya que tiene su propio sistema de archivos, librerías, terminal, etc.

Docker es una tecnología contenedor de aplicaciones construida sobre LXC.

## 1.1 Instalación

> Enlaces de interés:
> * [EN - Docker installation on SUSE](https://docs.docker.com/engine/installation/linux/SUSE)
> * [ES - Curso de Docker en vídeos](jgaitpro.com/cursos/docker/)

Ejecutar como superusuario:
* `zypper in docker`, instalar docker en OpenSUSE (`apt install docker` en Debian/Ubuntu).

![dockersuper](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker1.png)

* `systemctl start docker`, iniciar el servicio. NOTA: El comando `docker daemon` hace el mismo efecto.
* `systemctl enable docker`, si queremos que el servicio de inicie automáticamente al encender la máquina.
* `cat /proc/sys/net/ipv4/ip_forward`, consultar el estado de IP_FORWARD. Debe estar activo=1.  

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2.png)

## 1.2 Primera prueba

Como usuario root:
* Incluir a nuestro usuario (nombre-del-alumno) como miembro del grupo `docker`. Solamente los usuarios dentro del grupo `docker` tendrán permiso para usarlo.
* `id NOMBRE-ALUMNO`, debe mostrar que pertenecemos al grupo `docker`.
* Cerrar sesión y volver a entrar al sistema con nuestro usuario normal.
![dockersuper](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker1-2.png)

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker1-2-1.png)

Como usuario normal:
* `docker version`, comprobamos que se muestra la información de las versiones cliente y servidor.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker1-2-2.png)

* `docker run hello-world`, este comando hace lo siguiente:
    * Descarga una imagen "hello-world"
    * Crea un contenedor y
    * ejecuta la aplicación que hay dentro.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker1-2-3.png)
    
* `docker images`, ahora vemos la nueva imagen "hello-world" descargada en nuestro equipo local.
* `docker ps -a`, vemos que hay un contenedor en estado 'Exited'.
* `docker stop IDContainer`, parar el conteneder identificado por su IDContainer. Este valor lo obtenemos tras consultar la salida del comando anterior (docker ps -a).
* `docker rm IDContainer`, eliminar el contenedor.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker1-2-4.png)

Hemos comprobado que Docker funciona correctamente.

## 1.3 TEORIA: Sólo para LEER

Tabla de referencia para no perderse:

| Software   | Base   | Sirve para crear   | Aplicaciones |
| ---------- | ------ | ------------------ | ------------ |
| VirtualBox | ISO    | Máquinas virtuales | N |
| Vagrant    | Box    | Máquinas virtuales | N |
| Docker     | Imagen | Contenedores       | 1 |

Veamos cómo es el flujo de trabajo con los contenedores Docker:

![](images/docker-flujo.png)

Comandos útiles de Docker:

| Comando                   | Descripción           |
| ------------------------- | --------------------- |
| docker stop CONTAINERID   | Parar un contenedor   |
| docker start CONTAINERID  | Iniciar un contenedor |
| docker attach CONTAINERID | Conectar el terminal actual con el contenedor |
| docker ps                 | mostrar los contenedores en ejecución |
| docker ps -a              | mostrar todos los contenedores (en ejecución o no) |
| docker rm CONTAINERID     | Eliminar un contenedor |
| docker rmi IMAGENAME      | Eliminar una imagen    |

## 1.4 Alias

Para ayudarnos a trabajar de forma más rápida con la línea de comandos podemos agregaremos `alias d='docker'` a nuestro fichero `$HOME/.alias`.

Ahora `d ps` equivale a `docker ps`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker1-4.png)

# 2. Creación manual de nuestra imagen

Nuestro SO base es OpenSUSE, pero vamos a crear un contenedor Debian,
y dentro instalaremos Nginx.

## 2.1 Crear un contenedor manualmente

**Descargar una imagen**
* `docker search debian`, buscamos en los repositorios de Docker Hub contenedores con la etiqueta `debian`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-1.png)

* `docker pull debian`, descargamos una imagen en local.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-1-1.png)

* `docker images`, comprobamos que se ha descargado.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-1-2.png)

**Crear un contenedor**: Vamos a crear un contenedor con nombre `app1debian` a partir de la imagen `debian`, y ejecutaremos el programa `/bin/bash` dentro del contendor:
* `docker run --name=app1debian -i -t debian /bin/bash`

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-1-3.png)

## 2.2 Personalizar el contenedor

Ahora estamos dentro del contenedor, y vamos a personalizarlo a nuestro gusto:

**Instalar aplicaciones dentro del contenedor**

```
root@IDContenedor:/# cat /etc/motd            # Comprobamos que estamos en Debian
root@IDContenedor:/# apt update
```
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-2.png)

root@IDContenedor:/# apt install -y nginx # Instalamos nginx en el contenedor

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-2-1.png)

root@IDContenedor:/# apt install -y nano  # Instalamos editor nano en el contenedor

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-2-2.png)

**Crear un fichero HTML** `holamundo1.html`.

root@IDContenedor:/# echo "<p>Hola nombre-del-alumno</p>" > /var/www/html/holamundo1.html

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-2-3.png)

**Crear un script** `/root/server.sh` con el siguiente contenido:

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-2-4.png)

**Recordatorio:**
* Hay que poner permisos de ejecución al script para que se pueda ejecutar (`chmod +x /root/server.sh`).
* La primera línea de un script, siempre debe comenzar por `#!/`, sin espacios.
* Este script inicia el programa/servicio y entra en un bucle, para mantener el contenedor activo y que no se cierre al terminar la aplicación.

## 2.3 Crear una imagen a partir del contenedor

Ya tenemos nuestro contenedor auto-suficiente de Nginx, ahora vamos a crear una nueva imagen que incluya los cambios que hemos hecho.

* Abrir otra ventana de terminal.
* `docker commit app1debian nombre-del-alumno/nginx1`, a partir del contenedor modificado vamos a crear la nueva imagen que se llamará "nombre-del-alumno/nginx1".

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-3.png)

> NOTA:
>
> * Los estándares de Docker estipulan que los nombres de las imágenes deben seguir el formato `nombreusuario/nombreimagen`.
> * Todo cambio realizado que no se acompañe de un commit a la imagen, se perderá en cuanto se cierre el contenedor.

* `docker images`, comprobamos que se ha creado la nueva imagen.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker2-3-1.png)

* Ahora podemos parar el contenedor, `docker stop app1debian` y
* Eliminar el contenedor, `docker rm app1debian`.

** No lo hize en este punto pero si al final de la practica

# 3. Crear contenedor a partir de nuestra nueva imagen

## 3.1 Crear contenedor con Nginx

Ya tenemos una imagen "nombre-alumno/nginx" con Nginx preinstalado dentro.
* `docker run --name=app2nginx1 -p 80 -t nombre-alumno/nginx1 /root/server.sh`, iniciar el contenedor a partir de la imagen anterior.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker3-1.png)

> El argumento `-p 80` le indica a Docker que debe mapear el puerto especificado del contenedor, en nuestro caso el puerto 80 es el puerto por defecto sobre el cual se levanta Nginx.

* No cierres la terminal. El contenedor ya está en ejecución y se queda esperando a que nos conectemos a él usando un navegador.
* Seguimos.

## 3.2 Comprobamos

* Abrimos una nueva terminal.
* `docker ps`, nos muestra los contenedores en ejecución. Podemos apreciar que la última columna nos indica que el puerto 80 del contenedor está redireccionado a un puerto local `0.0.0.0.:PORT -> 80/tcp`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker3-2.png)

* Abrir navegador web y poner URL `0.0.0.0.:PORT`. De esta forma nos conectaremos con el servidor Nginx que se está ejecutando dentro del contenedor.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker3-2-1.png)


* Comprobar el acceso al fichero HTML. Abrir navegador web y poner URL `0.0.0.0.:PORT/holamundo1.html`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker3-2-2.png)

* Paramos el contenedor `app2nginx1` y lo eliminamos.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker3-2-3.png)

Como ya tenemos una imagen docker con Nginx (Servidor Web), podremos crear nuevos contenedores cuando lo necesitemos.

## 3.3 Migrar la imagen a otra máquina

¿Cómo puedo llevar los contenedores Docker a un nuevo servidor?

> Enlaces de interés
>
> * https://www.odooargentina.com/forum/ayuda-1/question/migrar-todo-a-otro-servidor-imagenes-docker-397
> * http://linoxide.com/linux-how-to/backup-restore-migrate-containers-docker/

**Exportar** imagen Docker a fichero tar:
* `docker save -o alumnoXXdocker.tar nombre-alumno/nginx1`, guardamos la imagen
"nombre-alumno/nginx1" en un fichero tar.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker3-3.png)

Intercambiar nuestra imagen exportada con la de un compañero de clase.

**Importar** imagen Docker desde fichero:
* Coger la imagen de un compañero de clase.
* Nos llevamos el tar a otra máquina con docker instalado, y restauramos.
* `docker load -i alumnoXXdocker.tar`, cargamos la imagen docker a partir del fichero tar. Cuando se importa una imagen se muestra en pantalla las capas que tiene. Las capas las veremos en un momento.
* `docker images`, comprobamos que la nueva imagen está disponible.
* Probar a crear un contenedor (`app3tar`), a partir de la nueva imagen.

## 3.4 Capas

**Teoría sobre las capas**. Las imágenes de docker están creadas a partir de capas que van definidas en el fichero Dockerfile. Una de las ventajas de este sistema es que esas capas son cacheadas y se pueden compartir entre distintas imágenes, esto es que si por ejemplo la creación de nuestra imagen consta de 10 capas, y modificamos una de esas capas, a la hora de volver a construir la imagen solo se debe ejecutar esta nueva capa, el resto permanecen igual.

Estas capas a parte de ahorrarnos peticiones de red al bajarnos una nueva versión de una imagen también ahorra espacio en disco, ya que las capas que no se hayan cambiado entre versiones no se descargarán.

* `docker image history nombre_imagen:latest`, para consultar las capas de la imagen del compañero.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker3-4.png)

# 4. Dockerfile

Ahora vamos a conseguir el mismo resultado del apartado anterior, pero
usando un fichero de configuración. Esto es, vamos a crear un contenedor a partir de un fichero `Dockerfile`.

## 4.1 Preparar ficheros

* Crear directorio `/home/nombre-alumno/dockerXXlocal`.
* Entrar el directorio anterior.
* Crear fichero `holamundo2.html` con el siguiente contenido:

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4.png)

* Crear el fichero `Dockerfile` con el siguiente contenido:

```
FROM debian

MAINTAINER nombre-del-alumnoXX 1.0

RUN apt update
RUN apt install -y apt-utils
RUN apt install -y nginx

COPY holamundo2.html /var/www/html
RUN chmod 666 /var/www/html/holamundo2.html

EXPOSE 80

CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
```
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-1-1.png)

> * Enlace de interés: https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-docker/

Descripción de los parámetros del Dockerfile:

| Parámetro  | Descripción |
| ---------- | ----------- |
| FROM       | Imagen a partir de la cual se creará el contenedor |
| MAINTAINER | Información del autor |
| RUN        | Comando que se ejeuctará dentro del contenedor |
| COPY       | Copiar un fichero dentro del contenedor |
| EXPOSE     | Puerto de contenedor que será visible desde el exterior |
| CMD        | Comando que se ejecutará al iniciar el contenedor |

> Ahora no nos hace falta el script /root/server.sh que mantenía la aplicación "despierta" porque estamos invocando (Instrucción CMD) al servidor Nginx con los parámetros "-g" y "daemon off;" que mantienen el servicio activo.

## 4.2 Crear imagen a partir del `Dockerfile`

El fichero Dockerfile contiene toda la información necesaria para construir el contenedor, veamos:

* `cd dockerXXlocal`, entramos al directorio con el Dockerfile.
* `docker build -t nombre-alumno/nginx2 .`, construye una nueva imagen a partir del Dockerfile. OJO: el punto final es necesario.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-1-2.png)

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-2.png)

* `docker images`, ahora debe aparecer nuestra nueva imagen.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-2-1.png)

## 4.3 Crear contenedor y comprobar

A continuación vamos a crear un contenedor con el nombre `app4nginx2`, a partir de la imagen `nombre-alumno/nginx2`. Probaremos con:

```
docker run --name=app4nginx2 -p 8082:80 -t nombre-alumno/nginx2
```
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-3.png)

* El terminal se ha quedado "bloqueado" porque el comando anterior no ha terminado y lo hemos lanzado en primer plano (foreground). Vamos a abrir otro terminal.

Desde otra terminal:
* `docker ps`, para comprobar que el contenedor está en ejecución y en escucha por el puerto deseado.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-3-1.png)

* Comprobar en el navegador:
    * URL `http://localhost:PORTNUMBER`
    
    ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-3-2.png)
    
    * URL `http://localhost:PORTNUMBER/holamundo2.html`
    
    ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-3-3.png)

Ahora que sabemos usar los ficheros Dockerfile, vemos que es más sencillo usar estos ficheros para intercambiar con nuestros compañeros que las herramientas de exportar/importar que usamos anteriormente.

## 4.4 Usar imágenes ya creadas

El ejemplo anterior donde creábamos una imagen Docker con Nginx, pero esto se puede simplificar aún más si aprovechamos las imágenes oficiales que ya existen.

> Enlace de interés:
> * [nginx - Docker Official Images] https://hub.docker.com/_/nginx

* Crea el directorio `dockerXXweb`. Entrar al directorio.
* Crear fichero `holamundo3.html` con:
    * Proyecto: dockerXXb
    * Autor: Nombre del alumno
    * Fecha: Fecha actual
    
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-4.png)

* Crea el siguiente `Dockerfile`

```
FROM nginx

COPY holamundo3.html /usr/share/nginx/html
RUN chmod 666 /usr/share/nginx/html/holamundo3.html
```
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-4-1.png)

* Poner el el directorio `dockerXXb` los ficheros que se requieran para construir el contenedor.
* `docker build -t nombre-alumno/nginx3 .`, crear la imagen.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-4-2.png)

* `docker run -d --name=app5nginx3 -p 8083:80 nombre-alumno/nginx3`, crear contenedor. En este caso hemos añadido la opción "-d" que sirve para ejecutar el contenedor en segundo plano (background).

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-4-3.png)

* Comprobar el acceso a "holamundo3.html".

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker4-4-4.png)

# 5. Docker Hub

Ahora vamos a crear un contenedor "holamundo" y subirlo a Docker Hub.

## 5.1 Creamos los ficheros necesarios

Crear nuestra imagen "holamundo":

* Crear carpeta `dockerXXpush`. Entrar en la carpeta.
* Crear un script (`holamundoXX.sh`) con lo siguiente:

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker5-1.png)

Este script muestra varios mensajes por pantalla al ejecutarse.

* Crear fichero Dockerfile

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker5-1-1.png)

* A partir del Dockerfile anterior crearemos la imagen `nombre-alumno/holamundo`.
* Comprobar que `docker run nombre-alumno/holamundo` se crea un contenedor que ejecuta el script. Eliminar el contenedor si todo va bien.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker5-1-2.png)

## 5.2 Subir la imagen a Docker Hub

* Registrarse en Docker Hub.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker5-2.png)

* `docker login -u USUARIO-DOCKER`, para abrir la conexión.
* `docker tag nombre-alumno/holamundo:latest USUARIO-DOCKER/holamundo:version1`, etiquetamos la imagen con "version1".
* `docker push USUARIO-DOCKER/holamundo:version1`, para subir la imagen (version1) a los repositorios de Docker.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker5-2-1.png)

# 6. Limpiar contenedores e imágenes

Cuando terminamos con los contenedores, y ya no lo necesitamos, es buena idea pararlos y/o destruirlos.

* `docker ps -a`, identificar todos los contenedores que tenemos.
* `docker stop ...`, parar todos los contenedores.
* `docker rm ...`, eliminar los contenedores.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker6-1.png)

Hacemos lo mismo con las imágenes. Como ya no las necesitamos las eliminamos:

* `docker images`, identificar todas las imágenes.
* `docker rmi ...`, eliminar las imágenes.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/docker/images/docker6-1.png)
