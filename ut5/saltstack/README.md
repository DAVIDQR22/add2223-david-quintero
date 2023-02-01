# 1. Salt-stack

Hay varias herramientas conocidas del tipo gestor de infrastructura como Puppet, Chef, Ansible y Terraform. En esta actividad vamos a practicar Salt-stack con OpenSUSE.

## 1.1 Preparativos
---
| Config   | MV1           | MV2          | MV3          |
| -------- | ------------- | ------------ | ------------ |
| Hostname | masterXXg     | minionXXg    | minionXXw    |
| SO       | OpenSUSE      | OpenSUSE     | Windows      |
| IP       | 172.19.25.31  | 172.19.25.32 | 172.19.25.11 |
---
> "25" es el número asignado a cada alumno.

* **OJO**: Configurar correctamente los nombres de los equipos antes de continuar
con la práctica. Comprobar salida de los comandos `hostname`, `hostname -a`,
`hostname -f` y `hostname -d`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster1.png)

# 2. Master: Instalar y configurar

* Ir a la MV1
* `zypper install salt-master`, instalar el software del Máster.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster2.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster2-1.png)

> ATENCIÓN: El fichero de configuración siguiente tiene formato YAML.
>
> 1. Los valores de clave(key) principal no tienen espacios por delante.
> 2. El resto de valores de clave(key) secundarios tendrán 2 espacios o 4 espacios por delante.
>
> Hay que cumplir estas restricciones para que el contenido del dichero sea válido.

* Modificar `/etc/salt/master` para configurar nuestro Máster con:

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster2-2.png)

* `systemctl enable salt-master.service`, activiar servicio en el arranque del sistema.
* `systemctl start salt-master.service`, iniciar servicio.
* `salt-key -L`, para consultar Minions aceptados por nuestro Máster. Vemos que no hay ninguno todavía.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster2-3.png)

# 3. Minion

Los Minios son los equipos que van a estar bajo el control del Máster.

## 3.1 Instalación y configuración

* `zypper install salt-minion`, instalar el software del agente (minion).

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-1-1.png)


* Modificar `/etc/salt/minion` para definir quien será nuestro Máster:

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-1-2.png)

* `systemctl enable salt-minion.service`, activar Minion en el arranque del sistema.
* `systemctl start salt-minion.service`, iniciar el servico del Minion.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-1-3.png)

* Comprobar que  que no tenemos instalado `apache2` en el Minion.

## 3.2 Cortafuegos

Hay que asegurarse de que el cortafuegos permite las conexiones al servicio Salt. Consultar URL [Opening the Firewall up for Salt](https://docs.saltstack.com/en/latest/topics/tutorials/firewall.html)

* Ir a la MV1 Máster.
* `firewall-cmd --get-active-zones`, consultar la zona de red. El resultado será public, dmz o algún otro. Sólo debe aplicar a las zonas necesarias.
* `firewall-cmd --zone=public --add-port=4505-4506/tcp --permanent`, abrir puerto de forma permanente en la zona "public".
* `firewall-cmd --reload`, reiniciar el firewall para que los cambios surtan efecto.
(También vale con `systemctl firewalld reload`)

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-2.png)

* `firewall-cmd --zone=public --list-all`, para consultar la configuración actual la zona `public`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-2-1.png)

## 3.3 Aceptación desde el Master

Ir a MV1:
* `salt-key -L`, vemos que el Máster recibe petición del Minion.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-3.png)

* `salt-key -a minionXXg`, para que el Máster acepte a dicho Minion.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-3-1.png)

## 3.4 Comprobamos conectividad

Desde el Máster comprobamos:
1. Conectividad hacia los Minions.
```
# salt '*' test.ping
minionXXg:
    True
```
2. Versión de Salt instalada en los Minions
```
# salt '*' test.version
minionXXg:
    3000
```

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion3-4.png)

> El símbolo `'*'` representa a todos los minions aceptados. Se puede especificar un minion o conjunto de minios concretos.

# 4. Salt States

> Enlaces de interés:
> * [Learning SaltStack - top.sls (1 of 2)](https://www.youtube.com/watch?v=UOzmExyAXOM&t=8s)
> * [Learning SaltStack - top.sls (2 of 2)](https://www.youtube.com/watch?v=1KblVBuHP2k)

## 4.1 Preparar el directorio para los estados

Vamos a crear directorios para guardar lo estados de Salt. Los estados de Salt son definiciones de cómo queremos que estén nuestras máquinas.

Ir a la MV Máster:
* Crear el directorio `/srv/salt/base`(para guardar nuestros estados), y el directorio `/srv/salt/devel` (para desarrollo o para hacer pruebas).

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-1.png)

* Crear archivo `/etc/salt/master.d/roots.conf` con el siguiente contenido:
```
file_roots:
  base:
    - /srv/salt/base
  devel:
    - /srv/salt/devel
```

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-1-1.png)

