# Servidor SSH
## Configuraciones de las maquinas
### 1. Configuración servidor Linux.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/1servidorssh.PNG)

 - Aqui tenemos la configuración que yo utilize para mi servidor linux ademas de algunas comprobaciones.
  
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/2servidorssh.PNG)

 - Comando lsblk para consultar las particiones y el comando blkid para consultar los UUID de la instalación

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/3servidorssh.PNG)

 - Creamos los usuarios que se ven en la imagen (PD: yo ya los tenia creados).

### 1.2. Configuración cliente Linux.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/4clienteGNU.PNG)
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/5clienteGNU.PNG)
  
 - Configuración del equipo cliente Linux

### 1.3. Configuración cliente Windows.
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/6clienteWindows.PNG)

 - Configuración del equipo cliente Windows

### 2. Instalación del servicio SSH en GNU/Linux
### 2.1 Comprobación
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/7clienteWindows.PNG)

  - Desde el propio servidor, verificar que el servicio está en ejecución.
  - systemctl status sshd, esta es la forma habitual de comprobar los servicios.
  - ps -ef|grep sshd, esta es otra forma de comprobarlo mirando los procesos del sistema.
  - sudo lsof -i:22 -Pn, comprobar que el servicio está escuchando por el puerto 22.

### 2.2 Primera conexión SSH desde cliente GNU/Linux
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/8primeraconexionlinuxcliente.PNG)
  
 - ping serverXXg, comprobar la conectividad con el servidor.
  nmap -Pn serverXXg, comprobar los puertos abiertos en el servidor (SSH debe estar open).
  
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/9primeraconexionlinuxcliente.PNG)
  
 - Desde el cliente GNU/Linux nos conectamos mediante ssh 1er-apellido-alumno1@serverXXg.

### 2.3 Primera conexión SSH desde cliente Windows
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/10primeraConexionWindowsCliente.PNG)
 
 - Desde el cliente Windows (usando PuTTY) nos conectamos al servidor SSH de GNU/Linux.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/11primeraConexionWindowsCliente.PNG)

 - Podremos ver el intercambio de claves que se produce en el primer proceso de conexión SSH. * ¿Te suena la clave que aparece? Es la    clave de identificación de la máquina del servidor.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/12primeraConexionWindowsCliente.PNG)

 - La siguiente vez que volvamos a usar PuTTY ya no debe aparecer el mensaje de advertencia porque hemos memorizado la identificación del servidor SSH. Comprobarlo.

### 3. Cambiamos la identidad del servidor
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/13identidadServer.PNG)

### 3.1 Regenerar certificados

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/14serverCertificados.PNG)

 - Como usuario root ejecutamos: ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key. ¡OJO! No poner password al certificado.
 - Reiniciar el servicio SSH: systemctl restart sshd.
 - Comprobar que el servicio está en ejecución correctamente: systemctl status sshd

### 3.2 Comprobamos
#### Windows
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/15ConexionWindows2.PNG)

 - Comprobar qué sucede al volver a conectarnos desde los dos clientes, usando los usuarios 1er-apellido-alumno2 y 1er-apellido-alumno1
 - Podemos comprobar que en windows nos dara un aviso diciendonos que nuestra conexion a este usuario podria ser peligroso debido a un cambio repentino en la información.
 
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/16ConexionWindows2.PNG)
  
 - Lo demas nos conectamos normalmente.
  
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/17ConexionWindows2.PNG)

 - Vemos que en usuario 2 no pasa nada ya que en este no nos habiamos metido todavía.

#### Linux
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/18ConexionLinux2.PNG)

 - Nos da un aviso de que se acaba de cambiar aolgo en el usuario quintero1 y que deberiamos tener cuidado.
 - Tambien comprobamos que el usuario quintero2 no dejara la conexión devido a que el servidor podría estar bajo un ataque.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/19errorLinux2.PNG)

 - Para solucionar el problema leemos los mensajes y ejecutamos el comando de la imagen.

### 4. Personalización del prompt Bash
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/23personalizacionbash.png)

 - Añadimos en nuestro servidor las lineas para cambiar el promt de nuestro usuario quintero1

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/24personalizacionbash.png)

 - Ademas creamos el fichero .alias si no lo teniamos creado en la ruta que pone en la imagen y añadimos esas lineas al fichero.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/25conexionlinuxbash.png)

### 5. Autenticación mediante claves públicas
#### Linux
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/20Linuxautenticacion.PNG)

 - ssh-keygen -t rsa para generar un nuevo par de claves para el usuario

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/26clavespublicasLinux.png)

 - Ahora vamos a copiar la clave pública (id_rsa.pub), al fichero "authorized_keys" del usuario remoto 1er-apellido-alumno4 que está definido en el servidor.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/27clavespublicasLinux.png)

 - Comprobamos que desde el usuario quintero4 de linux nos deja la conexión cliente linux 

