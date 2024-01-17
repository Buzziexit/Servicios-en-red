### Paso 1: Obtener tu Dirección IP
Antes de comenzar, asegúrate de conocer tu dirección IP pública. Puedes obtenerla ejecutando el siguiente comando en tu máquina local:

```
curl ifconfig.me
```

### Paso 2: Modificar la Configuración de Nginx
Edita el archivo de configuración de Nginx para el sitio departamentos.iesrodrigocaro.org:

bash
Copy code
sudo nano /etc/nginx/sites-available/default
Asegúrate de que la sección correspondiente a departamentos.iesrodrigocaro.org se vea algo así:

nginx
Copy code
server {
    listen 80;
    server_name departamentos.iesrodrigocaro.org;

    root /srv/www/departamentos;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
        # Añadir la siguiente línea para limitar el acceso por IP
        allow tu_direccion_ip;
        deny all;
    }
}
Reemplaza tu_direccion_ip con tu dirección IP pública.

Paso 3: Guardar y Reiniciar Nginx
Guarda los cambios y reinicia Nginx para aplicar la nueva configuración:

bash
Copy code
sudo systemctl restart nginx
Paso 4: Verificar el Acceso Restringido
Intenta acceder al sitio departamentos.iesrodrigocaro.org desde tu máquina y desde otra máquina con una dirección IP diferente. Deberías poder acceder solo desde la máquina con la dirección IP que especificaste en la configuración.

Esto limitará el acceso al sitio departamentos.iesrodrigocaro.org solo a la dirección IP especificada en la directiva allow. Ten en cuenta que las direcciones IP públicas pueden cambiar, así que asegúrate de ajustar la configuración si tu dirección IP cambia en el futuro.

Comprobación: 

Máquina permitida:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/2ae07beb-a52e-46ce-9275-14e6b966a7ff)

Máquina no permitida:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/1fdc43d5-1ed9-48e7-8513-3a0faf9e7c0c)

