# Servicio de Directorio con comandos

# 1. Prerequisitos

> Enlaces de interés:
>
> * https://doc.opensuse.org/documentation/leap/archive/15.3/security/html/book-security/cha-security-ldap.html
> * [Configuración básica 389-DS](https://www.javieranto.com/kb/GNU-Linux/pr%C3%A1cticas/Administraci%C3%B3n%20b%C3%A1sica%20389DS/)
> * https://directory.fedoraproject.org/docs/389ds/howto/quickstart.html

## 1.1 Nombre de equipo FQDN

* Vamos a usar una MV OpenSUSE para montar nuestro servidor LDAP ([Configuración MV](../../global/configuracion/opensuse.md)).
* Revisar `/etc/hostname`. Nuestra máquina debe tener un FQDN=`serverXXg.curso2021`.
* Revisar `/etc/hosts`

```
127.0.0.2   serverXXg.curso2021   serverXXg
```

* Comprobar salida de: `hostname -a`, `hostname -d` y `hostname -f`.

![hosts](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/1ldap.png)

# 2. Instalar el Servidor LDAP

Hay varias herramientas que implementan el servidor de directorios LDAP (389-DS, OpenLDAP, Active Directory, etc). Según parece [Red Hat y Suse retiran su apoyo a OpenLDAP2](https://www.ostechnix.com/redhat-and-suse-announced-to-withdraw-support-for-openldap/), por este motivo, hemos decido a partir de noviembre de 2018 cambiar OpenLDAP2 por 389-DS.

En esta guía vamos a instalar y configurar del servidor LDAP con 389-DS usando comandos.

## 2.1 Instalación del paquete

* Abrir una consola como root.
* `zypper in 389-ds`, instalar el software LDAP.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2ldap.png)

* `rpm -qa | grep 389-ds`, comprobar que la versión es >= 1.4.*

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-1ldap.png)

## 2.2 Configurar la instancia

* Crear el fichero `/root/instance.inf` con el siguiente contenido. Este fichero sirve para configurar el servidor:


* `dscreate from-file /root/instance.inf`, creamos una nueva instancia.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-2ldap.png)

* `dsctl localhost status`, comprobar el estado actual de la instancia de la base de datos LDAP

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-2-1ldap.png)

> NOTA: Si queremos eliminar una instancia de LDAP que ya tenemos creada haremos lo siguiente:
> * `dsctl -l`, muestra los nombres de todas las instancias.
> * `dsctl localhost stop`, para parar la instancia.
> * `dsctl localhost remove --do-it`,para eliminar la instancia.

* Creamos el fichero `/root/.dsrc` con el siguiente contenido. Este fichero sirve para configurar los permisos para acceder a la base de datos como administrador:

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-2-2ldap.png)

> NOTA:
>
> * Cada vez que aparece ldapXX, hay que cambiar XX por el identificador de cada alumno.
> * Recordar el nombre y clave de nuestro usuario administrador del servidor de directorios LDAP.
> * Los ficheros de configuración de nuestro servicio/instancia los tenemos en `/etc/dirsrv/slapd-localhost`
> * El fichero de configuración `/etc/dirsrv/slapd-localhost/dse.ldif` contiene los parámetros principales del servicio de directorio. Como el DN de la Base, del usuario administrador, clave, etc.

## 2.3 Abrir los puertos del cortafuegos

Podemos abrir los puertos "ldap" y "ldaps" por el cortafuegos de Yast, o usar los comandos:

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-3ldap.png)

## 2.4 Comprobamos el servicio

* `systemctl status dirsrv@localhost`, comprobar si el servicio está en ejecución.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-4ldap.png)

* `nmap -Pn serverXX | grep -P '389|636'`, para comprobar que el servidor LDAP es accesible desde la red. En caso contrario, comprobar cortafuegos.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-4-1ldap.png)

## 2.5 Comprobamos el acceso al contenido del LDAP

* `ldapsearch -b "dc=ldapXX,dc=curso2021" -x | grep dn`, muestra el contenido de nuestra base de datos LDAP. "dn" significa nombre distiguido, es un identificador que tiene cada nodo dentro del árbol LDAP.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-5ldap.png)

* `ldapsearch -H ldap://localhost -b "dc=ldapXX,dc=curso2021" -W -D "cn=Directory Manager" | grep dn`, en este caso hacemos la consulta usando usuario/clave.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/2-5-1ldap.png)

