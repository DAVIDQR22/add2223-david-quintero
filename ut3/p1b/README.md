# Servidor de Impresión GNU/Linux (CUPS)

# 1. Preparativos

Enlace de interés:
* [Vídeo LPIC-1 102 Printing using CUPS](https://youtu.be/6M4oGNn9cVc)

# 2. Servidor de Impresión

Instalación:
* Instalar el sistema de impresión CUPS para GNU/Linux.

![instalacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/1cupsserver.png)

* `systemctl status ...`, verificar que el servicio está en ejecución.

![status](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2cupsserver.png)

Configuración:
* Configurar CUPS `/etc/cups/cupsd.conf` (Ver vídeo). `Allow @LOCAL` significa que vamos a permitir el acceso al servicio vía Web del servidor a todas las máquinas de la red local (LAN).

![configuracion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2-1cupsserver.png)

* Reiniciar el servicio para que coja los cambios de configuración. Lo podemos hacer con un "reload".

![instalacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2-2cupsserver.png)

Cortafuegos:
* Abrir en el cortafuegos el acceso al servicio de impresión `ipp`. En el cortafuegos hay varias zonas, para saber la que tenemos activa hacemos `firewall-cmd --get-default-zone`. Seguramente la zona por defecto será `public` pero hay que comprobarlo.

![instalacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2-3cupsserver.png)

![instalacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2-4cupsserver.png)

Panel web de CUPS:
* A continuación, conectar a la interfaz web de CUPS.

![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2-5cupsserver.png)

* Acceder a la sección de `Administración` con el **usuario/clave de root**. Desde ahí acceder a la sección `Ver archivo de registro de accesos`. Esto sólo es para comprobar que podemos acceder correctamente vía interfaz web.

![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2-6cupsserver.png)

![archivo](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/2-7cupsserver.png)

# 3. Imprimir de forma local

Ahora vamos a usar una impresora de forma local en el servidor de impresión.

* Instalar el paquete `cups-pdf` que nos permite hacer uso de una impresora virtual PDF local. Usaremos esta impresora virtual para las pruebas en caso de no disponer de una impresora real.

![local](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/3cupsserver.png)

* La impresora debe estar configurada como impresora por defecto.

![local](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/3-4cupsserver.png)

* Crear una archivo "impresionXXlocal.txt" con algún contenido.
* Imprimir el documento en la impresora local.

![local](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/3-5cupsserver.png)

* Comprobar el resultado. Los trabajos de impresión de la impresora virtual PDF se guardan en alguno de estos directorios:

```
/home/usuario/PDF
/var/spool/cups-pdf/anonymous
```
![local](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/3-5-1cupsserver.png)

* Ver contenido del PDF.

![local](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/3-5-2cupsserver.png)

# 4. Imprimir de forma remota

> NOTA: Aunque lo indiquen los apuntes, para nuestras pruebas,
NO es necesario crear el fichero `/etc/cups/client.conf`
en la máquina cliente.

Ir al servidor.
* Habilitamos la impresora como recurso de red compartido.

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4cupsserver.png)

> Es importante que el cliente tenga una IP definida en la cláusula Allow del servidor. Esto lo hicimos cuando especificamos "Allow @LOCAL".

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4-1cupsserver.png)

Ir a un cliente.
* Abrir en el cortafuegos el acceso al servicio de impresión `ipp-client`. En el cortafuegos hay varias zonas, para saber la que tenemos activa hacemos `firewall-cmd --get-default-zone` (Seguramente la zona por defecto será `public`).

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4-2cupsserver.png)

* Iniciar la herramienta "configuración de impresión". Desbloqueamos con la clave de root.

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4-3cupsserver.png)

* Agregar impresora de red. Primero la buscamos en IP del servidor y nos debe aparecer automáticamente (Por ejemplo `ipp://ip-server:631/printers/CUPS-PDF`).

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4-4cupsserver.png)

* Crear una archivo "impresionXXremota.txt" con algún contenido.
* Imprimir el documento en la impresora remota.

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4-5cupsserver.png)

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4-6cupsserver.png)

Ir al servidor.
* Comprobamos que se ha realizado la impresión remota.
* Ver contenido del PDF.

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1b/images/4-7cupsserver.png)
