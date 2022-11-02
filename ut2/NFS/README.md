```
* Usada en los cursos 201516 y 201617.
```

# NFS (Network File System)

NFS es un protocolo para compartir recursos (directorios y archivos) por red entre sistemas operativos heterogéneos.

## Ejemplo de rúbrica:

| Sección               | Muy bien (2) | Regular (1) | Poco adecuado (0) |
| --------------------- | ------------ | ----------- | ----------------- |
| (1.1) Serv. NFS iniciado en Windows | | | |
| (1.2) Cliente accede a los recursos NFS | | | |
| (2.1) Serv. NFS iniciado en OpenSUSE | | | |
| (2.2) Cliente accede a los recursos NFS | | | |

# 1. Requisitos Windows

Vamos a necesitar las siguientes máquinas:

| MV         | Sistema Operativo           | Configuración |
| ---------- | --------------------------- | ------------- |
| NFS server | Windows Server (Enterprise) | [ Configurar MV ](./../../global/configuracion/windows-server.md)|
| NFS client | Windows (Enterprise) | [ Configurar MV ](./../../global/configuracion/windows.md) |

> **IMPORTANTE** usar la versión especificada porque el cliente NFS sólo lo tienen las versiones Enterprise.

# 2. Servidor NFS Windows

> Vídeos de interés:
>
> * [Vídeo que explica cómo instalar NFS en Windows Server](https://www.youtube.com/embed/1yigsSPwxds)
> * [Vídeo: NFS - Parte 1. SO Windows 7 FUNCIONANDO](http://www.youtube.com/watch?v=QWx-WlKf1DY&feature=youtu.be)
>
> Enlaces de interés:
>
> * Cliente NFS: [Montar directorios NFS bajo Windows 7](http://www.muspells.net/blog/2011/08/montar-directorios-nfs-bajo-windows-7/)
> * Servidor NFS: [Servidor NFS para Windows con WinNFSd](https://robleshermoso.wordpress.com/2010/07/15/tip-servidor-nfs-para-windows/)
> * Cliente NFS: [Compartir carpetas Windows mediante Servidor NFS](https://support.microsoft.com/es-es/kb/324089)
> * Comandos NFS: [Guía paso a paso de Servicios para NFS para Windows Server 2008 R2](https://support.microsoft.com/es-es/kb/324089)
>

## 2.1 Instalación del servicio NFS

Vamos a la MV con Windows 2008 Server
* Agregar rol `Servidor de Archivos`.
* Marcar `Servicio para NFS`.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver1.png)
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver2.png)
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver3.png)
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver4.png)
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver5.png)
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver6.png)

## 2.2 Configurar el servidor NFS

* Crear la carpeta `c:\exportsXX\public`.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver7.png)

    * Configurar en `Carpeta -> Botón derecho propiedades -> Compartir NFS`.
    * En modo lectura/escritura con NFS.
    * Acceso a todos los equipos.
    
   ![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver8.png)
    
* Crear la carpeta `c:\exportsXX\private`.
    * Configurar en `Carpeta -> Botón derecho propiedades -> Compartir NFS`.
    * Sólo en modo sólo lectura.
    * Acceso sólo al equipo cliente.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver9.png)

> En caso de problemas al acceder desde el cliente, configurar en el servidor
el recurso con "Permitir Acceso Anónimo".

* Ejecutamos el comando `showmount -e IP-DEL-SERVIDOR`, para comprobar que los recursos exportados.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver10.png)

# 3. Cliente NFS

Algunas versiones de Windows permiten trabajar con directorios de red NFS nativos de sistemas UNIX.
En esta sección veremos como montar y desmontar estos directorios bajo un entorno de Windows 7
Enterprise (Las versiones home y starter no tienen soporte para NFS).

## 3.1 Instalar el soporte cliente NFS bajo Windows

* En primer lugar vamos a instalar el componente cliente NFS para Windows.
Para ello vamos a `Panel de Control -> Programas -> Activar o desactivar características de Windows`.