* Reiniciar el servicio del Máster.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-1-2.png)

## 4.2 Crear un nuevo estado

Los estados de Salt se definen en ficheros SLS.
* Crear fichero `/srv/salt/base/apache/init.sls`:
```
install_apache:
  pkg.installed:
    - pkgs:
      - apache2

apache_service:
  service.running:
    - name: apache2
    - enable: True
    - require:
      - install_apache
```

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-2.png)

Entendamos las definiciones:
* Nuestro nuevo estado se llama `apache` porque el directorio donde están las definiciones se llama `srv/salt/base/apache`.
* La primera línea es un identificador (Por ejemplo: `install_apache` o `apache_service`), y es un texto que ponemos nosotros libremente, de forma que nos ayude a identificar lo que vamos a definir.
* `pkg.installed`: Es una orden de salt que asegura que los paquetes estén instalados.
* `service.running`: Es una orden salt que asegura de que los servicios estén iniciados o parados.

## 4.3 Asociar Minions a estados

Ir al Máster:
* Crear `/srv/salt/base/top.sls`, donde asociamos a todos los Minions con el estado que acabamos de definir.
```
base:       
  '*':
    - apache
```

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-3.png)

## 4.4 Comprobar: estados definidos

* `salt '*' state.show_states`, consultar los estados que tenemos definidos para cada Minion:
```
minionXXg:
    - apache
```

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-4.png)

## 4.5 Aplicar el nuevo estado

Ir al Master:
* Consultar los estados en detalle y verificar que no hay errores en las definiciones.
    * `salt '*' state.show_lowstate`

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-5.png)

    * `salt '*' state.show_highstate`,
    
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-5-1.png)

* `salt '*' state.apply apache`, para aplicar el nuevo estado en todos los minions. OJO: Esta acción puede tardar un tiempo.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-5-2.png)

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion4-5-3.png)

> NOTA: Con este comando `salt '*' state.highstate`, también se pueden invocar todos los estados.

# 5. Crear más estados

## 5.1 Crear estado "users"

Vamos a crear un estado llamado `users` que nos servirá para crear un grupo y usuarios en las máquinas Minions (ver ejemplos en el ANEXO).

* Crear directorio `/srv/salt/base/users`.
* Crear fichero `/srv/salt/base/users/init.sls` con las definiciones para crear los siguiente:
    * Grupo `mazingerz`
    * Usuarios `kojiXX`, `drinfiernoXX` dentro de dicho grupo.
    
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1.png)

* No olvidarse de meter nuestra nuevo estado en el archivo top.sls

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-1.png)

* `salt '*' state.show_states`, consultar los estados que tenemos definidos para cada Minion:

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-2.png)

* Consultar los estados en detalle y verificar que no hay errores en las definiciones.
    * `salt '*' state.show_lowstate`

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-3.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-3.png)

    * `salt '*' state.show_highstate`,
    
 ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-4.png) 
 ![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-5.png)
  
* Aplicar el estado.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-6.png)

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion5-1-8.png)

## 5.2 Crear estado "dirs"

> Para el estado dirs haremos los mismo que lo anterirores lo unico que cambia es el archivo init.sls.

* Crear estado `dirs` para crear las carpetas `private` (700), `public` (755) y `group` (750) en el HOME del usuario `koji` (ver ejemplos en el ANEXO).

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster5-2-1.png)

* Aplicar el estado `dirs`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster5-2-2.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster5-2-3.png)

## 5.3 Ampliar estado "apache"

* Crear el fichero `/srv/salt/base/files/holamundo.html`. Escribir dentro el nombre del alumno y la fecha actual.
* Incluir en el estado "apache" la creación del fichero "holamundo" en el Minion. Dicho fichero se descargará desde el servidor Salt Máster y se copiará en el Minion.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster5-3.png)

* Ir al master y aplicar el estado "apache".

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster5-3-1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackmaster5-3-2.png)

# 6. Añadir Minion de otro SO

## 6.1 Minion con Windows

* Crear MV3 con SO Windows (minionXXw)
* Instalar `salt-minion` en MV3.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion6-1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion6-1-1.png)
![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion6-1-2.png)

* El instalador nos da la opción de iniciar el servicio del minion. Pero también podemos iniciarlo desde una consola como administrador ejecutando `sc start salt-minion`.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion6-1-3.png)

* Ir a MV1(Máster) y aceptar al minion.

![](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut5/saltstack/images/saltstackminion6-1-4.png)

## 6.2 Aplicar estado

* Crear un estado para el Minion de Windows únicamente.
* Aplicar estado al Minion de Windows.




