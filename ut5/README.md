# Vagrant con VirtualBox

## Introducción

Según *Wikipedia*:

* Vagrant es una herramienta para la creación
y configuración de entornos de desarrollo virtualizados.
* Originalmente se desarrolló para VirtualBox y para sistemas de configuración
tales como Chef, Salt y Puppet. Sin embargo desde la versión 1.1 Vagrant es capaz de trabajar con múltiples proveedores, como VMware, Amazon EC2, LXC, DigitalOcean, etc.2
* Aunque Vagrant se ha desarrollado en Ruby se puede usar en multitud de
proyectos escritos en otros lenguajes.

> NOTA: Para desarrollar esta actividad se ha utilizado principalmente
la información del enlace anterior publicado por Jonathan Wiesel, el 16/07/2013.

## Instalar Vagrant

> Enlaces de interés:
> * [Introducción a Vagrant](https://code.tutsplus.com/es/tutorials/introduction-to-vagrant--cms-25917)
> * [Cómo instalar y configurar Vagrant](http://codehero.co/como-instalar-y-configurar-vagrant/)
> * [Instalar vagrant en OpenSUSE 13.2](http://gattaca.es/post/running-vagrant-on-opensuse/)
> * [Descargar y actualizar vagrant](https://www.vagrantup.com/downloads.html)

* Instalar Vagrant. La instalación vamos a hacerla en una máquina real.
* Hay que comprobar que las versiones de Vagrant y VirtualBox son compatibles entre sí.
    * `vagrant version`, para comprobar la versión actual de Vagrant.
    * `VBoxManage -v`, para comprobar la versión actual de VirtualBox.
* Crear el alias `va='vagrant'`.

# 1. Proyecto: Añadir cajas

## 1.1 Imagen, caja o box

Existen muchos repositorios desde donde podemos descargar la cajas de Vagrant (Imágenes o boxes). Incluso podemos descargarnos cajas de otros sistemas oprativos desde [VagrantCloud Box List](https://app.vagrantup.com/boxes/search?provider=virtualbox)

> OJO: Sustituir **BOXNAME** por `ubuntu/bionic64`

* `vagrant box add BOXNAME`, descargar la caja que necesitamos a través de vagrant.
* `vagrant box list`, lista las cajas/imágenes disponibles actualmente en nuestra máquina.

![caja](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant1-1.png)

```
> vagrant box list
BOXNAME (virtualbox, 0)
```

## 1.2 Directorio

* Crear un directorio para nuestro proyecto. Donde XX es el número de cada alumno:

```
mkdir nombre-alumnoXX-va1box.d
cd nombre-alumnoXX-va1box.d
```

A partir de ahora vamos a trabajar dentro de esta carpeta.
* Crear un fichero nuevo llamado `Vagrantfile` con el siguiente contenido:

![directorio](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant1-2.png)

> INFO: Si quisiéramos partir de un fichero Vagrantfile más complejo, podemos usar el comando `vagrant init` para crear un fichero de ejemplo.

## 1.3 Comprobar

Vamos a crear una MV nueva y la vamos a iniciar usando Vagrant:
* Debemos estar dentro de `nombre-alumnoXX-va1box.d`.
* `vagrant up`, para iniciar una nueva instancia de la máquina.

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant1-3.png)

* `vagrant ssh`: Conectar/entrar en nuestra máquina virtual usando SSH.

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant1-3-1.png)

> DUDAS:
> * ¿Podría funcionar si tenemos el fichero Vagrantfile en un disco duro externo o pendrive e intentamos levantar la máquina virtual con Vagrant?
> * ¿Tiene algo que ver que sea un almacenamiento externo o es por el sistema de ficheros?
> * ¿Qué requisito tiene vagrant con respecto al sistema de ficheros sobre el que va a trabajar?

## 1.4 Eliminamos la MV

* `exit` para salir fuera de la MV.

![eliminar](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant1-4.png)

* Ahora estamos en la máquina real.
* `vagrant halt`, para apagar la máquina virtual.
* `vagrant status`, consultar el estado actual de la máquina virtual.
* `vagrant destroy`, para eliminar la máquina virtual (No los ficheros de configuración).

![eliminar](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant1-4-1.png)

* > En mi caso no la borre en ese momento pero lo hice despues.

> **Otros comandos últiles de Vagrant son**:
> * `vagrant suspend`: Suspender la máquina virtual. Tener en cuenta que la MV en modo **suspendido** consume más espacio en disco debido a que el estado de la máquina virtual que suele almacenarse en la RAM se pasa a disco.
> * `vagrant resume` : Volver a despertar la máquina virtual.

# 2. Proyecto: Redirección de puertos

Ahora vamos a hacer otro proyecto añadiendo redirección de puertos.

## 2.1 Creamos los ficheros

En la máquina real:
* Crear carpeta `nombre-alumnoXX-va2port.d`.
* Entrar en el directorio.

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-1.png)

* Configurar Vagrantfile para usar nuestra caja BOXNAME y hostname = "nombre-alumnoXX-vagrant3".
* Modificar el fichero `Vagrantfile`, de modo que el puerto 42XX del sistema anfitrión sea enrutado al puerto 80 del ambiente virtualizado.
  * `config.vm.network :forwarded_port, host: 42XX, guest: 80`, sustituir XX por el número de cada uno. Por ejemplo el PC 1 será 01.

Incluir en el fichero `Vagrantfile` las configuraciones necesarias para:
* La MV de VirtualBox debe tener el nombre `vagrantXX-va2port`.
* La memoria RAM de la MV en VirtualBox debe ser de 2048 MiB.

![vagrantfile](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-1-1.png)

Levantar la MV podemos hace `vagrant up` pero también `time vagrant up` para medir el tiempo que se tarda en levantar la MV. El añadir el comando `time COMMAND` delante de un comando nos calcula el tiempo que tarda en ejecutarse dicho comandos/programa (COMMAND).

![up](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-1-2.png)

## 2.2 Entramos en la MV

* Entramos en la MV en la máquina virtual (`vagrant ssh`).
* Instalamos Apache2.

![apache](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-2.png)

> NOTA: Cuando la MV está iniciada y si el fichero de configuración ha cambiado y queremos recargarlo (que se vuelva a leer) hacemos `vagrant reload`.

## 2.3 Comprobar

Para confirmar que hay un servicio a la escucha en 4567:
* Ir a la máquina real.
* `vagrant port` para ver la redirección de puertos de la máquina Vagrant.

![port](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-3.png)

* Abrir el navegador web con el URL `http://127.0.0.1:42XX`. En realidad estamos accediendo al puerto 80 de nuestro sistema virtualizado.

![apache](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-3-1.png)

* `nmap -Pn localhost`

![nmap](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-3-2.png)

## 2.4 Eliminar la MV

(Ya sabes cómo hacerlo)

> Si levantamos 2 MV con Vagrant...
> * ¿Pueden verse entre ellas? (ping)
> * En caso negativo ¿cómo podemos configurar la red para que se vean?

![eliminar](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant2-4.png)

# 3. Proyecto: Suministro mediante shell script

Una de los mejores aspectos de Vagrant es el uso de herramientas de suministro. Esto es, ejecutar *"una receta"* o una serie de scripts durante el proceso de arranque del entorno virtual para instalar, configurar y personalizar un sin fin de aspectos del SO del sistema anfitrión.

## 3.1 Crear ficheros

Ahora vamos a suministrar a la MV un pequeño script para instalar Apache.

Ejemplo:
```
david42-va3script.d
├── html
│   └── index.html
├── install_apache.sh
└── Vagrantfile
```

* Crear directorio `nombre-alumnoXX-va3script.d` para nuestro proyecto.
* Entrar en dicha carpeta.

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-1.png)
* Pequeña correcion sobra uno de los nanos que despues se corrigio.

* Crear la carpeta `html` y crear fichero `html/index.html` con el siguiente contenido:

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-1-1.png)

