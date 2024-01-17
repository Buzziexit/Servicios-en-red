### Paso 1: Editar la Configuración de Nginx

Abre el archivo de configuración de Nginx para realizar los cambios:

```
sudo nano /etc/nginx/sites-available/default
```

### Paso 2: Agregar una nueva sección location

Agrega una nueva sección location para el alias que permitirá el acceso a /principal/documentos y configurará las restricciones especificadas:

```
server {
    listen 80;
    server_name www.iesrodrigocaro.org;

    root /srv/www/iesrodrigocaro;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location = /principal {
        rewrite ^ http://$host/principal/ permanent;
    }

    location /principal/ {
        alias /srv/www/iesrodrigocaro/;
        index index.html;
        autoindex off;
    }

    location /principal/documentos {
        alias /srv/doc;
        autoindex on;

        if (-e $request_filename) {
            break;
        }

        if ($request_filename ~ /principal/documentos/(.*)$) {
            set $document_path $1;
        }

        if (!-e $request_filename) {
            rewrite ^ /principal/documentos/ last;
        }
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

### Paso 3: Guardar y Reiniciar Nginx
Guarda los cambios en el archivo y reinicia Nginx:

```
sudo systemctl restart nginx
```

### Paso 4: Probar la Configuración
Ahora puedes acceder a http://www.iesrodrigocaro.org/principal/documentos para ver el contenido de /srv/doc. Asegúrate de que las restricciones mencionadas (listado de archivos y seguimiento de enlaces simbólicos solo si el dueño es el mismo) se apliquen correctamente. Si encuentras algún problema, revisa los logs de Nginx para obtener detalles:

```
sudo tail -f /var/log/nginx/error.log
```
