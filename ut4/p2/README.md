# Cliente para autenticación LDAP

Con autenticacion LDAP prentendemos usar la máquina servidor LDAP, como repositorio centralizado de la información de grupos, usuarios, claves, etc. Desde otras máquinas conseguiremos autenticarnos (entrar al sistema) con los usuarios definidos no en la máquina local, sino en la máquina remota con LDAP. Una especie de *Domain Controller*.

En esta actividad, vamos a configurar otra MV (GNU/Linux OpenSUSE) para que podamos hacer autenticación en ella, pero usando los usuarios y grupos definidos en el servidor de directorios LDAP de la MV1.

# 1. Preparativos

* Supondremos que tenemos una MV1 (serverXX) con DS-389 instalado, y con varios usuarios dentro del DS.
* Necesitamos MV2 con SO OpenSUSE ([Configuración MV](../../global/configuracion/opensuse.md))

Comprobamos el acceso al LDAP desde el cliente:
* Ir a MV cliente.
* `nmap -Pn IP-LDAP-SERVERXX | grep -P '389|636'`, para comprobar que el servidor LDAP es accesible desde la MV2 cliente. En caso contrario revisar que el cortafuegos está abierto en el servidor y que el servicio está activo.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/1ldap2.png)

* `ldapsearch -H ldap://IP-LDAP-SERVERXX:389 -W -D "cn=Directory Manager" -b "dc=ldapXX,dc=curso2021" "(uid=*)" | grep dn`, comprobamos que los usuarios del LDAP remoto son visibles en el cliente.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/1-1ldap2.png)

# 2. Configurar autenticación LDAP

> Enlaces de interés:
> * https://doc.opensuse.org/documentation/leap/archive/15.3/security/html/book-security/cha-security-ldap.html
> * [Configurar_servidor_de_autenticacion_usando_YaST](https://es.opensuse.org/Configurar_servidor_de_autenticacion_usando_YaST)
> * https://luiszambrana.com.ar/2020/12/09/gestion-de-usuarios-con-openldap/?s=09

## 2.1 Crear conexión con servidor

Vamos a configurar de la conexión del cliente con el servidor LDAP.

* Ir a la MV cliente.
* No aseguramos de tener bien el nombre del equipo y nombre de dominio (`/etc/hostname`, `/etc/hosts`)

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/2-1ldap2.png)

* Ir a `Yast -> LDAP y Kerberos`. En el caso de que no nos aparezca esta herramienta, la podemos instalar con el paquete `yast2-auth-client`.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/2-1-1ldap2.png)

* Configurar como la imagen de ejemplo:
    * BaseDN: `dc=ldapXX,dc=curso2021`
    * DN de usuario: `cn=Directory Manager`
    * Contraseña: CLAVE del usuario cn=Directory Manager

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/2-1-2ldap2.png)

* Pulsar el botón para `Probar conexión`.
* Aceptar.

## 2.2 Comprobar con comandos

Ir a la MV cliente:
* Vamos a la consola.
* `id mazinger`, consultar información del usuario.
* `getent passwd mazinger`, consultamos más datos del usuario
* `cat /etc/passwd | grep mazinger`, nos aseguramos que el usuario NO es local.
* `su -l mazinger`, entrar con el usuario definido en LDAP.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/2-2ldap2.png)

# 3. Crear usuarios usando otros comandos

> Para que funcionen bien los siguientes comandos el fichero /root/.dsrc
debe estar correctamente configurado.

Ir a la MV del servidor:
* `dsidm localhost user list`, consultar la lista de usuarios.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/3ldap2.png)

* Crear usuario robot1:

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/3-1ldap2.png)

* Poner la clave al usuario:

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/3-2ldap2.png)

* `dsidm localhost user list`, consultar la lista de usuarios.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/3-3ldap2.png)

Ir a la MV cliente:
* Abrir terminal con nuestro usuario normal (NO usar root).
* `su robot1`, entrar como ese usuario.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/3-4ldap2.png)

# 4. Usando Yast

## 4.1 Crear usuario LDAP usando Yast

En este punto vamos a escribir información dentro del servidor de directorios LDAP.
Este proceso se debe poder realizar tanto desde el Yast del servidor, como desde el Yast
del cliente.

* Ir a la MV cliente.
* `Yast -> Usuarios Grupos`.
* Set filter: `LDAP users`.

![ldap2](https://github.com/DAVIDQR22/add2223-david-quintero/blob/main/ut4/p2/images/4-1ldap2.png)

* Bind DN: `cn=Directory Manager,dc=ldapXX,dc=curso2021`.
* Crear el grupo `villanos` (Estos se crearán dentro de la `ou=groups`).
* Crear usuario `baron` (Se creará dentro de la `ou=people`).
* Incluir los usuarios `robot` y `drinfierno` en el grupo de `villanos`.

## 4.2 Comprobamos

* Ir a la MV cliente.
* `ldapsearch -H ldap://IP-LDAP-SERVER -W -D "cn=Directory Manager" -b "dc=ldapXX,dc=curso2021" "(uid=NOMBRE-DEL-USUARIO)"` comando para consultar en la base de datos LDAP la información del usuario con uid concreto.
* Iniciar sesión de entorno gráfico con algún usuario LDAP.

---
