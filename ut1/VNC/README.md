## Windows

## 1. Windows: Slave VNC
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/fe8e263f2582e623cec7948d2bcae25873837f5a/1trimestre/u1/vnc/images/Windows/1.png)

***1.2 Ir a una máquina con GNU/Linux***
![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/fe8e263f2582e623cec7948d2bcae25873837f5a/1trimestre/u1/vnc/images/Windows/1-2.png)

## 2 Windows: Master VNC

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/c4235b8d6b900b743e4520a51efc64a49ad622a2/1trimestre/u1/vnc/images/Windows/2-1-1.png)
    
    Conección desde Windows Master hacia el Windows Slave.(Normalmente te debería aparecer una configuración de TightVCN pero como yo lo habia hecho y estaba siguiendo la rubrica no pude sacar la captura pero seria: TightVNC -> Custom -> Viewer)

***2.1 Comprobaciones finales***

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/c4235b8d6b900b743e4520a51efc64a49ad622a2/1trimestre/u1/vnc/images/Windows/2-1-1-1.png)
Introducimos la informacion de nuestro servidor para la conección.

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/c4235b8d6b900b743e4520a51efc64a49ad622a2/1trimestre/u1/vnc/images/Windows/2-1-1-2.png)
Comprobamos que la conexión se ha realizado de manera correcta y tenemos control sobre nuestro servidor desde la máquina cliente.

![](https://github.com/DAVIDQR22/add21-22-david-quintero/blob/c4235b8d6b900b743e4520a51efc64a49ad622a2/1trimestre/u1/vnc/images/Windows/2-1-4.png)
Usando en VNC server el comando netstat -n para ver las conexiones VNC con el cliente.

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