* Crear el script `install_apache.sh`, dentro del proyecto con el siguiente
contenido:

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-1-2.png)

## 3.2 Vagrantfile

Incluir en el fichero de configuración `Vagrantfile` lo siguiente:
* `config.vm.hostname = "nombre-alumnoXX-va3script"`
* `config.vm.provision :shell, :path => "install_apache.sh"`, para indicar a Vagrant que debe ejecutar el script `install_apache.sh` dentro del entorno virtual.
* `config.vm.synced_folder "html", "/var/www/html"`, para sincronizar la carpeta exterior `html` con la carpeta interior. De esta forma el fichero "index.html" será visible dentro de la MV.

Incluir en el fichero `Vagrantfile` las configuraciones necesarias para:
* La MV de VirtualBox debe tener el nombre `vagrantXX-va3script`.
* La memoria RAM de la MV en VirtualBox debe ser de 2048 MiB.

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-2.png)

## 3.3 Comprobamos

* `vagrant up`, para crear la MV. IMPORTATE: Capturar imagen de todos los comandos y su salida completa.

![up](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-2-1.png)
![up](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-2-2.png)

* Al iniciar la máquina, aparecen mensajes de salida que muestran información sobre cómo se va instalando el paquete de Apache que indicamos.

![up](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-3.png)

