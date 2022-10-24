# 1. Servidor Samba
## 1.1 Usuarios locales

Vamos a GNU/Linux, y creamos los siguientes grupos y usuarios locales:
* Crear los grupos `piratas`, `soldados` y `sambausers`.
* Crear el usuario `sambaguest`. Para asegurarnos que nadie puede usar `sambaguest`

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1samba.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-1samba.png)

entrar en nuestra máquina mediante login, vamos a modificar este usuario y le ponemos
como shell `/bin/false`. NOTA: Podemos hacer estos cambios por entorno gráfico usando Yast, o
por comandos editando el fichero `/etc/passwd`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-2samba.png)

* Dentro del grupo `piratas` incluir a los usuarios `pirata1`, `pirata2` y `supersamba`.
* Dentro del grupo `soldados` incluir a los usuarios `soldado1` y `soldado2` y `supersamba`.
* Dentro del grupo `sambausers`, poner a todos los usuarios `soldados`, `piratas`, `supersamba` y a `sambaguest`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-3samba.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-4samba.png)

## 1.2 Crear las carpetas para los futuros recursos compartidos

* Creamos la carpeta base para los recursos de red de Samba de la siguiente forma:
    * `mkdir /srv/sambaXX`
    * `chmod 755 /srv/sambaXX`
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-5samba.png)

## 1.3 Configurar el servidor Samba

* `cp /etc/samba/smb.conf /etc/samba/smb.conf.bak`, hacer una copia de seguridad del fichero de configuración antes de modificarlo.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-7samba.png)

    * `Yast -> Samba Server`
    * Workgroup: `curso2021`
    * Sin controlador de dominio.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-8samba.png)    

* En la pestaña de `Inicio` definimos
    * Iniciar el servicio durante el arranque de la máquina.
    * Ajustes del cortafuegos -> Abrir puertos
    
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-9samba.png)

> **Comprobar CORTAFUEGOS**
> Para descartar un problema del servidor Samba con el cortafuegos, usaremos
el comando `nmap -Pn IP-servidor-Samba` desde otra máquina GNU/Linux.
Los puertos SMB/CIFS (139 y 445) deben estar abiertos.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-8-1samba.png)

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

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-11samba.png)

* `testparm`, verificar la sintaxis del fichero de configuración.
* `more /etc/samba/smb.conf`, consultar el contenido del fichero de configuración.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-12samba.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-12-1samba.png)

## 1.5 Usuarios Samba

Después de crear los usuarios en el sistema, hay que añadirlos a Samba.
* `smbpasswd -a USUARIO`, para crear clave Samba de USUARIO.
    * **¡OJO!: NO te saltes este paso.**
    * USUARIO son los usuarios que se conectarán a los recursos compartidos SMB/CIFS.
    * Esto hay que hacerlo para cada uno de los usuarios de Samba.
    ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-13samba.png)
    ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-13-1samba.png)
    ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-13-2samba.png)

* `pdbedit -L`, para comprobar la lista de usuarios Samba.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-14samba.png)
    
## 1.6 Reiniciar
* Ahora que hemos terminado con el servidor, hay que recargar los ficheros de configuración del servicio. Esto es, leer los cambios de configuración. Podemos hacerlo por `Yast -> Servicios`, o usar los comandos: `systemctl restart smb` y `systemctl restart nmb`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-15samba.png)

* `sudo lsof -i -Pn`, comprobar que el servicio SMB/CIF está a la escucha.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/1-16samba.png)

---

# 2. Windows

   *  Configurar el cliente Windows.
   *  Usar nombre y la IP que hemos establecido al comienzo.
   *  Configurar el fichero ...\etc\hosts de Windows.
   *  En los clientes Windows el software necesario viene preinstalado.
   
   ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2samba.png)

## 2.1 Cliente Windows GUI

Desde un cliente Windows vamos a acceder a los recursos compartidos del servidor Samba.

* Escribimos `\\ip-del-servidor-samba` y vemos lo siguiente:

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-1samba.png)

* Acceder al recurso compartido `public`.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/15-1windowscliente.png)
* Acceder al recurso compartido `castillo` con el usuario `soldado`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/SMB/images/2-3samba.png)
    
    * `net use` para ver las conexiones abiertas.
    * `net use * /d /y`, para borrar todas las conexión SMB/CIFS que se hayan realizado.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/15-4windowscliente.png)
* Acceder al recurso compartido `barco` con el usuario `pirata`.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/36barcos.png)
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/37barcos.png)
* Ir al servidor Samba.
* Capturar imagen de los siguientes comandos para comprobar los resultados:
    * `smbstatus`, desde el servidor Samba.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/38barcos.png)
    * `lsof -i -Pn`, desde el servidor Samba.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/39barcos.png)

