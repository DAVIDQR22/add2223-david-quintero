# Servidor SSH
## Configuraciones de las maquinas
### 1. Configuración servidor Linux.
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinux1.png)
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinux2.png)
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinux3.png)
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinux4.png)

 - Aqui tenemos la configuración que yo utilize para mi servidor linux.
 
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinux5.png)

 - Comprobamos los cambios que hemos echo en la configuracion de nuestro servidor.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinux6.png)

 - Creamos los usuarios que se ven en la imagen.

### 1.2. Configuración cliente Linux.
![ClienteLinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientelinux1.png)
  
- Añadir en /etc/hosts los equipos server25g, y client25w.

![ClienteLinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientelinux2.png)

- Comprobar haciendo ping a ambos equipos.

### 1.3. Configuración cliente Windows.
![Clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientewindows1.png)
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxping7.png)

- Añadir en C:\Windows\System32\drivers\etc\hosts los equipos serverXXg y clientXXg.
- Comprobar haciendo ping a ambos equipos.

### 2. Instalación del servicio SSH en GNU/Linux
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxssh.png)

- Comprobamos si tenenemos instalado openssh en nuestro servidor. 
- Si no lo tenemos intalado lo instalamos

### 2.1 Comprobación
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxcomprobacion1.png)

  - Desde el propio servidor, verificar que el servicio está en ejecución.
  - systemctl status sshd, esta es la forma habitual de comprobar los servicios.
  - ps -ef|grep sshd, esta es otra forma de comprobarlo mirando los procesos del sistema.
  - sudo lsof -i:22 -Pn, comprobar que el servicio está escuchando por el puerto 22.

### 2.2 Primera conexión SSH desde cliente GNU/Linux
![ClienteLinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientelinux3.png)
  
 - ping server25g, comprobar la conectividad con el servidor.
  nmap -Pn serverXXg, comprobar los puertos abiertos en el servidor (SSH debe estar open).
  
![ClienteLinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientelinuxconexionserver4.png)
  
 - Desde el cliente GNU/Linux nos conectamos mediante ssh 1er-apellido-alumno1@serverXXg.

### 2.3 Primera conexión SSH desde cliente Windows
![ClienteWindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientewindowsconexionserver1.png)
 
 - Desde el cliente Windows (usando PuTTY) nos conectamos al servidor SSH de GNU/Linux.

![ClienteWindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientewindowsconexionserver2.png)

 - Podremos ver el intercambio de claves que se produce en el primer proceso de conexión SSH. * ¿Te suena la clave que aparece? Es la    clave de identificación de la máquina del servidor.

![ClienteWindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientewindowsconexionserver3.png)
![ClienteWindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/clientewindowsconexionserver3-1.png)

 - La siguiente vez que volvamos a usar PuTTY ya no debe aparecer el mensaje de advertencia porque hemos memorizado la identificación del servidor SSH. Comprobarlo.

### 3. Cambiamos la identidad del servidor
- Los ficheros ssh_host*key y ssh_host*key.pub, son ficheros de clave pública/privada que identifican a nuestro servidor frente a nuestros clientes. Confirmar que existen el en /etc/ssh,:

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidad8.png)

- Modificar el fichero de configuración SSH (/etc/ssh/sshd_config) para dejar una única línea: HostKey /etc/ssh/ssh_host_rsa_key. Comentar el resto de líneas con configuración HostKey. Este parámetro define los ficheros de clave publica/privada que van a identificar a nuestro servidor. Con este cambio decimos que sólo se van a utilizar las claves del tipo RSA.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidad9.png)

### 3.1 Regenerar certificados

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidadcertificado10.png)

 - Como usuario root ejecutamos: ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key. ¡OJO! No poner password al certificado.
 
 ![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidad9-1.png)
 
 - Reiniciar el servicio SSH: systemctl restart sshd.
 - Comprobar que el servicio está en ejecución correctamente: systemctl status sshd

### 3.2 Comprobamos
#### Windows
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidadcertificado11-2.png)

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidadcertificado11-3.png)

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidadcertificado11-4.png)

 - Comprobar qué sucede al volver a conectarnos desde los dos clientes, usando los usuarios 1er-apellido-alumno2 y 1er-apellido-alumno1
 - Podemos comprobar que en windows nos dara un aviso diciendonos que nuestra conexion a este usuario podria ser peligroso debido a un cambio repentino en la información.
 
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/16ConexionWindows2.PNG)
  
 - Lo demas nos conectamos normalmente.
  
![](https://github.com/DAVIDQR22/add2122-david-quintero/blob/main/1trimestre/u1/ssh/images/17ConexionWindows2.PNG)

 - Vemos que en usuario 2 no pasa nada ya que en este no nos habiamos metido todavía.

#### Linux
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidadcertificado11.png)

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidadcertificado11-1.png)

 - Nos da un aviso de que se acaba de cambiar aolgo en el usuario quintero1 y que deberiamos tener cuidado.
 - Tambien comprobamos que el usuario quintero2 no dejara la conexión devido a que el servidor podría estar bajo un ataque.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxindetidadcertificado11-5.png)

 - Para solucionar el problema leemos los mensajes y ejecutamos el comando de la imagen.

### 4. Personalización del prompt Bash
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxprompt13.png)

 - Añadimos en nuestro servidor las lineas para cambiar el promt de nuestro usuario quintero1

