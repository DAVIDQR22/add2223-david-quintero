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


### 8. Restricciones de uso

#### 8.1 Restricción sobre un usuario


 - Comprobamos que X11Forwarding esta en modo yes.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones25.png)
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones26.png)

 - Consultar/modificar fichero de configuración del servidor SSH (/etc/ssh/sshd_config) para restringir el acceso a determinados usuarios. Consultar las opciones AllowUsers, DenyUsers.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones27.png)

 - /usr/sbin/sshd -t; echo $?, comprobar si la sintaxis del fichero de configuración del servicio SSH es correcta (Respuesta 0 => OK, 1 => ERROR).

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones29.png)

 - Comprobarlo la restricción al acceder desde los clientes.

#### 8.2 Restricción sobre una aplicación


- Crear grupo remoteapps

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones28.png)

- Incluir al usuario 1er-apellido-alumno4 en el grupo remoteapps.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones28-1.png)

- Localizar un programa (Por ejemplo "geany"). Posiblemente tenga permisos 755.
- Poner al programa el grupo propietario "remoteapps".
- Poner los permisos del ejecutable del programa a 750. Para impedir que los usuarios que no pertenezcan al grupo puedan   ejecutar el programa.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones30.png)

- Comprobamos el funcionamiento en el servidor en local.

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones30-1.png)

- Comprobamos el funcionamiento desde el cliente en remoto (Recordar ssh -X ...).

![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones30-2.png)
![serverlinux](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverlinuxrestricciones30-3.png)

### 9. Servidor SSH en Windows


 - Comprobamos la configuración de nuestro servidor Windows y vemos si este tiene conexión con las otras maquinas.

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows1.png)

 - Configuración del fichero hosts de nuestro servidor.
 
![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows2.png)

### 9.1 Instalacion Openshh

 
 - Descargamos el comprimido de OpenSSH y lo descomprimimos en C:\archivos de programa (86)\ 

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows3.png)

 - cd ‘C:\Program files\OpenSSH’, Iniciar PowerShell como Administrador y movernos hasta C:\Program files\OpenSSH:
 - Ejecutar el script para instalar los servicios “sshd” y “ssh-agent”
 - Set-ExecutionPolicy –ExecutionPolicy Bypass
 - .\install-sshd.ps1

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows4.png)

 - Al terminar debe indicar que los servicios se han instalado de forma satisfactoria. Podemos comprobar que se han instalado los servicios con el siguiente comando: PS> Get-Service sshd,ssh-agent

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows5.png)

- Generar las claves (certificados) del servido
 - .\ssh-keygen.exe –A
 - .\FixHostFilePermissions.ps1 -Confirm:$false

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows6.png)

### 9.1.1 Regla Firewall

- Habilitar la regla de nombre “SSH” en el Firewall de Windows para permitir (Allow) conexiones TCP entrantes (Inbound) en el puerto 22 (SSH): PS> New-NetFirewallRule -Protocol TCP -LocalPort 22 -Direction Inbound -Action Allow -DisplayName SSH

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows7.png)

- Creamos una regla para permitir la conexion por Openssh.

- Configuramos los servicios para que inicien automáticamente

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows8.png)

### 9.2 Comprobaciones

- Comprobar acceso SSH desde los clientes Windows y GNU/Linux al servidor SSH Windows.

##### Linux
- lsof -i -nP en GNU/Linux.

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows9.png)



##### Windows
- netstat -n en Windows.

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows10.png)

- Servidor Windows

![clientewindows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/SSH/images/serverWindows11.png)

- Cliente Windows





