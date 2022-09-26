David Quintero Reeve
# Clientes Ligeros LTSP en Ubuntu

___

## Definición

  Es una computadora cliente o un software de cliente en una arquitectura de red cliente-servidor que depende primariamente del servidor central para las tareas de procesamiento, y se enfoca principalmente en transportar la entrada y la salida entre el usuario y el servidor remoto.

  - Video de funcionamiento

      https://youtu.be/rq606gmQ6GU

___

# 1. Preparación de máquinas

Crear las siguientes máquinas:

  - Servidor LTSP

      - SO: Ubuntu desktop
      - Almacenamiento: 25GB
      - Red 1: adaptador puente
      - Red 2: red interna

  - Cliente 1

    - SO: imagen creada por servidor LTSP
    - Almacenamieno: sin disco
    - Red: red inerna
  - Cliente 2

    - SO: imagen creada por servidor LTSP
    - Almacenamieno: sin disco
    - Red: red inerna
___

# 2. Servidor LTSP Ubuntu

Lo primero que haremos sera instalar Ubuntu en nuestra máquina virtual ya configurada.

## 2.1 Configuración de las redes

  Según acabe la instalación y ya estemos dentro nos tocará configurar las conexiones a la red.

  - Red 1 (adaptador puente)

    - IP: 172.19.25.41
    - Mascara: 255.255.0.0
    - Puerta de enlace: 172.19.0.1
    - DNS: 8.8.4.4
    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/3-1.png)


  - Red 2 (red interna)

    - IP: 192.168.67.1
    - Mascara: 255.255.255.0
    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/3-1-1.png)

## 2.2 Configuración de host.

  Cambiaremos el nombre de host para tenerlo a nuestro gusto, en este caso para la asginatura.

  - Primero modificaremos el archivo hosts que se encuentra en la ruta `/etc/hosts`.

    - Pondremos: `quintero25d quintero25d.curso2223`

  - Luego modificaremos el archivo hostname que estara en la ruta `/etc/hostname`.

    - Pondremos: `quintero25d`

## 2.3 Comandos de comprobación

  - ip a
  - route -n
  - hostname -a
  - hostname -f
  - uname -a
  ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/3-2.png)


## 2.4 Creación de usuarios

Ahora creamos 3 usuarios `quintero1,quintero2,quintero3` usando el comando `sudo useradd -m usuarionuevo`.

![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/5.PNG)

## 2.5 Instalación de servicio LTSP

  - Primero instalaremos el servicio ssh para la conexión remota `apt-get install openssh-server`

    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/3-2ssh.png)

### Cambiar la configuración de SSH

* Entrar en la consola con el usuario `root`.
* Editar el fichero `nano /etc/ssh/sshd_config`:
     * Quitar y/o comentar la línea `PermitRootLogin without-password`.
     * Dejar la siguiente configuración `PermitRootLogin yes`. SIN ALMOHADILLA.
     Las almohadillas `#` al comienzo de la línea la desactivan/deshabilitan/la comentan.
   ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/3-2-1ssh.png)

* `systemctl restart sshd`, iniciar el servicio. Antes se hacía con `service ssh restart`.
* `systemctl enable sshd`, para asegurarnos de que se va a iniciar el servicio cuando se encienda la máquina.
* `systemctl status sshd`, comprobamos.

    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/3-2-2ssh.png)

  - Ahora pasaremos a instalar el servidor LTSP con los comandos (antes de eso ejecutar el comando: add-apt-repository ppa:ltsp, si no nos dejara ejecutar los comandos de abajo).
    * "apt install --install-recommends ltsp ipxe dnsmasq nfs-kernel-server"
    * "ltsp dnsmasq --proxy-dhcp=0"
    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/2.PNG)
    
  - Además vamos a instalar más herramientas/paquetes
    * "apt install openssh-server squashfs-tools ethtool net-tools epoptes"
    * "gpasswd -a administrator epoptes"
    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/3.PNG)

## Configuración del DHCP

- Ahora pasaremos a configurar el DHCP del servidor LTSP. Existen dos formas:
    
    * ltsp dnsmasq: uno es evitar cualquier configuración; esto generalmente significa que tiene una sola NIC en el servidor LTSP y un servidor DHCP externo, por ejemplo, un enrutador, pfsense o un servidor de Windows.

    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/4.PNG)
    
    * ltsp dnsmasq --proxy-dhcp=0: Otro método es tener un servidor LTSP de NIC dual, donde un NIC está conectado a la red normal donde está Internet y el otro NIC está conectado a un conmutador separado con solo los clientes LTSP.

## 3.5 Configuraciones

**Creamos una imagen del SO** se cargará en la memoria de los clientes ligeros cuando se inicien.
* `time ltsp image`, para crear una imagen de 64 bits del SO. Usamos `time` sólo para medir el tiempo que va a tardar esta operación. Hay que tener paciencia en este punto. Tarda 15 minutos o más.
* Ejecutar `ltsp info`, para consultar información.

![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/6.PNG)
![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/6-1.PNG)

**iPXE**: Después de crear la imagen inicial ejecutar el comando siguiente para generar el menú iPXE y copiar los archivos binarios iPXE en TFTP: `ltsp ipxe`

![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/7.PNG)

**NFS Server**: Para configurar el servidor LTSP para servir imágenes a través de NFS ejecuta: `ltsp nfs`

![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/8.PNG)

**Generar ltsp.img**: `ltsp initrd`

![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/9.PNG)

# 4. Preparar MV Cliente

* Crear la MV cliente1 en VirtualBox:
    * Sin disco duro y sin unidad de DVD.
    * Sólo tiene RAM, floppy
    * Tarjeta de red PXE en modo "red interna".
    * Configurar memoria gráfica a 128MB y habilitar el soporte 3D.
    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/cliente10.PNG)
    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/cliente11.PNG)
    ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/cliente12.PNG)

* Con el servidor encendido, iniciar la MV cliente1 desde red/PXE:
    * Comprobar que todo funciona correctamente.
    * Si la tarjeta de red no inicia correctamente el protocolo PXE,
    conectar disquete Etherboot en la disquetera, tal y como se indica
    en la documentación de la web de LTSP.
    
     ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/cliente13.PNG)
     
     - Comprobamos que el cliente esta pillando la configuración del servidor.
     
     ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/cliente14.PNG)
     
     - Vemos que nos esta detectando bien la máquina cliente desde el servidor.
     
     ![host](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut1/LTPS/images/cliente15.PNG)
     
     - Probamos a entrar como un usuario y vemos que funciona bien.
     - Con todo esto ya deberiamos haber terminado la configuración de nuestra maquina servidora LTSP.
