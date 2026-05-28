# Instalación y configuración de un servicio de directorio LDAP

## Introducción

El objetivo de esta actividad es instalar y configurar un servicio de directorio utilizando OpenLDAP, una de las soluciones de software libre más utilizadas para la gestión de directorios en sistemas Linux.

Para ello, se ha utilizado un sistema Ubuntu Server, sobre el cual se realizará toda la instalación y configuración necesaria.

---

# Instalación del servidor OpenLDAP

Antes de comenzar, es recomendable actualizar el servidor:

```bash
sudo apt update
sudo apt upgrade
```

## Instalación de los paquetes necesarios

Instalamos los paquetes proporcionados por OpenLDAP:

```bash
sudo apt install slapd ldap-utils


```

Durante la instalación, el sistema solicitará la contraseña del administrador LDAP.

---

# Configuración básica del servidor LDAP

Ejecutamos el asistente de configuración:

```bash
sudo dpkg-reconfigure slapd
```

Durante la configuración se introducen los siguientes datos:

* Omitir configuración OpenLDAP: **No**
* Nombre del dominio DNS: `IOC-domini.cat`
* Nombre de la organización: `IOC-domini`
* Contraseña del administrador: `servidor`
* Eliminar la base de datos al purgar el paquete: **No**
* Mover la base de datos antigua: **Yes**

Una vez finalizado el asistente, el servidor LDAP queda correctamente configurado.

---

# Gestión del servicio OpenLDAP

## Crear grupos LDAP

Creamos el archivo:

```bash
nano grups.ldif
```

Contenido del archivo:

```ldif
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

Importamos el archivo al directorio:

```bash
sudo ldapadd -x -D cn=admin,dc=IOC-domini,dc=cat -W -f grups.ldif
```

---

# Crear usuarios LDAP

Creamos el archivo:

```bash
nano usuaris.ldif
```

Contenido:

```ldif
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

Importamos el usuario:

```bash
sudo ldapadd -x -D cn=admin,dc=IOC-domini,dc=cat -W -f usuaris.ldif
```

---

# Establecer contraseña LDAP

El archivo LDIF no incluye contraseña, por lo que se añade posteriormente:

```bash
sudo ldappasswd -S -W -D "cn=admin,dc=IOC-domini,dc=cat" -x "cn=Queralt Serra,cn=smx,ou=alumnat,dc=IOC-domini,dc=cat"
```

---

# Consultas LDAP

## Mostrar todo el árbol LDAP

```bash
sudo ldapsearch -x -LLL ldap:/// -b dc=IOC-domini,dc=cat dn
```

## Mostrar una unidad organizativa

```bash
sudo ldapsearch -x -H ldap:/// -b ou=alumnat,dc=IOC-domini,dc=cat dn
```

## Mostrar un usuario concreto

```bash
sudo ldapsearch -x -H ldap:/// -b "cn=Queralt Serra,cn=smx,ou=alumnat,dc=IOC-domini,dc=cat"
```

---

# Herramientas gráficas para LDAP

Además de la línea de comandos, LDAP puede administrarse mediante herramientas gráficas como:

* Apache Directory Studio
* LDAP Account Manager (LAM)
* phpLDAPadmin
* GOsa

Estas herramientas permiten:

* Navegar por el árbol LDAP
* Crear y modificar usuarios y grupos
* Administrar permisos
* Gestionar unidades organizativas

---

# Instalación de phpLDAPadmin

Instalamos la herramienta gráfica:

```bash
sudo apt install phpldapadmin
```

---

# Configuración de phpLDAPadmin

El archivo de configuración se encuentra en:

```bash
/usr/share/phpldapadmin/config/config.php
```

Lo editamos con:

```bash
sudo nano /usr/share/phpldapadmin/config/config.php
```

Dentro del archivo se configuran los parámetros del dominio LDAP.

---

# Instalación y comprobación de Apache2

Instalamos Apache2:

```bash
sudo apt install apache2
```

Comprobamos el estado del servicio:

```bash
sudo systemctl status apache2
```

---

# Acceso a phpLDAPadmin

Una vez configurado todo, se puede acceder desde el navegador:

```text
http://192.168.13.100/phpldapadmin
```

Desde esta interfaz es posible gestionar usuarios, grupos y demás objetos LDAP de forma gráfica.

---

# Conclusión

Con esta práctica se ha realizado la instalación y configuración básica de un servicio de directorio OpenLDAP, así como la instalación de una herramienta gráfica para su administración.

El uso de phpLDAPadmin facilita notablemente la gestión del directorio, permitiendo administrar los datos de forma visual y remota.