# 3. Usuarios LDAP

> Enlaces de interés: [Consultas a directorios LDAP utilizando ldapsearch](https://www.linuxito.com/gnu-linux/nivel-alto/1023-consultas-a-directorios-ldap-utilizando-ldapsearch)

## 3.1 Buscar Unidades Organizativas

Deberían estar creadas las OU People y Groups, es caso contrario hay que crearlas (Consultar ANEXO). Ejemplo para buscar las OU:

```
ldapsearch -H ldap://localhost:389 \
           -W -D "cn=Directory Manager" \
           -b "dc=ldapXX,dc=curso2021" "(ou=*)" | grep dn
```

> * Importante: No olvidar especificar la base (-b). De lo contrario probablemente no haya resultados en la búsqueda.
> * `"(ou=*)"` es un filtro de búsqueda de todas las unidades organizativas.
> * `"(uid=*)"` es un filtro de búsqueda de todos los usuarios.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/3-1ldap.png)

## 3.2 Agregar usuarios

> Enlaces de interés: VÍDEO Teoría [Los ficheros LDIF](http://www.youtube.com/watch?v=ccFT94M-c4Y)

Uno de los usos más frecuentes para el directorio LDAP es para la administración de usuarios. Vamos a utilizar ficheros **ldif** para agregar usuarios.

* Fichero `mazinger-add.ldif` con la información para crear el usuario `mazinger` (Cambiar el valor de dn por el nuestro):

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/3-2ldap.png)

> WARNING: Los valores de cada parámetro no deben tener espacios extra al final de la línea, porque provoca un error de sintáxis.

* `ldapadd -x -W -D "cn=Directory Manager" -f mazinger-add.ldif`, para escribir los datos del fichero **ldif** anterior dentro de LDAP.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/3-2-1ldap.png)

## 3.3 Comprobar el nuevo usuario

Estamos usando la clase `posixAccount`, para almacenar usuarios dentro de un directorio LDAP. Dicha clase posee el atributo `uid`. Por tanto, para listar los usuarios de un directorio, podemos filtrar por `"(uid=*)"`.

* `ldapsearch -W -D "cn=Directory Manager" -b "dc=ldapXX,dc=curso2021" "(uid=mazinger)"`, para comprobar si se ha creado el usuario correctamente en el LDAP.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/3-3ldap.png)

> INFO: Para **eliminar usuario del árbol del directorio** hacemos lo siguiente:
> * Crear un archivo `mazinger-delete.ldif`:
>
> ```
> dn: uid=mazinger,ou=people,dc=ldapXX,dc=curso2021
> changetype: delete
> ```
>

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/3-3-1ldap.png)

> * Ejecutamos el siguiente comando para eliminar un usuario del árbol LDAP: `ldapmodify -x -D "cn=Directory Manager" -W -f mazinger-delete.ldif`

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/3-3-2ldap.png)

# 4. Agregar más usuarios

## Contraseñas pwdhash
> Vamos a agregar al LDAP los usuarios de la tabla, pero en este caso vamos usar
> la herramienta **pwdhash** para generar las claves encriptadas dentro de los ficheros "ldif".

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/4ldap.png)

## 4.1 Agregar los siguientes usuarios

| Full name       | Account(uid) | uidNumber | Clave encriptada SHA  |
| --------------- | ------------ | --------- | --------------------- |
| Koji Kabuto     | koji         | 2002      | Contraseña encriptada |
| Boss            | boss         | 2003      | Contraseña encriptada |
| Doctor Infierno | drinfierno   | 2004      | Contraseña encriptada |

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/4-1ldap.png)

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/4-2ldap.png)

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/4-3ldap.png)

## 4.2 Comprobar desde el cliente

* Ir a la MV cliente LDAP.
* `nmap -Pn IP-LDAP-SERVER`, comprobar que el puerto LDAP del servidor está abierto.
Si no aparecen los puertos abiertos, entonces revisar el cortafuegos.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/4-4ldap.png)

* `ldapsearch -H ldap://IP-LDAP-SERVER -W -D "cn=Directory Manager" -b "dc=ldapXX,dc=curso2021" "(uid=*)" | grep dn` para consultar los usuarios LDAP que tenemos en el servicio de directorio remoto.

![ldapserver](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p1/images/4-5ldap.png)
