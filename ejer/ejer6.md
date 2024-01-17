### Paso 1: Crear el Archivo .htpasswd
Primero, necesitas crear el archivo .htpasswd que contendrá los usuarios y las contraseñas. Puedes hacerlo utilizando la herramienta htpasswd que a menudo está disponible en sistemas Linux.

Ejecuta el siguiente comando para crear el archivo .htpasswd en el directorio /etc/nginx/:

```
sudo htpasswd -c /etc/nginx/.htpasswd usuario1
```

Te pedirá que ingreses una contraseña para el usuario especificado (usuario1). Puedes agregar más usuarios sin el flag -c si ya tienes el archivo .htpasswd creado.

### Paso 2: Modificar la Configuración de Nginx
Edita el archivo de configuración de Nginx correspondiente al sitio en el que deseas habilitar la autenticación básica:

```
sudo nano /etc/nginx/sites-available/tu_sitio
```

Agrega o modifica la sección para incluir las directivas auth_basic y auth_basic_user_file:

```
server {
    listen 80;
    server_name tu_sitio;

    root /ruta/a/tu/sitio;
    index index.html;

    location / {
        try_files $uri $uri/ =404;

        # Configuración para la autenticación básica
        auth_basic "Área Restringida";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
}
```

Reemplaza tu_sitio con el nombre de tu sitio y ajusta la ruta a tu sitio en la directiva root.

### Paso 3: Guardar y Reiniciar Nginx
Guarda los cambios y reinicia Nginx para aplicar la configuración:

```
sudo systemctl restart nginx
```

### Paso 4: Verificar la Autenticación
Ahora, cuando intentes acceder a tu sitio, se te solicitará un nombre de usuario y una contraseña. Ingresa las credenciales que configuraste en el archivo .htpasswd para acceder al sitio.

Recuerda que es importante proteger el archivo .htpasswd para evitar accesos no autorizados.

Este proceso establece una autenticación básica en tu sitio utilizando un archivo .htpasswd. Asegúrate de adaptar los nombres de usuario, las contraseñas y las rutas según tus necesidades.

Comprobación:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/29787972-59c8-446e-8bfb-68b36dac8249)