#### Windows
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/27clavespublicasWindows.png)

 - Comprobamos que desde el usuario quintero4 no nos deja la conexión cliente windows 

### 6. Uso de SSH como túnel para X
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/29tunelSSH.png)

 - Instalar en el servidor una aplicación de entorno gráfico (APP1) que no esté en los clientes. Por ejemplo Geany. Si estuviera en el cliente entonces buscar otra aplicación o desinstalarla en el cliente.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/30tunelSSH.png)

- Modificar servidor SSH para permitir la ejecución de aplicaciones gráficas, desde los clientes. Consultar fichero de configuración /etc/ssh/sshd_config (Opción X11Forwarding yes)

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/31tunelSSH.png)

 - Vamos a comprobar desde clientXXg, que funciona APP1(del servidor).
 - ssh -X primer-apellido-alumno1@serverXXg, nos conectamos de forma remota al servidor, y ahora ejecutamos APP1 de forma remota.

### 7. Aplicaciones Windows nativas

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/32WineSSH.png)
 - Pude instalar la aplicacion pero a la hora de intentar hacerla me daba un error y al final lo dejé a parte.

### 8. Restricciones de uso

#### 8.1 Restricción sobre un usuario
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/33Retricciones.png)

 - Comprobamos que X11Forwarding esta en modo yes.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/34Retricciones.png)

 - Consultar/modificar fichero de configuración del servidor SSH (/etc/ssh/sshd_config) para restringir el acceso a determinados usuarios. Consultar las opciones AllowUsers, DenyUsers.

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/35Retricciones.png)

 - /usr/sbin/sshd -t; echo $?, comprobar si la sintaxis del fichero de configuración del servicio SSH es correcta (Respuesta 0 => OK, 1 => ERROR).

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/36Retricciones.png)

 - Comprobarlo la restricción al acceder desde los clientes. (me falto el cliente de windows pero si funciono en Linux por logica tambien funcionaria en Windows).

#### 8.2 Restricción sobre una aplicación
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/37RetriccionesApp.png)

 - En la imagen pone el usuario 1 pero despues esto es cambiado mas adelante por el usuario 4(esta parte tampoco la hize porque no acabe de entenderla).

### 9. Servidor SSH en Windows
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/38ServerwindowsLinux.png)

 - Comprobamos la configuración de nuestro servidor Windows y vemos si este tiene conexión con las otras maquinas.


![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/39ServerwindowsWindows.png)

 - Configuración del fichero hosts de nuestro servidor.
 
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/40ServerwindowsWindows.png)

 - Hacemos ping a travez de nuestro cliente windows al servidor

### 9.1 Instalacion Openshh
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/41sshwindowsserver.png)
 
 - Descargamos la el comprimido de OpenSSH y lo descomprimimos en C:\archivos de programa\ 

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/42sshwindowsserver.png)

 - cd ‘C:\Program files\OpenSSH’, Iniciar PowerShell como Administrador y movernos hasta C:\Program files\OpenSSH:
 - Ejecutar el script para instalar los servicios “sshd” y “ssh-agent”
 - Set-ExecutionPolicy –ExecutionPolicy Bypass
 - .\install-sshd.ps1

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/43sshwindowsserver.png)

 - Al terminar debe indicar que los servicios se han instalado de forma satisfactoria. Podemos comprobar que se han instalado los servicios con el siguiente comando: PS> Get-Service sshd,ssh-agent

![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/44sshwindowsserver.png)
 - Yo cambie aquí el orden porque si no me daba error asi que en vez de iniciar al final el servicio, lo inicie antes de crear las llaves
 - .\ssh-keygen.exe –A
 - .\FixHostFilePermissions.ps1 -Confirm:$false

### 9.1.1 Regla Firewall
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/45firewalReglawindowsserver.png)

 - Creamos una regla para permitir la conexion por Openssh.

### 9.1.2
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/45shellwindowsserver.png)

 - Abrimos el “Editor del Registro” de Windows (regedit.exe).
 - Nos desplazamos hasta la clave Equipo\HKEY_LOCAL_MACHINE\SOFTWARE.
 - Creamos la clave “OpenSSH”.
 - Dentro de la clave “OpenSSH”, creamos un “Valor de cadena” (REG_SZ) con el nombre “DefaultShell” y cuyo valor es la ruta al
 - ejecutable de PowerShell: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe”

### 9.2 Comprobaciones

##### Linux
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/46ComprobacionLinux.png)

 - Finalmente comprobamos en Linux si la configuración funciono y tenemos conexión.

##### Windows
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/46ComprobacionWindows.png)

 - Finalmente comprobamos en Windows si la configuración funciono y tenemos conexión.


