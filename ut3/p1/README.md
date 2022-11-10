# Servidor de Impresión en Windows

Ejemplo de rúbrica:

| Sección               | Muy bien (2) | Regular (1) | Poco adecuado (0) |
| --------------------- | ------------ | ----------- | ----------------- |
| (1.3) Comprobar que se imprime de forma local  | | | |
| (2.2) Comprobar que se imprime de forma remota | | | |
| (3.3) Comprobar que se imprime desde el navegador | | | |


Necesitaremos 2 MV:
* MV1: Windows Server 2019 (en mi caso vale cualquier server con rol de impresión)
* MV2: Windows 7/10 cliente

---

# 1. Impresora compartida

## 1.1 Rol impresión

* Vamos al servidor
* Instalar rol/función de servidor de impresión. Incluir impresión por Internet.

> DUDA: Instalar rol/función de cliente de impresión por Internet.

## 1.2 Instalar impresora PDF

Vamos a conectar e instalar localmente una impresora al servidor Windows Server, de modo que estén disponibles para ser accedidas por los clientes del dominio.

En nuestro caso, dado que es posible de que no tengan una impresora física en casa y no es de mucho interés forzar la instalación de una impresora que no se tiene, vamos a instalar un programa que simule una impresora de PDF.

PDFCreator es una utilidad completamente gratuita con la que podrás crear archivos PDF desde cualquier aplicación, desde el Bloc de notas hasta Word, Excel, etc. Este programa funciona simulando ser una impresora, de esta forma, instalando PDFCreator todas tus aplicaciones con opción para imprimir te permitirán crear archivos PDF en cuestión de segundos.

* Descargar PDFCreator (URL recomendada `www.pdfforge.org/pdfcreator/download`) e instalar.
* En PDFCreator, configurar en `perfiles -> Guardar -> Automático`. Ahí establecemos la carpeta destino.
En esta carpeta su guardarán los archivos que se impriman.

![configuracion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/2impresion.png)

> NOTA: PDFCreator puede requerir NET FrameWork v4. Esto tarda unos minutos.

## 1.3 Probar la impresora en local

Puedes probar la nueva impresora abriendo el Bloc de notas y creando un fichero luego selecciona imprimir. Cuando finalice el proceso se abrirá un fichero PDF con el resultado de la impresión.

* Probar la impresora local imprimiendo documento `imprimir25s-local`.

![configuracion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/3impresion.png)

> Dese a saber que yo en la primera prueba en vez de usar un dociumento de texto plano creado por mi utilize un archivo que habia por internet.

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/4impresion.png)

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/5impresion.png)

> Se ve que no hubo problemas.

# 2. Compartir por red

## 2.1 En el servidor

Vamos a la MV del servidor.
* Ir al `Administrador de Impresión -> Impresoras`
* Elegir impresora PDFCreator.
    * `Botón derecho -> Propiedades -> Compartir`
    * Como nombre del recurso compartido utilizar `PDFdavidquintero25`.
    
    ![server](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/6impresion.png)

## 2.2 Comprobar desde el cliente

Vamos al cliente:
* Buscar recursos de red del servidor. Si tarda en aparecer ponemos `\\172.19.25.60 o server25w` en la barra de navegación.

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/7impresioncliente.png)

* Seleccionar impresora -> botón derecho -> conectar.
    * Ponemos usuario/clave del Windows Server.
    
    ![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/8impresioncliente.png)
    
    ![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/9impresioncliente.png)
    > Le damos a instalar controlador.
    
* Ya tenemos la impresora remota configurada en el cliente.
* Probar la impresora remota imprimiendo documento `imprimir25w-remoto`. Si tenemos problemas para que aparezca el PDF en el servidor, iniciar el
programa PDFCreator y esperar un poco.

![cliente](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/10impresioncliente.png)

> **INFO**: Para **buscar archivos** (Por ejemplo PDF) dentro de Windows podemos usar las funcionar de [buscar](https://www.islabit.com/10080/una-mejor-forma-de-buscar-archivos-en-windows-7.html)

# 3. Acceso Web

Realizaremos una configuración para habilitar el acceso web a las impresoras del dominio.

## 3.1 Instalar característica impresión WEB

* Vamos al servidor.
* Nos aseguramos de tener instalado el servicio "Impresión de Internet".

![configuracion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/11impresioncliente.png)

## 3.2 Configurar impresión WEB

En este apartado vamos a volver a agregar la impresora de red remota en el cliente, pero
usando otra forma diferente de localizar la impresora.

Vamos a la MV cliente:
* Abrimos un navegador Web.
* Ponemos URL `http://<ip-del-servidor>/printers` (o `http://<nombre-del-servidor>/printers`)
para que aparezca en nuestro navegador un entorno que permite gestionar las impresoras de dicho equipo, previa autenticación como uno de los usuarios del habilitados para dicho fin (por ejemplo el "Administrador").

![configuracion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/12impresioncliente.png)

* Pincha en la opción propiedades y captura lo que se ve. Apuntar el URL asociado al nombre de red de la impresora para usarlo más adelante.

![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/13impresionclienteweb.png)

* Agregar nueva impresora (NO es local) en el SO cliente.
* Crear nueva impresora usando el URL (nombre de red) anterior.

![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/14impresionclienteweb.png)
![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/15impresionclienteweb.png)
![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/16impresionclienteweb.png)
![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/17impresionclienteweb.png)

## 3.3 Comprobar desde el navegador

> **PDF Creator Plus** (YERAY HERNÁNDEZ HERNÁNDEZ)
>
> * [Enlace a las funcioinalidades del PDF Creator Plus](https://www.pdfforge.org/pdfcreator/plus)
> * Comprobaremos que la versión Free tiene limitaciones.

Vamos a realizar seguidamente una prueba sencilla en tu impresora de red:
* Accede a la configuración de la impresora a través del navegador.
    * Poner en `pausa` los trabajos de impresión de la impresora.
* Ir a MV cliente.

![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/18impresionclienteweb.png)

* Probar la impresora remota imprimiendo documento `imprimir25w-web`.
    * Comprobar que al estar la impresora en pausa, el trabajo aparece en cola de impresión del servidor.

![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/18impresionclienteweb.png)

> * Misma captura porque a fin de cuentas se demuestra los mismo.
 
* Finalmente pulsa en reanudar el trabajo para que tu documento se convierta a PDF.

![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/19impresionclienteweb.png)
![web](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut3/p1/images/linux/20impresionclienteweb.png)

> En la ultima imagen se bugueo un poco pero se puede ver en el architec que el archivo se paso a pdf.
> Tambien podemos ver como en el pdfcreator se puede identificar que el archivo se paso sin problemas.

* Si tenemos problemas para que aparezca el PDF en el servidor, iniciar el
programa PDFCreator y esperar un poco.

> **Si hay problemas** para acceder a la impresora de red desde el cliente Windows entonces:
>
> * Revisar la configuración de red de la máquina (Incluido la puerta de enlace)
> * Reiniciar el servidor Windows Server que contiene la impresora compartida de red.
