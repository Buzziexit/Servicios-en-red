# proFTPd

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

## Configuración:

Fichero para la configuración ```/etc/proftpd/proftpd.conf```

```
nano /etc/proftpd/proftpd.conf
```

Buscaremos la línea comentada «DefaultRoot» y la descomentamos borrando la almohadilla #. Esto nos va a permitir que cuando cada usuario acceda a su cuenta del FTP, estos accederán directamente a su carpeta «home».

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/2a18fe33-0f45-45b3-90e4-8ccff94d986c)

Si queremos que todos los usuarios que inicien sesión accedan por defecto a una misma carpeta, debemos cambiar el parámetro DefaultRoot y añadir la ruta a la que queramos que accedan. 

```
sudo /etc/init.d/proftpd restart
```

## Crear usuarios y contraseñas para el FTP