* Para verificar que efectivamente el servidor Apache ha sido instalado e iniciado, abrimos navegador en la máquina real con URL `http://127.0.0.1:42XX`.

![up](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant3-3-1.png)

> NOTA: Podemos usar `vagrant reload`, si la MV está en ejecución, para que coja los cambios de configuración sin necesidad de reiniciar.

# 4. Proyecto: Suministro mediante Puppet

## 4.1 Preparativos

Se pide hacer lo siguiente.
* Crear directorio `nombre-alumnoXX-va4puppet.d` como nuevo proyecto Vagrant.

![preparativo](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-1.png)

* Modificar el archivo `Vagrantfile` de la siguiente forma:

```
Vagrant.configure("2") do |config|
  ...
  config.vm.hostname = "nombre-alumnoXX-va4puppet"
  ...
  # Nos aseguramos de tener Puppet en la MV antes de usarlo.
  config.vm.provision "shell", inline: "sudo apt-get update && sudo apt-get install -y puppet"

  # Hacemos aprovisionamiento con Puppet
  config.vm.provision "puppet" do |puppet|
    puppet.manifest_file = "nombre-del-alumnoXX.pp"
  end
 end
```
![preparativo](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-2.png)

> Cuando usamos `config.vm.provision "shell", inline: '"echo "Hola"'`, se ejecuta directamente el comando especificado en la MV. Es lo que llamaremos provisión inline.

* Crear la carpeta `manifests`. OJO: un error muy típico es olvidarnos de la "s" final.
* Crear el fichero `manifests/nombre-del-alumnoXX.pp`, con las órdenes/instrucciones Puppet necesarias para instalar el software que elijamos. Ejemplo:

![preparacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-1-2.png)

> NOTA:
> * El Puppet es un gestor de infraestructura que veremos en profundidad otra actividad.
> * Podemos hacer el suministro con otros gestores de infraestructura como Salt-stack. Consultar enlace  [Salt Provisioner](https://www.vagrantup.com/docs/provisioning/salt.html).

## 4.2 Vagrantfile

Incluir en el fichero `Vagrantfile` las configuraciones necesarias para:
* La MV de VirtualBox debe tener el nombre `vagrantXX-va4puppet`.
* La memoria RAM de la MV en VirtualBox debe ser de 2048 MiB.

![preparativo](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-2.png)

## 4.3 Comprobamos

Para que se apliquen los cambios de configuración tenemos 2 caminos:
* **Con la MV encendida**
    1. `vagrant reload`, recargar la configuración.
    2. `vagrant provision`, volver a ejecutar la provisión.

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-3.png)

* **Con la MV apagada**:
    1. `vagrant destroy`, destruir la MV.
    
![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-3-1.png)
    2. `vagrant up` volver a crearla.

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-3-2.png)

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-3-3.png)

Ir a la MV:
* Comprobar que el software está instalado.

![comprobacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant4-3-4.png)

# 5. Proyecto: Caja personalizada

En los apartados anteriores hemos descargado una caja/box de un repositorio de Internet, y la hemos personalizado. En este apartado vamos a crear nuestra propia caja/box a partir de una MV de VirtualBox que tengamos.

## 5.1 Preparar la MV VirtualBox