!![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxprompt12.png)

 - Ademas creamos el fichero .alias si no lo teniamos creado en la ruta que pone en la imagen y añadimos esas lineas al fichero.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxprompt14.png)

### 5. Autenticación mediante claves públicas
#### Linux
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxautenticacion16.png)

 - ssh-keygen -t rsa para generar un nuevo par de claves para el usuario

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxautenticacion17.png)

 - Ahora vamos a copiar la clave pública (id_rsa.pub), al fichero "authorized_keys" del usuario remoto 1er-apellido-alumno4 que está definido en el servidor.

 - Comprobamos que desde el usuario quintero4 de linux nos deja la conexión cliente linux 

#### Windows
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxautenticacion18.png)

 - Comprobamos que desde el usuario quintero4 no nos deja la conexión cliente windows 

### 6. Uso de SSH como túnel para X
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxtunelssh19.png)

 - Instalar en el servidor una aplicación de entorno gráfico (APP1) que no esté en los clientes. Por ejemplo Geany. Si estuviera en el cliente entonces buscar otra aplicación o desinstalarla en el cliente.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxtunelssh20.png)

- Modificar servidor SSH para permitir la ejecución de aplicaciones gráficas, desde los clientes. Consultar fichero de configuración /etc/ssh/sshd_config (Opción X11Forwarding yes)

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxtunelssh21.png)

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxtunelssh22.png)
 - Vamos a comprobar desde clientXXg, que funciona APP1(del servidor).
 - ssh -X primer-apellido-alumno1@serverXXg, nos conectamos de forma remota al servidor, y ahora ejecutamos APP1 de forma remota.

### 7. Aplicaciones Windows nativas

`No hay captura de la instlaciòn pero se da por hecho que se instaló wine con "zypper install wine" y que se utilizó el notepad que está instalado por defecto como aplicación gráfica para la prueba de esta parte.` 

- Instalar emulador Wine en el server25g.
- Ahora podríamos instalar alguna aplicación de Windows en el servidor SSH usando el emulador Wine. O podemos usar el Block de Notas que viene con Wine: wine notepad.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxnativa24.png)

- Comprobar el funcionamiento del programa en server25g.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxnativa23.png)

- Comprobar funcionamiento del programa, accediendo desde client25g.

- `Aviso tuve un problema con las imagenes y asi que posiblemente lo cambie en la versión del siguiente commit`

### 8. Restricciones de uso

#### 8.1 Restricción sobre un usuario


 - Comprobamos que X11Forwarding esta en modo yes.



 - Consultar/modificar fichero de configuración del servidor SSH (/etc/ssh/sshd_config) para restringir el acceso a determinados usuarios. Consultar las opciones AllowUsers, DenyUsers.



 - /usr/sbin/sshd -t; echo $?, comprobar si la sintaxis del fichero de configuración del servicio SSH es correcta (Respuesta 0 => OK, 1 => ERROR).



 - Comprobarlo la restricción al acceder desde los clientes. (me falto el cliente de windows pero si funciono en Linux por logica tambien funcionaria en Windows).

#### 8.2 Restricción sobre una aplicación


 - En la imagen pone el usuario 1 pero despues esto es cambiado mas adelante por el usuario 4(esta parte tampoco la hize porque no acabe de entenderla).

### 9. Servidor SSH en Windows


 - Comprobamos la configuración de nuestro servidor Windows y vemos si este tiene conexión con las otras maquinas.




 - Configuración del fichero hosts de nuestro servidor.
 


 - Hacemos ping a travez de nuestro cliente windows al servidor

### 9.1 Instalacion Openshh

 
 - Descargamos la el comprimido de OpenSSH y lo descomprimimos en C:\archivos de programa\ 



 - cd ‘C:\Program files\OpenSSH’, Iniciar PowerShell como Administrador y movernos hasta C:\Program files\OpenSSH:
 - Ejecutar el script para instalar los servicios “sshd” y “ssh-agent”
 - Set-ExecutionPolicy –ExecutionPolicy Bypass
 - .\install-sshd.ps1



 - Al terminar debe indicar que los servicios se han instalado de forma satisfactoria. Podemos comprobar que se han instalado los servicios con el siguiente comando: PS> Get-Service sshd,ssh-agent


 - Yo cambie aquí el orden porque si no me daba error asi que en vez de iniciar al final el servicio, lo inicie antes de crear las llaves
 - .\ssh-keygen.exe –A
 - .\FixHostFilePermissions.ps1 -Confirm:$false

### 9.1.1 Regla Firewall


 - Creamos una regla para permitir la conexion por Openssh.

### 9.1.2


 - Abrimos el “Editor del Registro” de Windows (regedit.exe).
 - Nos desplazamos hasta la clave Equipo\HKEY_LOCAL_MACHINE\SOFTWARE.
 - Creamos la clave “OpenSSH”.
 - Dentro de la clave “OpenSSH”, creamos un “Valor de cadena” (REG_SZ) con el nombre “DefaultShell” y cuyo valor es la ruta al
 - ejecutable de PowerShell: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe”

### 9.2 Comprobaciones

##### Linux


 - Finalmente comprobamos en Linux si la configuración funciono y tenemos conexión.

##### Windows


 - Finalmente comprobamos en Windows si la configuración funciono y tenemos conexión.


