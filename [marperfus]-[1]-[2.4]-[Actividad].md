# Instalación y configuración de un servicio de directorio LDAP

## Instalación del servidor OpenLDAP

El objetivo de esta actividad es instalar y configurar un servicio de directorio utilizando **OpenLDAP**, que es una de las soluciones de software libre más utilizadas para la gestión de directorios en sistemas Linux.

Para ello, se ha utilizado un sistema **Ubuntu Server**, sobre el cual se realizará toda la instalación y configuración necesaria.

Cabe aclarar que antes de empezar, es recomendable actualizar nuestro servidor si no se ha actualizado recientemente, con los comandos:
```
sudo apt update
```
Y con el comando:
```
sudo apt upgrade
```

### Instalación de los paquetes necesarios

En primer lugar, se instalan los paquetes proporcionados por el proyecto OpenLDAP. Para ello, se ejecuta el siguiente comando en el terminal:

```
sudo apt install slapd ldap-utils
```
<img width="628" height="139" alt="imagen" src="https://github.com/user-attachments/assets/36b892a1-5f03-46c3-a2e6-95bf90a80eed" />

Durante el proceso de instalación, el sistema solicita la contraseña del administrador LDAP, la cual se utilizará posteriormente para gestionar el directorio.

Una vez finalizada la instalación, es necesario realizar su configuración inicial.

## Configuración básica del servidor LDAP

Para configurar el servidor OpenLDAP se utiliza el asistente de configuración incluido en el paquete slapd. Este asistente permite definir los parámetros básicos del directorio.

El comando utilizado es el siguiente:
```
sudo dpkg-reconfigure slapd
```

Durante el proceso de configuración, el asistente va solicitando diferentes datos que se introducen de la siguiente forma:
Nos pedira si queremos omitir y le daremos a: No

<img width="712" height="178" alt="imagen" src="https://github.com/user-attachments/assets/05820b3e-d8dd-4bc2-bdeb-93a9c42fc3a4" />

-Nombre del dominio DNS: prova-ioc.cat

<img width="1270" height="203" alt="imagen" src="https://github.com/user-attachments/assets/bc561c21-a70b-4b92-ae7d-6c66622695e6" />

-Nombre de la organización: prova-ioc

<img width="737" height="180" alt="imagen" src="https://github.com/user-attachments/assets/5b70f00e-c5a9-4bb6-ae48-4c3ac23c1f0d" />

-Contraseña del administrador: servidor

<img width="692" height="182" alt="imagen" src="https://github.com/user-attachments/assets/8c45e24d-54e9-4c35-a707-aac126342a3c" />

-Eliminar la base de datos cuando se purgue el paquete: No

<img width="629" height="170" alt="imagen" src="https://github.com/user-attachments/assets/f1be3101-d936-4f82-9adc-3537906ecb76" />

-Mover la base de datos antigua: Yes

<img width="1246" height="189" alt="imagen" src="https://github.com/user-attachments/assets/7198ccb2-f026-4378-8f6c-1dfd1c83f1b4" />


Una vez finalizado el asistente, el servidor LDAP queda correctamente configurado y listo para su uso, con una base de datos asociada al dominio prova-ioc.cat.

## Gestión del servicio OpenLDAP

#### Crear el archivo grups.ldif

Crear el archivo:

```
nano grups.ldif
```

Contenido:

```
dn: ou=alumnat,dc=IOC-domini,dc=cat
objectClass: organizationalUnit
objectClass: top
ou: alumnat

dn: cn=smx,ou=alumnat,dc=IOC-domini,dc=cat
objectClass: posixGroup
objectClass: top
cn: smx
gidNumber: 1001
```
<img width="785" height="190" alt="imagen" src="https://github.com/user-attachments/assets/dd66915f-bafc-442a-b7db-0dd43289bb12" />

#### Importar el archivo en el directorio

```
sudo ldapadd -x -D cn=admin,dc=IOC-domini,dc=cat -W -f grups.ldif
```
<img width="548" height="799" alt="imagen" src="https://github.com/user-attachments/assets/3473b5cd-73e5-466c-a173-90008e657292" />

#### Crear el archivo usuaris.ldif

```
nano usuaris.ldif
```

Contenido:
```
dn: cn=Queralt Serra,cn=smx,ou=alumnat,dc=IOC-domini,dc=cat
cn: Queralt Serra
sn: Serra
uid: queralt
uidNumber: 1100
gidNumber: 1001
homeDirectory: /home/queralt
loginShell: /bin/bash
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: posixAccount
objectClass: top
```

<img width="786" height="237" alt="imagen" src="https://github.com/user-attachments/assets/5cde9cdc-17a0-4a52-a95f-695885dee43b" />

#### Importar el usuario