Captura imagen del resultado final.
* Nos desplazamos por el menú hasta localizar Servicios para NFS y dentro de este, Cliente NFS.
* Marcamos ambos y le damos a Aceptar.
* En unos instantes tendremos el soporte habilitado.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver11.png)

Iniciar el servicio cliente NFS. Captura imagen del proceso.
* Para iniciar el servicio NFS en el cliente, abrimos una consola con permisos de Administrador.
* Ejecutamos el siguiente comando: `nfsadmin client start`

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver12.png)

## 3.2 Montando el recurso

Ahora necesitamos montar el recurso remoto para poder trabajar con él.
* Esto no lo hacemos con Administrador, sino con nuestro usuario normal.
* Consultar desde el cliente los recursos que ofrece el servidor: `showmount -e ip-del-servidor`
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver13.png)

* Montar recurso remoto: `mount –o anon,nolock,r,casesensitive \\ip-del-servidor\Nombre-recurso-NFS *`
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver14.png)
![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsservercomprobacion19.png)

> **Descripción de los parámetros**
>
> * anon: Acceso anónimo al directorio de red.
> * nolock: Deshabilita el bloqueo. Esta opción puede mejorar el rendimiento si sólo necesita leer archivos.
> * r: Sólo lectura. Para montar en modo lectura/escritura no usaremos este parámetro.
> * casesensitive: Fuerza la búsqueda de archivos con distinción de mayúsculas y minúsculas (similar a los clientes de NFS basados en UNIX).

* Hemos decidido asignar la letra de unidad de forma automática, así que si no hay otras unidades de red
en el sistema nos asignará la Z.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsserver15.png)


> Si hay problemas, entonces:
>
> * Comprobar que la configuración del cortafuegos del servidor permite accesos NFS.
> * Desde un cliente GNU/Linux hacer `nmap IP-del-servidor -Pn`. Debe aparecer abierto el puerto del servicio NFS

* Comprobar en el cliente los recursos montados con `net use`.
![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsservercomprobacion16.png)
![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsservercomprobacion20.png)

* Leer y escribir en los recursos compartidos.

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsservercomprobacion17.png)

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsservercomprobacion21.png)

* `netstat -n`, para comprobar el acceso a los recursos NFS desde el cliente.
![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/windows/windowsservercomprobacion18.png)
* Para desmontar la unidad simplemente escribimos en una consola: `umount z:`

---

# 4. SO OpenSUSE

Vamos a necesitar las siguientes máquinas:

| MV         | Sistema Operativo | Configuración | Nombre del host |
| ---------- | ----------------- | ------------- | --------------- |
| NFS server | SO OpenSUSE | [Configurar MV](./../../global/configuracion/opensuse.md) | serverXX |
| NFS client | SO OpenSUSE | [Configurar MV](./../../global/configuracion/opensuse.md) | clientXX |

> NOTA:
> * Para cambiar el nombre de máquina podemos usar Yast o modificar directamente los ficheros `/etc/hostname` y `/etc/hosts`.
> * Podemos configurar el fichero /etc/hosts del cliente y servidor, añadiendo estas líneas:
> ```
> 172.18.XX.52 serverXX.curso2122   serverXX
> 172.18.XX.62 clientXX.curso2122   clientXX
> ```

## 4.1 Servidor NFS

