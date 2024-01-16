### Paso 1: Instalar Nginx
Asegúrate de tener Nginx instalado. Puedes instalarlo utilizando el siguiente comando:

```
sudo apt-get update
sudo apt-get install nginx
```

### Paso 2: Crear los Directorios y Páginas HTML

```
sudo mkdir -p /srv/www/iesrodrigocaro
sudo mkdir -p /srv/www/departamentos

echo "Bienvenida a la página del instituto." | sudo tee /srv/www/iesrodrigocaro/index.html
echo "Bienvenida a la página de los departamentos del instituto." | sudo tee /srv/www/departamentos/index.html
```

### Paso 3: Configurar Sitios en Nginx
Edita el archivo de configuración de Nginx:

```
sudo nano /etc/nginx/sites-available/default
```

Reemplaza el contenido con la siguiente configuración:


```
server {
    listen 80;
    server_name www.iesrodrigocaro.org;

    root /srv/www/iesrodrigocaro;
    index index.html;

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

### Paso 4: Crear Enlaces Simbólicos y Reiniciar Nginx

```
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

### Paso 5: Configurar Resolución de Dominios (Opcional)

Edita el archivo /etc/hosts para apuntar los dominios a tu dirección IP local. Añade las siguientes líneas:

```
127.0.0.1 www.iesrodrigocaro.org
127.0.0.1 departamentos.iesrodrigocaro.org
```

### Comprobacion:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/bf2562b6-c35d-44a5-998f-e50620cdad29)

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/33dc37c1-f043-4803-b237-d58f5e3fa7be)