## 2.2 Cliente Windows comandos

* Abrir una shell de windows.
Capturar imagen de los comandos siguientes:
* `net view \\IP-SERVIDOR-SAMBA`, para ver los recursos del servidor remoto.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/16configuracionsambaserver.png)
Montar el recurso `barco` de forma persistente.
* `net use S: \\IP-SERVIDOR-SAMBA\recurso contraseña /USER:usuario /p:yes` crear una conexión con el recurso compartido y lo monta en la unidad S. Con la opción `/p:yes` hacemos el montaje persistente. De modo que se mantiene en cada reinicio de máquina.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/17configuracionsambaserver.png)

* Ahora podemos entrar en la unidad S ("s:") y crear carpetas, etc.
* Capturar imagen de los siguientes comandos para comprobar los resultados:
    * `sudo smbstatus`, desde el servidor Samba.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/18configuracionsambaserver.png)
    * `lsof -i -Pn`, desde el servidor Samba.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/19configuracionsambaserver.png)
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/20configuracionsambaserver.png)

---

# 3 Cliente GNU/Linux
## 3.1 Cliente GNU/Linux GUI

Desde en entorno gráfico, podemos comprobar el acceso a recursos compartidos SMB/CIFS.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/21configuracionclientelinux.png)


Capturar imagen de lo siguiente:
* Probar a crear carpetas/archivos en `castillo` y en  `barco`.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/22configuracionclientelinux.png)
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/23configuracionclientelinux.png)
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/24configuracionclientelinux.png)
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/25configuracionclientelinux.png)

* Comprobar que el recurso `public` es de sólo lectura.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/26configuracionclientelinux.png)
* Capturar imagen de los siguientes comandos para comprobar los resultados:
    * `sudo smbstatus`, desde el servidor Samba.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/27comprobacionclientelinux.png)
    * `sudo lsof -i -Pn`, desde el servidor Samba.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/28comprobacionclientelinux.png)
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/28-2comprobacionclientelinux.png)
## 3.2 Cliente GNU/Linux comandos
Capturar imagenes de todo el proceso.

* Vamos a un equipo GNU/Linux que será nuestro cliente Samba. Desde este
equipo usaremos comandos para acceder a la carpeta compartida.

* Probar desde el cliente GNU/Linux el comando `smbclient --list IP-SERVIDOR-SAMBA`, que muestra los recursos SMB/CIFS del servidor remoto.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/29clientelinuxcomandos.png)

>No olvidarse de parar el cortafuegos para poder hacer la conexion sin problemas

* Ahora crearemos en local la carpeta `/mnt/remotoXX/castillo`.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/30clientelinuxcomandos.png)
* **MONTAJE MANUAL**: Con el usuario root, usamos el siguiente comando para montar un recurso compartido de Samba Server, como si fuera una carpeta más de nuestro sistema:

```
mount -t cifs //172.AA.XX.31/castillo /mnt/remotoXX/castillo -o username=soldado1
```
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/31clientelinuxcomandos.png)
No olvidarse de poner sudo o ponerse como super usuario.

* `df -hT`, para comprobar que el recurso ha sido montado.

![]()

> * Si montamos la carpeta de `castillo`, lo que escribamos en `/mnt/remotoXX/castillo`
debe aparecer en la máquina del servidor Samba. ¡Comprobarlo!
> * Para desmontar el recurso remoto usamos el comando `umount`.

* Capturar imagen de los siguientes comandos para comprobar los resultados:
    * `sudo smbstatus`, desde el servidor Samba.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/32clientelinuxcomandos.png)
    * `sudo lsof -i -Pn`, desde el servidor Samba.


## 3.3 Montaje automático

* Hacer una instantánea de la MV antes de seguir. Por seguridad.
* Capturar imágenes del proceso.
* Reiniciar la MV.
* `df -hT`. Los recursos ya NO están montados. El montaje anterior fue temporal.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/33clientelinuxcomandos.png)

Para configurar acciones de montaje automáticos cada vez que se inicie el equipo,
debemos configurar el fichero `/etc/fstab`.

* Hacer una copia de seguridad al fichero `/etc/fstab`.
> No saque la captura pero hize una copia del fstab con el comando cp /etc/fstab /etc/fstabcopia

* Modificar el fichero, incluyendo una línea de la siguiente forma:

```
//IP-servidor-samba/public /mnt/remotoXX/public cifs username=soldado1,password=CLAVE-DE-SOLDADO1 0 0
```
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/34clientelinuxcomandos.png)
* Reiniciar el equipo y comprobar que se realiza el montaje automático al inicio.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u2/samba/images/35clientelinuxcomandos.png)

---

