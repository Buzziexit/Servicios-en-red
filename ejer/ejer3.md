### Paso 1: Crear el Directorio para Páginas de Error
Crea un nuevo directorio llamado error dentro de la raíz de tu directorio de documentos web. Puedes hacerlo con el siguiente comando:

```
sudo mkdir /srv/www/iesrodrigocaro/error
```

### Paso 2: Crear Páginas de Error Personalizadas
Dentro del directorio error, crea dos archivos HTML, uno para el error 404 y otro para el error 403. Puedes usar un editor de texto o un comando como nano o vim:

Archivo 404.html:

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Error 404 - No Encontrado</title>
</head>
<body>
    <h1>Error 404 - No Encontrado</h1>
    <p>Lo siento, la página que buscas no existe.</p>
</body>
</html>
```

Archivo 403.html:

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Error 403 - Prohibido</title>
</head>
<body>
    <h1>Error 403 - Prohibido</h1>
    <p>No tienes permisos para acceder a esta página.</p>
</body>
</html>
```

Guarda ambos archivos dentro de /srv/www/iesrodrigocaro/error.

### Paso 3: Configurar Nginx para Utilizar Páginas de Error Personalizadas
Ahora, ajusta tu configuración de Nginx para utilizar estas páginas de error. Agrega o modifica las siguientes líneas en tu archivo de configuración de Nginx:

```
server {
    listen 80;
    server_name www.iesrodrigocaro.org;

    root /srv/www/iesrodrigocaro;
    index index.html;

    error_page 404 /error/404.html;
    error_page 403 /error/403.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # Resto de tu configuración...
}
```

### Paso 4: Guardar y Reiniciar Nginx
Guarda los cambios en tu archivo de configuración y reinicia Nginx:

```
sudo systemctl restart nginx
```

Con estos pasos, Nginx ahora debería mostrar las páginas HTML personalizadas que has creado cuando se encuentre un error 404 (objeto no encontrado) o un error 403 (no permitido) en tu sitio web. Puedes personalizar aún más el contenido de estas páginas según tus necesidades.


![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/45deb2ea-0fe4-4ee2-a62f-997a1262f04a)

