# 1. Servidor Samba
## 1.1 Usuarios locales

Vamos a GNU/Linux, y creamos los siguientes grupos y usuarios locales:
* Crear los grupos `piratas`, `soldados` y `sambausers`.
* Crear el usuario `sambaguest`. Para asegurarnos que nadie puede usar `sambaguest`

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-1samba.png)

entrar en nuestra máquina mediante login, vamos a modificar este usuario y le ponemos
como shell `/bin/false`. NOTA: Podemos hacer estos cambios por entorno gráfico usando Yast, o
por comandos editando el fichero `/etc/passwd`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-2samba.png)

* Dentro del grupo `piratas` incluir a los usuarios `pirata1`, `pirata2` y `supersamba`.
* Dentro del grupo `soldados` incluir a los usuarios `soldado1` y `soldado2` y `supersamba`.
* Dentro del grupo `sambausers`, poner a todos los usuarios `soldados`, `piratas`, `supersamba` y a `sambaguest`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-3samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-4samba.png)

## 1.2 Crear las carpetas para los futuros recursos compartidos

* Creamos la carpeta base para los recursos de red de Samba de la siguiente forma:
    * `mkdir /srv/sambaXX`
    * `chmod 755 /srv/sambaXX`
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-5samba.png)

## 1.3 Configurar el servidor Samba

* `cp /etc/samba/smb.conf /etc/samba/smb.conf.bak`, hacer una copia de seguridad del fichero de configuración antes de modificarlo.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-7samba.png)

    * `Yast -> Samba Server`
    * Workgroup: `curso2021`
    * Sin controlador de dominio.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-8samba.png)    

* En la pestaña de `Inicio` definimos
    * Iniciar el servicio durante el arranque de la máquina.
    * Ajustes del cortafuegos -> Abrir puertos
    
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-9samba.png)

> **Comprobar CORTAFUEGOS**
> Para descartar un problema del servidor Samba con el cortafuegos, usaremos
el comando `nmap -Pn IP-servidor-Samba` desde otra máquina GNU/Linux.
Los puertos SMB/CIFS (139 y 445) deben estar abiertos.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-8-1samba.png)

## 1.4 Crear los recursos compartidos de red

Vamos a configurar los recursos compartidos de red en el servidor.
Podemos hacerlo modificando el fichero de configuración o por entorno gráfico con Yast.

* Tenemos que conseguir una configuración con las secciones: `global`, `public`,
`barco`, y `castillo` como la siguiente:
    * Donde pone XX, sustituir por el número del puesto de cada uno.
    * `public`, será un recurso compartido accesible para todos los usuarios en modo lectura.
    * `barco`, recurso compartido de red de lectura/escritura para todos los piratas.
    * `castillo`, recurso compartido de red de lectura/escritura para todos los soldados.
    
    ![permisos](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/permisos3.png)
    
* Podemos modificar la configuración:
    * (a) Editando directamente el ficher `/etc/samba/smb.conf` o
    * (b) `Yast -> Samba Server -> Recursos compartidos -> Configurar`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-11samba.png)

* `testparm`, verificar la sintaxis del fichero de configuración.
* `more /etc/samba/smb.conf`, consultar el contenido del fichero de configuración.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-12samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-12-1samba.png)

## 1.5 Usuarios Samba

Después de crear los usuarios en el sistema, hay que añadirlos a Samba.
* `smbpasswd -a USUARIO`, para crear clave Samba de USUARIO.
    * **¡OJO!: NO te saltes este paso.**
    * USUARIO son los usuarios que se conectarán a los recursos compartidos SMB/CIFS.
    * Esto hay que hacerlo para cada uno de los usuarios de Samba.
    ![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-13samba.png)
    ![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-13-1samba.png)
    ![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-13-2samba.png)

* `pdbedit -L`, para comprobar la lista de usuarios Samba.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-14samba.png)
    
## 1.6 Reiniciar
* Ahora que hemos terminado con el servidor, hay que recargar los ficheros de configuración del servicio. Esto es, leer los cambios de configuración. Podemos hacerlo por `Yast -> Servicios`, o usar los comandos: `systemctl restart smb` y `systemctl restart nmb`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-15samba.png)

* `sudo lsof -i -Pn`, comprobar que el servicio SMB/CIF está a la escucha.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-16samba.png)

---

# 2. Windows

   *  Configurar el cliente Windows.
   *  Usar nombre y la IP que hemos establecido al comienzo.
   *  Configurar el fichero ...\etc\hosts de Windows.
   *  En los clientes Windows el software necesario viene preinstalado.
   
   ![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2samba.png)

## 2.1 Cliente Windows GUI

Desde un cliente Windows vamos a acceder a los recursos compartidos del servidor Samba.

* Escribimos `\\ip-del-servidor-samba` y vemos lo siguiente:

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-1samba.png)

