# Configuración de GoAccess con actualizaciones periódicas en Linux

## Requisitos Previos

- Máquina con sistema operativo Linux instalado.
- Servidor web Apache2 configurado y en funcionamiento.
- GoAccess instalado en el sistema

## Pasos:

### 1. Instalación de GoAccess

Asegúrate de tener GoAccess instalado en tu sistema. Puedes instalarlo usando el gestor de paquetes de tu distribución. Si estás en Ubuntu, puedes usar los siguientes comandos:

```
sudo apt update
sudo apt install goaccess
```

### 2. Preparación de Logs

Asegúrate de que los registros de acceso (access logs) de tu servidor Apache estén configurados y accesibles en una ubicación específica. Puedes verificar la configuración del acceso a los logs de Apache ejecutando:

```
cat /etc/apache2/sites-available/000-default.conf | grep CustomLog
```

### 3. Configuración del Script

Crea un nuevo script en tu directorio personal con el siguiente comando:

```
#!/bin/bash

goaccess -f /var/log/apache2/access.log -o /var/www/html/informe.html --log-format=COMBINED

```
Luego, dale permisos de ejecución al script:

```
chmod 777 nombre_script.sh
```
### 4. Programación de tareas con cron

```
crontab -e
```
Agrega la siguiente línea al final del archivo crontab:

```
* * * * * /ruta/del/script/nombre_script.sh
```

### 5. Verificación

```
crontab -l
```

### Hay otra opción desde la linea de comandos:

```
nohup goaccess -o /var/www/html/stats/index.html --log-format=COMBINED --real-time-html < /var/log/apache2/access.log 2> %1
```
