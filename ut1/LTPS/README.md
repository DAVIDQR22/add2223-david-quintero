David Quintero Reeve
# Clientes Ligeros LTSP en Ubuntu

___

## Definición

  Es una computadora cliente o un software de cliente en una arquitectura de red cliente-servidor que depende primariamente del servidor central para las tareas de procesamiento, y se enfoca principalmente en transportar la entrada y la salida entre el usuario y el servidor remoto.

  - Video de funcionamiento

      - No se ha podido crear el video por fallos técnicos.

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


  - Red 2 (red interna)

    - IP: 192.168.67.1
    - Mascara: 255.255.255.0

## 2.2 Configuración de host.

  Cambiaremos el nombre de host para tenerlo a nuestro gusto, en este caso para la asginatura.

  - Primero modificaremos el archivo hosts que se encuentra en la ruta `/etc/hosts`.

    - Pondremos: `quintero25d quintero25d.curso2223`

      ![hosts]()

  - Luego modificaremos el archivo hostname que estara en la ruta `/etc/hostname`.

    - Pondremos: `quintero25d`

      ![hosts]()

## 2.3 Comandos de comprobación

  - ip a
  - route -n
  - hostname -a
  - hostname -f
  - uname -a


## 2.4 Creación de usuarios

Ahora creamos 3 usuarios `quintero1,quintero2,quintero3` usando el comando `sudo useradd -m usuarionuevo`.

![hosts]()

## 2.5 Instalación de servicio LTSP

  - Primero instalaremos el servicio ssh para la conexión remota `apt-get install openssh-server`

    ![hosts]()

  - Ahora pasaremos a instalar el servidor LSTP con el comando `apt install --install-recommends ltsp ipxe dnsmasq nfs-kernel-server
ltsp dnsmasq --proxy-dhcp=0` en Ubuntu

    ![hosts]()

  - Además vamos a instalar más herramientas/paquetes:
apt install openssh-server squashfs-tools ethtool net-tools epoptes
gpasswd -a davidquintero epoptes

    ![hosts]()

      Esto puede tardar un rato dependiendo del sistema operativo al que se le haga la imágen.



## 