* Acceder al recurso compartido `public`.
 
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-14faltabapublic.png)

* Acceder al recurso compartido `castillo` con el usuario `soldado`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-3samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-2samba.png)
   
   * `net use` para ver las conexiones abiertas.
   * `net use * /d /y`, para borrar todas las conexión SMB/CIFS que se hayan realizado.
    
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-3-1samba.png)

* Acceder al recurso compartido `barco` con el usuario `pirata`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-4samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-5samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-6samba.png)

* Ir al servidor Samba.
* Capturar imagen de los siguientes comandos para comprobar los resultados:
   * `smbstatus`, desde el servidor Samba.
    
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-7samba.png)

   * `lsof -i -Pn`, desde el servidor Samba.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-8samba.png)

## 2.2 Cliente Windows comandos

* Abrir una shell de windows.
Capturar imagen de los comandos siguientes:
* `net view \\IP-SERVIDOR-SAMBA`, para ver los recursos del servidor remoto.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-9.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-10.png)


Montar el recurso `barco` de forma persistente.
* `net use S: \\IP-SERVIDOR-SAMBA\recurso contraseña /USER:usuario /p:yes` crear una conexión con el recurso compartido y lo monta en la unidad S. Con la opción `/p:yes` hacemos el montaje persistente. De modo que se mantiene en cada reinicio de máquina.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-11.png)

* Ahora podemos entrar en la unidad S ("s:") y crear carpetas, etc.
* Capturar imagen de los siguientes comandos para comprobar los resultados:
    * `sudo smbstatus`, desde el servidor Samba.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-12.png)

   * `lsof -i -Pn`, desde el servidor Samba.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-13.png)

---

# 3 Cliente GNU/Linux
## 3.1 Cliente GNU/Linux GUI

Desde en entorno gráfico, podemos comprobar el acceso a recursos compartidos SMB/CIFS.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-1samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-4samba.png)

Capturar imagen de lo siguiente:
* Probar a crear carpetas/archivos en `castillo` y en  `barco`.
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-2samba.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-3samba.png)

* Comprobar que el recurso `public` es de sólo lectura.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-5samba.png)

* Capturar imagen de los siguientes comandos para comprobar los resultados:
    * `sudo smbstatus`, desde el servidor Samba.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-6samba.png)

   * `sudo lsof -i -Pn`, desde el servidor Samba.
    
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-7samba.png)

## 3.2 Cliente GNU/Linux comandos
> Existen comandos (`smbclient`, `mount` , `smbmount`, etc.) para ayudarnos
a acceder vía comandos al servidor Samba desde el cliente.
> Puede ser que con las nuevas actualizaciones y cambios de las distribuciones
alguno haya cambiado de nombre. ¡Ya lo veremos!

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-8samba.png.png)

* Vamos a un equipo GNU/Linux que será nuestro cliente Samba. Desde este
equipo usaremos comandos para acceder a la carpeta compartida.

* Probar desde el cliente GNU/Linux el comando `smbclient --list IP-SERVIDOR-SAMBA`, que muestra los recursos SMB/CIFS del servidor remoto.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-9samba.png.png)

> No olvidarse de parar el cortafuegos para poder hacer la conexion sin problemas

* Ahora crearemos en local la carpeta `/mnt/remotoXX/castillo`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-10samba.png.png)

* **MONTAJE MANUAL**: Con el usuario root, usamos el siguiente comando para montar un recurso compartido de Samba Server, como si fuera una carpeta más de nuestro sistema:

```
mount -t cifs //172.AA.XX.31/castillo /mnt/remotoXX/castillo -o username=soldado1
```
* `df -hT`, para comprobar que el recurso ha sido montado.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-11samba.png.png)


## 3.3 Montaje automático

* Hacer una instantánea de la MV antes de seguir. Por seguridad.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-13samba.png.png)

* Capturar imágenes del proceso.
* Reiniciar la MV.
* `df -hT`. Los recursos ya NO están montados. El montaje anterior fue temporal.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-14samba.png.png)

Para configurar acciones de montaje automáticos cada vez que se inicie el equipo,
debemos configurar el fichero `/etc/fstab`.

* Hacer una copia de seguridad al fichero `/etc/fstab`.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-15samba.png.png)

> No saque la captura pero hize una copia del fstab con el comando cp /etc/fstab /etc/fstabcopia

* Modificar el fichero, incluyendo una línea de la siguiente forma:

```
//IP-servidor-samba/public /mnt/remotoXX/public cifs username=soldado1,password=CLAVE-DE-SOLDADO1 0 0
```
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-16samba.png.png)

* Reiniciar el equipo y comprobar que se realiza el montaje automático al inicio.

![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-17samba.png.png)
![SMB](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/3-18samba.png.png)

---