> Enlace de interés:
> * Indicaciones de [¿Cómo crear una Base Box en Vagrant a partir de una máquina virtual](http://www.dbigcloud.com/virtualizacion/146-como-crear-un-vase-box-en-vagrant-a-partir-de-una-maquina-virtual.html) para preparar la MV de VirtualBox.

### Elegir una máquina virtual

Lo primero que tenemos que hacer es preparar nuestra máquina virtual con una determinada configuración para poder publicar nuestro Box.

* Crear una MV VirtualBox nueva o usar una que ya tengamos.
* Configurar la red en modo automático o dinámico (DHCP).
* Instalar OpenSSH Server en la MV.

![zypper](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-1.png)

### Crear usuario con acceso SSH

Vamos a crear el usuario `vagrant`. Esto lo hacemos para poder acceder a la máquina virtual por SSH desde fuera con este usuario. Y luego, a este usuario le agregamos una clave pública para autorizar el acceso sin clave desde Vagrant. Veamos cómo:

* Ir a la MV de VirtualBox.
* Crear el usuario `vagrant`en la MV.
    * `su`
    * `useradd -m vagrant`

![user](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-1-1.png)

* Poner clave "vagrant" al usuario vagrant.
* Poner clave "vagrant" al usuario root.
* Configuramos acceso por clave pública al usuario `vagrant`:
    * `mkdir -pm 700 /home/vagrant/.ssh`, creamos la carpeta de configuración SSH.
    * `wget --no-check-certificate 'https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub' -O /home/vagrant/.ssh/authorized_keys`, descargamos la clave pública.
    * `chmod 0600 /home/vagrant/.ssh/authorized_keys`, modificamos los permisos de la carpeta.
    * `chown -R vagrant /home/vagrant/.ssh`, modificamos el propietario de la carpeta.

![user](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-1-2.png)

> NOTA:
> * Podemos cambiar los parámetros de configuración del acceso SSH. Mira la teoría...
> * Ejecuta `vagrant ssh-config`, para averiguar donde está la llave privada para cada máquina.

## Sudoers

Tenemos que conceder permisos al usuario `vagrant` para que pueda hacer tareas privilegiadas como configurar la red, instalar software, montar carpetas compartidas, etc. Para ello debemos configurar el fichero `/etc/sudoers` (Podemos usar el comando `visudo`) para que no nos solicite la password de root, cuando realicemos estas operaciones con el usuario `vagrant`.

* Añadir `vagrant ALL=(ALL) NOPASSWD: ALL` al fichero de configuración `/etc/sudoers`. Comprobar que no existe una linea indicando requiretty si existe la comentamos.

![sudoers](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-1-3.png)

**Añadir las VirtualBox Guest Additions**

* Debemos asegurarnos que tenemos instalado las VirtualBox Guest Additions con una versión compatible con el host anfitrión. Comprobamos:
```
root@hostname:~# modinfo vboxguest |grep version
version:        6.0.24
```
* Apagamos la MV.

## 5.2 Crear caja Vagrant

Una vez hemos preparado la máquina virtual ya podemos crear el box.

* Vamos a crear una nueva carpeta `nombre-alumnoXX-va5custom.d`, para este nuevo proyecto vagrant.

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-2.png)

* `VBoxManage list vms`, comando de VirtualBox que muestra los nombres de nuestras MVs. Elegir una de las máquinas (VMNAME).
* Nos aseguramos que la MV de VirtualBox VMNAME está apagada.

![vboxmanage](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-2-1.png)

* `vagrant package --base VMNAME --output nombre-alumnoXX.box`, parar crear nuestra propia caja.
* Comprobamos que se ha creado el fichero `nombre-alumnoXX.box` en el directorio donde hemos ejecutado el comando.

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-2-2.png)

* `vagrant box add nombre-alumno/va5custom nombre-alumnoXX.box`, añadimos la nueva caja creada por nosotros, al repositorio local de cajas vagrant de nuestra máquina.

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-2-3.png)

* Consultar ahora la lista de cajas Vagrant disponibles.

![creacion](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-2-4.png)

## 5.3 Vagrantfile

* Crear un nuevo fichero Vagrantfile para usar nuestra caja.

![vagrantfile](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-3.png)

* Incluir en el fichero `Vagrantfile` las configuraciones necesarias para:
    * La MV de VirtualBox debe tener el nombre `vagrantXX-nombre-alumno`.
    * La memoria RAM de la MV en VirtualBox debe ser de 2048 MiB.

![vagrantfile](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-3-1.png)

## 5.4 Usar la nueva caja

* Levantamos una nueva MV a partir del Vagranfile.

![caja](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-3-2.png)

* Nos debemos conectar sin problemas (`vagant ssh`).

![ssh](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant5-3-3.png)

# 6. Caja Windows

* El punto 6 no me salio porque me daba un par de errores

## 6.1 Windows con vagrant

> OJO: Puede ser que nos haga falta instalar algún plugin de Vagrant:
> * `vagrant plugin install winrm`
> *  y puede ser que también haga falta winrm-elevated.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant6-1.png)

* Crear una MV Windows usando vagrant.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant6-1-2.png)



![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant6-1-2-1.png)



![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant6-1-3.png)

* Comprobar que funciona.

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant6-1-4.png)

![windows](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/images/vagrant6-1-5.png)

 

## 6.2 Limpiar

Cuando terminemos la práctica, ya no nos harán falta las cajas (boxes) que tenemos cargadas en nuestro repositorio local. Por tanto, podemos borrarlas para liberar espacio en disco:

* `vagrant box list`, para consultar las cajas disponibles.
* `vagrant box remove BOXNAME`, para eliminar una caja BOXNAME de nuestro repositorio local.
