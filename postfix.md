# Instalación y configuración de Postfix

Este repositorio proporciona una guía paso a paso para instalar y configurar el servidor de correo Postfix en un sistema Debian. Se incluyen instrucciones detalladas para preparar el entorno, instalar el software necesario y configurar tanto Postfix como el cliente de correo Thunderbird.


### Preparación

Antes de instalar y configurar Postfix, es necesario realizar algunas preparaciones en el sistema.

#### 1. Establecer el hostname

Primero, asegúrate de que el archivo /etc/hostname contenga el nombre de host adecuado para tu servidor de correo. Por ejemplo, si quieres utilizar el nombre de host "superdominio.prueba", puedes editarlo con el siguiente comando:

```
nano /etc/hostname
```

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/2d3ec4a5-2979-476e-80fd-26da78ae1bfc)

#### 2. Modificar el archivo hosts

Se modificará el fichero /etc/hosts para que contenga:

```
nano /etc/hosts
```

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/d9290ec0-141b-4111-acbb-c05279a91d1f)


### Instalación del software:

```
apt update
apt install postfix dovecot-imapd mailutils thunderbird
```
Esto instalará Postfix como servidor de correo saliente, Dovecot como servidor de correo entrante (IMAP/POP3), el paquete mailutils que proporciona herramientas de correo adicionales, y el cliente de correo Thunderbird para probar la configuración.

## Configuracion Postfix


```
nano /etc/postfix/main.cf
```

En este archivo, encontrarás una gran cantidad de opciones de configuración para ajustar el comportamiento de Postfix. Asegúrate de configurar correctamente los parámetros necesarios según tus requerimientos. Por ejemplo, puedes especificar el nombre de dominio de correo, los dominios virtuales, los buzones de correo, entre otros.


![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/11aa3be3-b2dc-47eb-b67e-974e9a93c49f)



## Configuración de Thunderbird

Para probar la configuración del servidor de correo, crearemos un nuevo usuario en el sistema y configuraremos Thunderbird para acceder al servidor.



```
adduser manoli
```


![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/4754d42a-853f-4c4c-8d27-8ffe16f05894)


Comprobamos:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/1d7b88b1-0428-4caf-bb43-9d3754d34479)
