# proFTPd

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/5ee75c74-0695-44f8-8865-8aa143cc2cea)

```ProFTPD Version 1.3.8```

El servidor proFTPd es una implementación altamente configurable y segura del protocolo FTP (File Transfer Protocol). Ofrece una amplia gama de características, incluyendo soporte para SSL/TLS, autenticación avanzada, y opciones de configuración flexibles. Este repositorio proporciona una configuración básica y ejemplos para ayudarte a implementar proFTPd en tu entorno.


## Instalación:

Sigue estos pasos para instalar proFTPd en tu sistema:

```
apt update
apt upgrade
sudo apt install proftpd
service proftpd start
systemctl enable proftpd
```
## Archivos Configuración:

| Nombre de archivo                | Función                                                                                                                                       |
|----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| ~/.ftpaccess                     | Proporciona un mecanismo de control adicional para cada host virtual. El archivo tiene que ubicarse en el directorio root del host virtual. Consulte [aquí](http://www.castaglia.org/proftpd/doc/devel-guide/internals/ftpaccess.html) para obtener más información. |
| /etc/proftpd.conf                | Incluye la mayoría de los parámetros de configuración que se deben definir para que el servicio ProFTPD funcione.                                |
| /etc/shutmsg                     | Incluye la información utilizada por el comando ftpshut.                                                                                       |
| /etc/ftpd/ftpusers               | Muestra los usuarios a los que se va a quitar los privilegios de inicio de sesión FTP. Proporcionado para la compatibilidad con el servicio wu-ftpd.|
| /var/log/xferlog                 | Muestra la información de registro de ProFTPD.                                                                                                 |
| /var/run/proftpd.scoreboard      | Incluye información de seguimiento para cada sesión actual, utilizada por los comandos ftpcount, ftptop y ftpwho. Consulte [aquí](http://www.proftpd.org/docs/howto/Scoreboard.html) para obtener más información.  |

## Configuración:

Fichero para la configuración ```/etc/proftpd/proftpd.conf```

```
nano /etc/proftpd/proftpd.conf
```

Buscaremos la línea comentada «DefaultRoot» y la descomentamos borrando la almohadilla #. Esto nos va a permitir que cuando cada usuario acceda a su cuenta del FTP, estos accederán directamente a su carpeta «home».

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/2a18fe33-0f45-45b3-90e4-8ccff94d986c)

Si queremos que todos los usuarios que inicien sesión accedan por defecto a una misma carpeta, debemos cambiar el parámetro DefaultRoot y añadir la ruta a la que queramos que accedan. 

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/e3056544-0656-4ef1-a148-82476d4187ef)


```
sudo /etc/init.d/proftpd restart
```

## Crear usuarios y contraseñas para el FTP

Para gestionar el contenido de manera sencilla y rápida en nuestro servidor FTP proFTPd, podemos crear usuarios en cualquier momento. Esta flexibilidad nos permite tener tantos usuarios como necesitemos. A continuación, mostraremos cómo crear un usuario con un nombre, contraseña, directorio y cuota de almacenamiento opcional.

```
sudo adduser usuario
```

Dentro del asistente, siempre nos va a indicar que pongamos la contraseña del nuevo usuario, no obstante, podríamos cambiar esta contraseña en cualquier momento poniendo al siguiente orden:

```
sudo passwd usuario
```
A partir de ahora, «usuario» podrá conectarse al FTP y accederá, por defecto, a la carpeta especificada en DefaultRoot

## Utilizar
Al servidor ftp se puede acceder a través de un simple navegador, o utilizando un cliente ftp, como Filezilla:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/e330114c-c10b-4fdc-bbac-82a1f42994dc)

