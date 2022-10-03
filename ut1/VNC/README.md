# Windows

## 1. Windows: slave VNC

### Instalación del servidor VNC
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente2.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/server1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente4.png)

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/server2.png)

*  `Importante no olvidarse la contraseña será usada más adelante.`

***1.2 Ir a una máquina con GNU/Linux***

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/server3.png)

*  Ejecutamos el comando nmpa -Pn ipserver para comprobar que los servicios son visibles desde fuera de la máquina VNC-SERVER.

## 2 Windows: Master VNC

### Instalación del cliente VNC

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente2.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente3.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente4.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente5.png)


***2.1 Comprobaciones finales***

Conectar desde Window Master hacia el Windows Slave.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente6.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente7.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente8.png)

Ir al servidor VNC y usar el comando netstat -n para ver las conexiones VNC con el cliente.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/server4.png)

# Linux

## 3 OpenSUSE: Slave VNC

* Antes de empezar a hacer nada Ir a `Yast -> VNC`
    * Permitir conexión remota. Esto configura el servicio `xinet`.
    * Abrir puertos VNC en el cortafuegos
    
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linux0.png)

* `Yast -> VNC`
    * Permitir conexión remota. Esto configura el servicio `xinet`.
    * Abrir puertos VNC en el cortafuegos

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linux1.png)

* Ahora ejecutamos el comando vncserver
   * Ponemos claves para las conexiones VNC a nuestro escritorio.
   * Al final se nos muestra el número de nuestro escritorio remoto. 
   `Apuntar este número porque lo usaremos más adelante.`

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linux2.png)
  
* vdir /home/nombrealumno/.vnc, vemos que se nos han creado unos ficheros de configuración VNC asociados a nuestro usuario.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linux3.png)

* Ejecutar ps -ef|grep vnc para comprobar que los servicios relacionados con vnc están en ejecución.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linux4.png)

* Ejecutar lsof -i -nP para comprobar que están los servicios en los puertos VNC (580X y 590X).

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linux5.png)

***3.1 Ir a una máquina GNU/Linux***

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente6.png)

## 4 OpenSUSE: Master VNC

* `vncviewer` es un cliente VNC que viene con OpenSUSE.
* En la conexión remota, hay que especificar `IP:5901`, `IP:5902`, etc.
(Usar el número del escritorio remoto obtenido anteriormente).

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente7.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente8.png)

## 4.1 Comprobaciones finales

Comprobaciones para verificar que se han establecido las conexiones remotas:
* Conectar desde GNU/Linix Master hacia GNU/Linux Slave.
    * Si tenemos problemas, cerrar la sesión en la máquina Slave,
    antes de iniciar la sesión desde la máquina Master.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente9.png)    

* Ejecutar como superusuario `lsof -i -nP` en el servidor para comprobar las conexiones VNC.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente10.png)

* Ejecutar `vncserver -list` en el servidor.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente11.png)

 
 # 5 Comprobaciones con SSOO cruzados

* Conectar el cliente GNU/Linux con el Servidor VNC Windows.
Usaremos el comando `vncviewer IP-vnc-server` sin especificar puerto alguno.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcruzado1.png)

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcruzado2.png)


* Ejecutar `netstat -n` en el servidor Windows.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcruzado3.png)


* Conectar el cliente Windows con el servidor VNC GNU/Linux.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/windowscruzado1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/windowscruzado2.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/windowscruzado3.png)

* Ejecutar en el servidor GNU/Linux `lsof -i -nP`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/windowscruzado4.png)