```
sudo ldapadd -x -D cn=admin,dc=IOC-domini,dc=cat -W -f usuaris.ldif
```
<img width="583" height="787" alt="imagen" src="https://github.com/user-attachments/assets/0374f7e9-eab6-46ff-8c8d-2b5efe0fe012" />

#### Establecer contraseña con ldappasswd

El archivo LDIF no incluye contraseña. Se añadirá posteriormente:
```
sudo ldappasswd -S -W -D "cn=admin,dc=IOC-domini,dc=cat" -x "cn=Queralt Serra,cn=smx,ou=alumnat,dc=IOC-domini,dc=cat"
```
<img width="1283" height="67" alt="imagen" src="https://github.com/user-attachments/assets/58a8b47a-bf3b-4908-9d88-058b257d617a" />

(He de aclarar que no me ha dejado cambiarla por un tema de credenciales)

### Mostrar todo el árbol LDAP

```
sudo ldapsearch -x -LLL ldap:/// -b dc=IOC-domini,dc=cat dn
```

### Mostrar la unidad organizativa

```
sudo ldapsearch -x -H ldap:/// -b ou=alumnat,dc=IOC-domini,dc=cat dn
```
<img width="793" height="264" alt="imagen" src="https://github.com/user-attachments/assets/65821dd3-66de-466a-9e3c-d2afc7760926" />

### Mostrar un usuario concreto

```
sudo ldapsearch -x -H ldap:/// -b "cn=Queralt Serra,cn=smx,ou=alumnat,dc=IOC-domini,dc=cat"
```

Además de la línea de comandos, el directorio puede administrarse mediante herramientas gráficas.

Herramientas libres recomendadas:

- Apache Directory Studio
- LDAP Account Manager (LAM)
- phpLDAPadmin
- GOsa

Estas aplicaciones permiten:
- Navegar por el árbol LDAP
- Crear y modificar usuarios y grupos
- Administrar permisos
- Gestionar unidades organizativas
## Instalación de una aplicación gráfica de gestión del directorio

Para facilitar la administración del servicio de directorio LDAP, se instala una aplicación gráfica llamada phpLDAPadmin, que permite gestionar el directorio desde una interfaz web de forma sencilla.

Instalación de phpLDAPadmin

La instalación se realiza ejecutando el siguiente comando:
```
sudo apt install phpldapadmin
```
<img width="658" height="112" alt="imagen" src="https://github.com/user-attachments/assets/3484967e-3555-4861-8c28-01cff2265127" />

Este paquete instala tanto la aplicación como las dependencias necesarias para su funcionamiento a través de un navegador web.

## Configuración de phpLDAPadmin

Una vez instalada la aplicación, es necesario realizar una pequeña configuración para adaptarla al dominio LDAP creado anteriormente.

El archivo de configuración que se debe modificar es el siguiente:

```
/usr/share/phpldapadmin/config/config.php
```

Para editarlo, utilizaremos el editor nano:

```
sudo nano /usr/share/phpldapadmin/config/config.php
```

Dentro del archivo, se configura el nombre del dominio LDAP (prova-ioc.cat) y los parámetros necesarios para que la aplicación se conecte correctamente al servidor OpenLDAP.

Una vez realizados los cambios, se guarda el archivo y se cierra el editor.

<img width="1279" height="781" alt="imagen" src="https://github.com/user-attachments/assets/7ab29229-997b-406e-a66a-7c5bcc255156" />

## Acceso y conexión al servidor LDAP
Primero tenemos que comprobar si el servicio apache2 funciona, yo no lo habia instalado por lo que puse sudo apt update, y luego sudo apt install apache2, para comprobar que funcione lo haremos con el comando:
```
sudo systemctl status apache2
```
<img width="834" height="373" alt="imagen" src="https://github.com/user-attachments/assets/5d890485-a246-491f-9ae9-5a3bcf2dc5e7" />

Después de la configuración que hicimos antes, ya es posible acceder a phpLDAPadmin desde cualquier navegador web.

Para ello, introducimos en la barra de direcciones la dirección IP del servidor Ubuntu, por ejemplo:

```
http://IP_DEL_SERVIDOR/phpldapadmin
```
<img width="1201" height="771" alt="imagen" src="https://github.com/user-attachments/assets/b65db6ef-b9f8-4dcd-85db-c04c5de6406e" />


Al acceder, la aplicación permite conectarse al servidor LDAP utilizando las credenciales del administrador configuradas anteriormente.

Desde esta interfaz es posible gestionar usuarios, grupos y demás objetos del directorio de forma gráfica y centralizada.

# Conclusión

Con esta práctica se ha realizado la instalación y configuración básica de un servicio de directorio OpenLDAP, así como la instalación de una herramienta gráfica para su gestión.

El uso de phpLDAPadmin facilita notablemente la administración del directorio, permitiendo gestionar los datos de forma visual y remota, lo que resulta especialmente útil en entornos de red y administración de sistemas