> Enlaces de interés:
>
> * Vídeo [LPIC-2 202 NFS Server Configuration](https//www.youtube.com/embed/Kc3LZum5DIQ?list=UUFFLP0dKesrKWccYscdAr9A)
> * Vídeo [NFS. Learning Linux: Lesson 17 NFS Server and Installation Repository](https//www.youtube.com/embed/9N8QTh-oYis?list=PL3E447E094F7E3EBB)
> * Enlace de interés a [NFS Sistema de Archivos de red](http://recursostic.educacion.es/observatorio/web/es/software/software-general/733-nfs-sistema-de-archivos-de-red)

Instalar servidor NFS por Yast.
![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/6cupsconfig.png)


* Crear las siguientes carpetas/permisos:
    * `/srv/exportsXX/public`, usuario y grupo propietario `nobody:nogroup`
    * `/srv/exportsXX/private`, usuario y grupo propietario `nobody:nogroup`, permisos 770
    ![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/1-1cupscarpetas.png)
    
* Vamos configurar el servidor NFS de la siguiente forma:
    * La carpeta `/srv/exportsXX/public`, será accesible desde toda la red en modo lectura/escritura.
    * La carpeta `/srv/exportsXX/private`, sea accesible sólo desde la IP del cliente, sólo en modo lectura.
* Para ello usaremos o Yast o modificamos el fichero `/etc/exports` añadiendo las siguientes líneas:

![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/1-2cupscarpetas.png)


> OJO: NO debe haber espacios entre la IP y abrir paréntesis.

* Para iniciar y parar el/los servicio/s de NFS, usaremos Yast o `systemctl`.
Si al iniciar el/los servicio/s aparecen mensajes de error o advertencias,
debemos resolverlas. Consultar los mensajes de error del servicio.

> [Enlace de interés](http://www.unixmen.com/setup-nfs-server-on-opensuse-42-1/)

* `showmount -e localhost`, muestra la lista de recursos exportados por el servidor NFS.

![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/7cupsconfig.png)

# 5. Cliente NFS

Ahora vamos a comprobar que las carpetas del servidor son accesibles desde el cliente.
Normalmente el software cliente NFS ya viene preinstalado pero si tuviéramos que instalarlo en
OpenSUSE:
* `zypper search nfs`, para buscar los paquetes nfs.
* `zypper install nfs-client`, para instalar el paquete cliente.
![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/7-1cupscliente.png)

## 5.1 Comprobar conectividad desde cliente al servidor

* `ping ip-del-servidor`: Comprobar la conectividad del cliente con el servidor. Si falla hay que revisar las configuraciones de red.
* `nmap ip-del-servidor -Pn`: nmap sirve para escanear equipos remotos, y averiguar que servicios están ofreciendo al exterior. Hay que instalar el paquete nmap, porque normalemente no viene preinstalado.
* `showmount -e ip-del-servidor`: Muestra la lista de recursos exportados por el servidor NFS.

![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/8cupscliente.png)

## 5.2 Montar y usar cada recurso compartido desde el cliente

* Estamos en el equipo cliente.
* Crear las carpetas:
    * `/mnt/remotoXX/public`
    * `/mnt/remotoXX/private`
* `showmount -e IP-DEL-SERVIDOR`, para consultar los recursos que exporta el servidor.
![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/9cupscliente.png)

* Montar cada recurso compartido en su directorio local correspondiente.

* `mount` o `df -hT`, para comprobar los recursos montados en nuestras carpetas locales.
![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/10cupscliente.png)
* `netstat -ntap`, para comprobar el acceso a los recursos NFS desde el cliente.
* Ahora vamos a crear ficheros/carpetas dentro del recurso public.
* Comprobar que el recurso private es de sólo lectura.

## 5.3 Montaje automático

> Acabamos de acceder a recursos remotos, realizando un montaje de forma manual (comandos mount/umount).
Si reiniciamos el equipo cliente, podremos ver que los montajes realizados de forma manual
ya no están. Si queremos volver a acceder a los recursos remotos debemos repetir el proceso,
a no ser que hagamos una configuración permanente o automática.

* Vamos a configurar el montaje autoḿatico del recurso compartido public. Para
ello podemos hacerlo de dos formas:
    * Usando `Yast -> particionador -> NFS -> Add`.
    * Modificar directamente en el fichero `/etc/fstab` ([Consultar](http://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-rg-es-4/s1-nfs-client-config.html))
    ![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/11cupsclienteauto.png)
    ![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/12cupsclienteauto.png)
    
* Incluir contenido del fichero `/etc/fstab` en la entrega.
![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/13cupsclienteauto.png)

* Reiniciar el equipo y comprobar que se monta el recurso remoto automáticamente.
* `mount` o `df -hT`

![linux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut2/NFS/images/linux/12cupsclienteauto.png)

