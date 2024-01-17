### Paso 1: Actualizar Configuración de Nginx

Edita el archivo de configuración de Nginx:

```
sudo nano /etc/nginx/sites-available/default
```
Agrega una nueva sección location dentro del servidor que maneja el dominio www.iesrodrigocaro.org. Asegúrate de que la configuración se vea así:

```
server {
    listen 80;
    server_name www.iesrodrigocaro.org;

    root /srv/www/iesrodrigocaro;
    index index.html;

    location /principal {
        rewrite ^/$ /principal/ permanent;
        index index.html;
    }

    location /principal/ {
        autoindex off;
        try_files $uri $uri/ =404;
    }

    location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen 80;
    server_name departamentos.iesrodrigocaro.org;

    root /srv/www/departamentos;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### Paso 2: Aplicar los Cambios y Reiniciar Nginx

Guarda los cambios en el archivo y cierra el editor. Luego, crea el enlace simbólico y reinicia Nginx:

```
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

### Paso 3: Acceder a la Redirección

Después de realizar estos pasos, al acceder a http://www.iesrodrigocaro.org, se redirigirá automáticamente a http://www.iesrodrigocaro.org/principal, donde se mostrará el mensaje de bienvenida. Además, la restricción de listado de archivos y seguimiento de enlaces simbólicos se aplicará en el directorio /principal/.

### Comprobación:


![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/768fff93-cce2-418b-9d00-c56fc4950897)





