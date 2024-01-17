# Configuración ftp

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/51dc1747-fff2-48b4-8348-b3d726d7a8c3)


Creado en AWS con Debian 12.

Archivo de configuración: /etc/vsftpd/vsftpd.conf



```
apt update
apt upgrade
sudo apt-get install vsftpd
systemctl enable vsftpd.service
service vsftpd start
systemctl enable vsftpd
useradd -g ftp -d /var/www/html/carpetaFTP usuarioftp
chown usuarioftp:ftp /var/www/html/carpetaFTP -R
passwd usuarioftp
```

Para entrar en el servidor: 

```
ftp IP_PUBLICA
```


