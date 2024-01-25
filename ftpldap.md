# Configuracion proFTPd con LDAP

Para utilizar la autenticación de usuarios mediante LDAP en ProFTPD, primero, asegúrate de que el paquete proftpd-mod-ldap está instalado. En Debian Lenny, al instalar el paquete proftpd, este módulo se instala automáticamente como una dependencia.

Luego, necesitarás activar el módulo LDAP en la configuración de ProFTPD. Sigue estos pasos:

1. Abre el archivo /etc/proftpd/modules.conf en un editor de texto. Puedes usar nano u otro editor de tu elección:

```
sudo nano /etc/proftpd/modules.conf
```

2. Busca la línea que se refiere al módulo LDAP y la descomentala.

```# LoadModule mod_ldap.c```

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/2d612eea-5dd1-4227-8167-839a2a6b3dd7)

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/3d335c6d-6bd0-4a70-9758-1e77b93a215c)


3. Editamos el fichero ```/etc/proftpd/proftpd.conf``` , buscamos la línea:

```
#Include /etc/proftpd/ldap.conf
```
y también la descomentamos

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/962e7a11-d5e2-49b6-b855-28124088f51b)

4. Editamos el fichero /etc/proftpd/ldap.conf, en el que dejamos las tres líneas siguientes:

```
LDAPServer ldap://localhost/??sub LDAPDNInfo "cn=admin,dc=cursolinux,dc=net" "asdasd" LDAPDoAuth on "ou=People,dc=cursolinux,dc=net"
```
Donde especificamos el servidor LDAP, el nombre distinguido del usuario que se conecta, su contraseña y la ubicación de la rama que contiene los usuarios.

5. Abre el archivo de configuración principal de ProFTPD en un editor de texto.

```
sudo nano /etc/proftpd/proftpd.conf
```

Para que puedan autenticarse usuarios con la shell (/bin/false) hay que descomentar la directiva:

```
#RequireValidShell off 
```

6. Después de hacer todas estas modificaciones reiniciamos el demonio con:

```
/etc/init.d/proftpd restart 
```
