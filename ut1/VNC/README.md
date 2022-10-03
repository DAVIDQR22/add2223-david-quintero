## Windows

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

*   Ahora hacemos la Conección desde Windows Master hacia el Windows Slave.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente6.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente7.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/cliente8.png)

- Vemos que hace la conexión sin problemas

***2.1 Comprobaciones finales***

Comprobaciones para verificar que se han establecido las conexiones remotas:

* Conectar desde GNU/Linix Master hacia GNU/Linux Slave.
** Si tenemos problemas, cerrar la sesión en la máquina Slave, antes de iniciar la sesión desde la máquina Master.

* Ejecutar como superusuario lsof -i -nP en el servidor para comprobar las conexiones VNC.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente10.png)

* Ejecutar vncserver -list en el servidor

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/linux/linuxcliente11.png)

Usando en VNC server el comando netstat -n para ver las conexiones VNC con el cliente.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/VNC/windows/server4.png)

## Linux

## OpenSUSE: Slave VNC
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/c4235b8d6b900b743e4520a51efc64a49ad622a2/1trimestre/u1/vnc/images/Linux/3.png)
Probamos la conexion vnc desde nuestras maquinas linux

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/c4235b8d6b900b743e4520a51efc64a49ad622a2/1trimestre/u1/vnc/images/Linux/1.png)
Ademas de la ejecución como superusuario lsof -i -nP en el servidor para comprobar las conexiones VNC.

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/c4235b8d6b900b743e4520a51efc64a49ad622a2/1trimestre/u1/vnc/images/Linux/2.png)
Ejecutar vncserver -list en el servidor.

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/3yast.png)

Ir a Yast -> VNC
   
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/3yast-1.png)

    Permitir conexión remota.
    Abrir puertos VNC en el cortafuegos.
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/3yast-2.png)

Ir a Yast -> Cortafuegos

    Revisar la configuración del cortafuegos.
    
    Debe estar permitido las conexiones a vnc-server.
 
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/3yast-3.png)

Con nuestro usuario normal

    Ejecutar vncserver en el servidor para iniciar el servicio VNC.
    Ponemos claves para las conexiones VNC a nuestro escritorio.
    Al final se nos muestra el número de nuestro escritorio remoto. Apuntar este número porque lo usaremos más adelante.
    
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/3yast-4.png)

    Ejecutar ps -ef|grep vnc para comprobar que los servicios relacionados con vnc están en ejecución.

## OpenSUSE: Master VNC
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/4.png)

    Hacemos una conexión remota con vncviewer desde nuestro cliente.
    Ponemos lo siguiente para la conexión vncviewer IP-vnc-server:590N(siendo N el numero final del puerto de la conexión remota).

**Comprobaciones finales**
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/5.png)

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/5-1.png)
    
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/5-2.png)

    Las tres imagenes de arriba muestran el intento de una conexion desde GNU/Linix Master hacia GNU/Linux Slave. 
 
 ## Comprobaciones con SSOO cruzados
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/5-3.png)
    
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/5-3-1.png)

    Conexión al cliente Windows con el servidor VNC GNU/Linux.
    
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/5-3-2.png)

    Conexión al cliente Windows con el servidor VNC GNU/Linux.
    
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/5fa79e648f5a63d1a5e13bfaea93befe47c3fcd9/1trimestre/u1/vnc/images/Linux/5-4.png)

    Ejecutar en el servidor GNU/Linux lsof -i -nP.
